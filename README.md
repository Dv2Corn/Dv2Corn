### Hi, I'm Aleksey 👋

QA / SDET focused on networking software: VPN and proxy panels, clients and network libraries. I find bugs, write reproducible bug reports and back every fix with a regression test that fails before the fix and passes after it.

**Accepted upstream bug fixes:** 59 in 28 open-source projects, among them sing-box's networking libraries (SagerNet/sing, sing-tun), 3x-ui, mihomo, v2rayN/v2rayNG, Hiddify, SoftEther VPN, sshuttle, dnscrypt-proxy, fail2ban, redis-py, go-redis, fasthttp and the Node.js ws library. [Full list →](CONTRIBUTIONS.md)

**Case studies** — bug → reproduction → root cause → test design → fix:

- [go-redis: a 0.5 ms timeout turned into "wait forever"](case-studies/go-redis-sub-millisecond-timeouts.md) — boundary values
- [fasthttp: a proxy URL ending in "/" broke every request](case-studies/fasthttp-proxy-trailing-slash.md) — input formats, regression
- [sshuttle: one unreachable DNS server killed the tunnel](case-studies/sshuttle-unreachable-nameserver.md) — fault injection
- [3x-ui: a partial API update silently disabled clients](case-studies/3x-ui-partial-update-data-loss.md) — API contract, data loss

**In-depth investigations** (in [still-ui-qa/case-studies](https://github.com/Dv2Corn/still-ui-qa/tree/main/case-studies)):

- [3x-ui: a partial update silently wiped a VPN key](https://github.com/Dv2Corn/still-ui-qa/blob/main/case-studies/01-3x-ui-update-wipes-awg-psk.md) — test data classes, read-after-write checks
- [Xray ≥ 26.9 silently rejects common clients](https://github.com/Dv2Corn/still-ui-qa/blob/main/case-studies/02-xray-26-9-rejects-fp-ios.md) — compatibility matrix as a release gate
- [sing-tun: UDP replies echo congestion marks](https://github.com/Dv2Corn/still-ui-qa/blob/main/case-studies/03-sing-tun-udp-replies-echo-ecn.md) — protocol compliance, RFC 3168
- [72-hour memory soak against upstream](https://github.com/Dv2Corn/still-ui-qa/blob/main/case-studies/04-72h-panel-memory-soak.md) — soak test with a control group

**QA artifacts:** [still-ui-qa](https://github.com/Dv2Corn/still-ui-qa) — test plan, bug reports and an API + UI test suite (pytest, requests, Playwright) running in GitHub Actions against a real release of still-ui.

**How I test**
- Reproduce first, in a clean container, with the exact error.
- Test design: boundary values, equivalence classes, negative cases, state transitions, fault injection.
- Every fix ships with a regression test, and I run the project's full suite, not only the new test.
- Bug reports and PRs say plainly what was not tested.

**Tools:** Python (pytest, requests, Playwright), Go testing, Docker, GitHub Actions, Linux networking, Git.

**Own projects**
- [still-ui](https://github.com/Dv2Corn/still-ui) — a maintained fork of the 3x-ui panel; releases built and attested by GitHub Actions.
- [xui-doctor](https://github.com/Dv2Corn/xui-doctor) — a read-only health checker for 3x-ui and still-ui panels.
