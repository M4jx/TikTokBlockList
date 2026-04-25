# TikTokBlockList

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Last Updated](https://img.shields.io/github/last-commit/M4jx/TikTokBlockList?label=updated)](https://github.com/M4jx/TikTokBlockList/commits/main)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

Network-wide blocklists for the TikTok mobile app: domains, IPv4/IPv6 CIDRs, and ASNs.

Drop these lists into pfSense, OPNsense, Pi-hole, AdGuard Home, or any blocklist-aware firewall to cut TikTok off your network.

Domains are derived from live mobile-traffic captures of the TikTok app (packet inspection & API analysis), so the list reflects what the app actually contacts.

## Table of Contents

- [Good to Know](#good-to-know)
- [Quick Setup](#quick-setup)
- [Contributing](#contributing)
- [License](#license)

## Good to know

TikTok can bypass simple DNS blocking by resolving its domains through DoH (DNS-over-HTTPS) and DoT (DNS-over-TLS). This means it may still obtain IP addresses for domains listed in a hosts blocklist. To ensure effective blocking, you should also include an IP-based blocklist in the `ipv4s` and `ipv6s` files to prevent connections to those resolved addresses.

## Quick Setup

Point your firewall / resolver at the raw URLs below. Each list goes into a different feature: DNS for `hosts`, IP blocklist for `ipv4s` and `ipv6s`, ASN blocklist for `asns`.

| List          | Raw URL                                                             | Where It Goes                                                          |
| ------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Domains       | `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/hosts` | DNS blocklist (Pi-hole adlist, pfBlockerNG DNSBL, AdGuard Home filter) |
| IPv4 CIDRs    | `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/ipv4s` | Firewall IPv4 alias / IP blocklist                                     |
| IPv6 prefixes | `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/ipv6s` | Firewall IPv6 alias / IP blocklist                                     |
| ASNs          | `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/asns`  | Firewall ASN blocklist (one entry per ASN)                             |

You must at least use `hosts` and `ipv4s` and `ipv6s` lists for optimal blocking.

#### Optional

In most cases, using the `hosts`, `ipv4s`, and `ipv6s` blocklists is sufficient to block the app.
However, if you want to prevent TikTok from resolving its domains via DoT and DoH, you should also:

1. Block Outbound DNS traffic (port 53).
2. Block Outbound DoT traffic (port 853)
3. Block DoH by blocking known DoH provider domains with an additional blocklist

## Contributing

Pull requests welcome. Useful additions:

- New domains observed via packet capture
- New ByteDance/TikTok ASNs or CIDR ranges
- False-positive reports (a domain that shouldn't be on the list)

## License

[MIT](LICENSE)
