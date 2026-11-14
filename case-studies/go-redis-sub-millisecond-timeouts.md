# go-redis: a 0.5 ms timeout turned into "wait forever"

**Project:** [redis/go-redis](https://github.com/redis/go-redis) · **PR:** [#4292](https://github.com/redis/go-redis/pull/4292) (merged) · **Type:** boundary value bug

## Symptom

`XRead` with `Block: 500 * time.Microsecond` should return `redis.Nil` after about a millisecond. Instead it hung for 10 seconds and failed with `i/o timeout`. `XReadGroup`, `Wait` and `WaitAOF` behaved the same.

The realistic trigger is a block time computed from a deadline (`Block: time.Until(deadline)`): in the last millisecond before the deadline, the call turns into "wait forever".

## Reproduction

```go
_, err := rdb.XRead(ctx, &redis.XReadArgs{
	Streams: []string{"stream", "$"},
	Block:   500 * time.Microsecond,
}).Result()
// after 10 s: read tcp ...: i/o timeout   (expected: redis.Nil after ~1 ms)
```

## Root cause

The command builders converted the duration with integer division: `500µs / 1ms = 0`. For Redis, `BLOCK 0` and `WAIT ... 0` mean "no timeout". The rest of the client already had a helper, `formatMs`, that rounds a positive sub-millisecond value up to 1 ms. Six call sites did not use it.

## Test design

Boundary values around the unit of conversion:

| Input | Expected argument | Why |
|---|---|---|
| `500µs` | `1` | positive value below one unit — the bug |
| `0` | `0` | explicit "block forever" must stay as is |
| `2500µs` | `2` | whole-millisecond values unchanged (truncation is existing behaviour) |

- **Unit test** `TestSubMillisecondDurationArgs` (no server): checks the argument list of all six call sites. Before the fix, 6 of 8 subtests failed.
- **Integration specs** against a real Redis 8.10: before the fix both failed with `i/o timeout` after 10.0 s; after the fix both pass.
- **Regression run:** the full suite on standalone, cluster, TLS cluster, sentinel and ring setups — 1615 specs passed.

## Fix

The six call sites now use the existing `formatMs` helper. I searched the repository for every other place that divides by `time.Millisecond` or `time.Second` in a command builder; there were no more.

## Not covered

Durations above 1 ms are still truncated (2.5 ms is sent as 2). This is how the whole client works, so I left it unchanged.
