# Streaming VPN bypass for GL.iNet

A starter domain list that routes selected streaming destinations through your normal internet connection. These destinations will see your ISP/public WAN address. This does not change your location to another country.

Download or subscribe to `streaming-bypass.txt`. It contains 45 unique domains, one per line, with no comments, wildcards, URLs, or hosts-file prefixes. Root domains cover their subdomains. See `SERVICE-DOMAINS.md` for the service mapping.

## Coverage and status

Scope: only selected services with documented VPN restrictions: **Hulu, Disney+, HBO Max, Prime Video, and Netflix**. This is a primarily US-oriented starter list, not an exhaustive worldwide catalog. Inclusion means the provider documents VPN restrictions or VPN-related playback failures; it does not mean every VPN is blocked on every device.

| Service | Reason included |
| --- | --- |
| Hulu | [Anonymous proxy error](https://help.hulu.com/article/hulu-anonymous-proxy-error): Hulu requires disabling VPNs to access video. |
| Disney+ | [Error 73](https://help.disneyplus.com/article/disneyplus-error-73): VPN/IP anonymizer use can trigger a location error. |
| HBO Max | [VPN detected message](https://help.hbomax.com/gr-en/Answer/Detail/000002566): support says to turn off VPN/proxy services to stream. |
| Prime Video | [Playback troubleshooting](https://www.primevideo.com/help?nodeId=GU85HKX66NVFNQ9Y): support instructs disabling VPN/proxy servers for playback failures. |
| Netflix | [VPN restrictions](https://help.netflix.com/en/node/114701/): live events and ad-supported viewing do not support VPNs; other viewing may be limited to globally licensed content. This is a conditional restriction, not a blanket VPN ban. |

YouTube/YouTube TV, Apple TV+, ESPN, Tubi, Pluto TV, Peacock, and Paramount+ are not included in this version. Omission is not a claim that they always work over VPN; adequate provider evidence was not established for this release. Sign-in, ads, DRM, and regional playback may require additional domains.

Syntax and duplicate checks passed. User-reported playback test: Prime Video works with this list on a GL-BE9300 running firmware 4.9.0. This confirms playback in that setup, not an independent routing audit. Other services remain untested. The list is a manually curated snapshot, not an automatically maintained feed. Providers change domains over time.

Broad shared infrastructure domains such as amazonaws.com, cloudfront.net, googleapis.com, apple.com, and amazon.com are deliberately omitted. Selected provider domains may still serve other products. Domain routing ultimately uses destination IP addresses, so services sharing IPs can be affected too. This is not precise application-level isolation.

## Host on GitHub

1. Create a public repository, for example `streaming-vpn-bypass`.
2. Upload the files in this folder to the repository root and commit them.
3. Open `streaming-bypass.txt`, select **Raw**, and copy that URL.

Subscription URL for this repository:

```text
https://raw.githubusercontent.com/lolfym/streaming-vpn-bypass/main/streaming-bypass.txt
```

The raw URL should load only domains without requiring a login. Do not use the GitHub `/blob/` page URL.

## Configure GL.iNet

Target router: GL-BE9300, firmware 4.9.0. Online text imports are supported from firmware 4.7.

1. Open **VPN → VPN Dashboard** and edit your VPN tunnel's routing rules.
2. Select **All Clients** and destination **Exclude specified Domain / IP List**.
3. In the list input, select **Subscription URL** if offered and paste the public raw text URL. If your 4.9.0 screen offers only manual entry, paste the contents of `streaming-bypass.txt` instead; this requires manual updates.
4. Apply. Under the dashboard's **All Other Traffic** settings, choose **Allow Non-VPN Traffic** so excluded streaming traffic can use WAN. **Enhanced Kill Switch** blocks that unmatched traffic.
5. Keep the individual tunnel's **Kill Switch** enabled if traffic assigned to the VPN should stop when the tunnel disconnects. Review other tunnel rules so they do not capture the streaming destinations.

The official 4.9 guide confirms exclusion rules and these kill-switch controls. Its written instructions describe manual entry; subscription controls should be checked on the actual 4.9.0 interface. The 4.8 guide explicitly documents URL subscriptions and daily refreshes. Updating the GitHub file changes what subscribing routers fetch; it does not automatically discover new streaming domains.

## Verify and troubleshoot

Restart streaming apps after applying the policy so they establish new connections. Test sign-in, actual playback, seeking, ads, and live channels where applicable. Check router connection/routing information to confirm streaming connections use WAN while unrelated traffic uses the VPN. A browser IP check alone does not prove the streaming app bypasses the VPN.

If playback still fails, inspect DNS requests from the affected device. Domain routing depends on the router learning relevant DNS answers; device-level encrypted DNS and cached answers can interfere. Test using the router as the device's DNS resolver and reconnect the device. Check IPv6 routing separately if enabled. Add only the specific missing domains you observe, then retest.

For a dedicated streaming device, bypassing that device can be more reliable, but sends all of its traffic outside the VPN.

## Sources and contributions

Domain candidates were checked against the following community-maintained rule sources on November 17, 2026. These sources classify services; they are not provider guarantees of complete playback requirements. Only a selected subset is included, and source-specific rule prefixes, regular expressions, broad shared CDNs, and most marketing domains were omitted.

- [v2fly domain-list-community](https://github.com/v2fly/domain-list-community/tree/master/data): `netflix`, `disney`, `hulu`, `bamtech`, and `hbo`.
- [LM-Firefly streaming rules](https://github.com/LM-Firefly/Rules/blob/master/PROXY.list): Prime Video selection.
- [GL.iNet online text file format](https://docs.gl-inet.com/router/en/4/tutorials/how_to_configure_domain_and_ip_filtering_rules_for_glinet_routers_via_an_online_text_file/).
- [GL.iNet firmware 4.9 VPN policy](https://docs.gl-inet.com/router/en/4/interface_guide/vpn_dashboard/).
- [GL.iNet firmware 4.8 VPN policy](https://docs.gl-inet.com/router/en/4/interface_guide/vpn_dashboard_v4.8/).

For additions, include provider documentation of VPN blocking or a reproducible VPN-specific error. Also report the service, country, router model, firmware, failing playback action, and specific observed domain. Avoid sharing account tokens or full authenticated request URLs. Keep the text file sorted, unique, and one domain per line. Record tested fixes in the service mapping.
