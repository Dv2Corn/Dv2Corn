### Hi, I'm Aleksey 👋

I hunt bugs in open-source VPN and proxy software —
panels, clients and network cores. Every report comes with a reproduction and a test that fails before the fix.

**Accepted upstream fixes**

| Project | Fix |
|---|---|
| [SagerNet/sing-tun](https://github.com/SagerNet/sing-tun) | [#106](https://github.com/SagerNet/sing-tun/pull/106) Fix IP headers of synthesized UDP replies (stale ECN marks, RFC 3168) · [#107](https://github.com/SagerNet/sing-tun/pull/107) Do not route replies of peer-initiated UDP flows into the TUN |
| [SagerNet/sing](https://github.com/SagerNet/sing) | [#119](https://github.com/SagerNet/sing/pull/119) Fix a race between Read and Close in cached connections · [#118](https://github.com/SagerNet/sing/pull/118) Cancel the winning attempt's context in parallel dial |
| [2dust/v2rayN](https://github.com/2dust/v2rayN) | [#10548](https://github.com/2dust/v2rayN/pull/10548) Keep the WebSocket path query when removing `ed` (sing-box) |
| [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) | [#7042](https://github.com/MHSanaei/3x-ui/pull/7042) A partial client update no longer wipes fields it does not mention |
| [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo) | [#3415](https://github.com/MetaCubeX/mihomo/pull/3415) Convert HTTPUpgrade share links correctly |
| [MetaCubeX/metacubexd](https://github.com/MetaCubeX/metacubexd) | [#2246](https://github.com/MetaCubeX/metacubexd/pull/2246) Keep config edits on remote profiles across refresh · [#2247](https://github.com/MetaCubeX/metacubexd/pull/2247) Offer only TUN stacks mihomo accepts |
| [hiddify/hiddify-app](https://github.com/hiddify/hiddify-app) | [#2465](https://github.com/hiddify/hiddify-app/pull/2465) Accept a base64 profile title without padding |
| [hiddify/hiddifypanel](https://github.com/hiddify/hiddifypanel) | [#176](https://github.com/hiddify/hiddifypanel/pull/176) Turning off HTTP no longer drops Shadowsocks 2022 and Mieru · [#178](https://github.com/hiddify/hiddifypanel/pull/178) Renew no longer exceeds max active users |

**Own projects**

- [still-ui](https://github.com/Dv2Corn/still-ui) — a maintained fork of the 3x-ui panel: releases built and attested by GitHub Actions.
- [xui-doctor](https://github.com/Dv2Corn/xui-doctor) — a read-only health checker for 3x-ui and still-ui panels.

**How I work:** reproduce first, prove it with a test that fails before the fix, keep the change small, say plainly what was not tested.
