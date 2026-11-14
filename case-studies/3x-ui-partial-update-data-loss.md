# 3x-ui: renewing a subscription through the API silently disabled the customer

**Project:** [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) · **PR:** [#7042](https://github.com/MHSanaei/3x-ui/pull/7042) (merged) · **Type:** API contract / data loss

## Symptom

`POST /panel/api/clients/update/:email` with a body that only sets a new `expiryTime` did extend the expiry date, but it also:

- switched the client off (`enable=false`);
- reset the traffic and device limits, the Telegram id, the comment, the group and the renewal settings to zero.

A shop bot that renews subscriptions this way would quietly turn off paying customers and remove their limits.

## Reproduction

On v3.11.0:

1. Create a client with limits, a Telegram id, a comment and a group.
2. Send an update with only `{"email": "...", "expiryTime": ...}`.
3. Read the client back: the expiry date changed, and `enable` and every listed field are now zero.

## Root cause

The handler decoded the body into a full client struct. Any field missing from the JSON got its zero value and was written over the stored one, so "not sent" and "set to zero" looked the same.

## Test design

The main question was **what must not change**. Cases:

| Case | Expected |
|---|---|
| body with only `expiryTime` | expiry updated, every other field kept |
| explicit `0`, `""`, `false`, `null` | the field is cleared (callers that send the full object behave as before) |
| key in a different case (`limitIP`) | treated as `limitIp`, the way Go's JSON decoder binds keys |
| `flow` omitted, client in several inbounds | each inbound keeps its own flow |
| `reverse` omitted / `reverse: null` | kept / cleared |
| WireGuard keys, credentials | kept, as before (pinned with a test) |

- **Service-level tests** in `client_partial_update_test.go` cover each row.
- **HTTP-level tests** go through the real handler: `TestUpdateHandlerKeepsFieldsTheBodyOmits`.
- **Frontend test:** the panel's own editor sends `null` for a cleared reverse tag.
- **Regression run:** the whole Go suite (53 packages, with `-race` on the touched packages); on the frontend, `vitest` (1950 tests), lint, typecheck and build.

## Fix

A field missing from the body is now taken from the stored record; an explicit empty value still clears it. The API reference in four languages and the OpenAPI file describe the new behaviour.

## Not covered

The Storybook browser tests were not run. A caller that relied on an omitted field being reset now has to send an explicit empty value; this is documented in the PR as the only behaviour change.
