# sshuttle: one unreachable DNS server killed the whole tunnel

**Project:** [sshuttle/sshuttle](https://github.com/sshuttle/sshuttle) · **PR:** [#1318](https://github.com/sshuttle/sshuttle/pull/1318) (merged) · **Type:** error handling / resilience

## Symptom

With `--dns`, the sshuttle server crashed — and the tunnel with it — as soon as a DNS request went to a nameserver the remote host could not reach. A typical case: an IPv6 nameserver in the remote's `/etc/resolv.conf` on a host without IPv6.

```
OSError: [Errno 101] Network is unreachable
c : fatal: ssh connection to server (pid 9) exited with returncode 1
```

## Reproduction

Run the server in a Linux container with no network, so the route really is missing. Point `--to-ns` at an unroutable address (`192.0.2.53`) and send one DNS query through the tunnel. The server crashes, and none of 5 TCP connections opened afterwards gets through.

## Root cause

The code already retried a failed `send()` up to three times with another nameserver. But for a UDP socket, a missing route is reported by `connect()`, and `connect()` was called *before* the `try` block. So that error was never caught and ended the server.

## Test design

Fault injection with mocked sockets whose `connect()` raises `ENETUNREACH`:

1. The first nameserver is unreachable, the second works → the request goes to the second one, and both sockets are tracked so they get closed later.
2. The nameserver stays unreachable → exactly 3 attempts, no crash, the request is dropped.

Both tests fail on the old code with the `OSError` and pass with the fix.

- **End to end:** the real client loop and a real server process in a container without network. Before the fix: the traceback above, and no connections relayed. After: three debug log lines, and all 5 connections relayed. A nameserver on loopback was used as a control: the query and its answer pass through.
- **Regression run:** `pytest` (612 passed) and `flake8`.

## Fix

Move `connect()` inside the existing `try` block — a one-line move. An unreachable nameserver is now handled like a failed `send()`.

## Not covered

When every try fails, the DNS client gets no answer and has to time out. That is the existing behaviour for a failed `send()` too.
