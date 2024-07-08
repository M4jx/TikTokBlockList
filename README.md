# TikTokBlockList

This collection of domain names, IP addresses and ASNs has been gathered from the TikTok mobile application. It is primarily intended for use in pfSense, OPNsense, Pi-hole, etc., to effectively block TikTok application network-wide.

The data was compiled by analyzing the TikTok application's traffic on mobile devices, performing packet inspections, and examining the TikTok API.

## Important Note ❗

TikTok uses DNS over HTTPS (DoH) and DNS over TLS (DoT) to resolve its domains if the primary DNS resolver fails. Therefore, simply using a DNS blacklist is insufficient. You must also block all outbound traffic on port 53 TCP/UDP to prevent the application from using alternative DNS servers. Additionally, ensure you block DoH and DoT.

## Usage

### pfSense

1. Install the `pfBlockerNG` package [tutorial](https://www.youtube.com/watch?v=oNo77CMoxUM).
2. Add DNS Block List

   Navigate to `Firewall > pfBlockerNG> DNSBL > DNSBL Groups`

   Click `Add`

   Name and Description: `TikTok`

   State: `ON`

   Source: `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/hosts`

   Header/Label: `TikTok`

   Save

3. Add IPv4 Block List

   Navigate to `Firewall > pfBlockerNG> IP > IPv4`

   Click `Add`

   Name and Description: `TikTok`

   State: `ON`

   Source: `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/ipv4s`

   Header/Label: `TikTok`

   Action: `Deny Both`

   Update Frequency: `Once a day`

   Save

4. Add IPv6 Block List

   Navigate to `Firewall > pfBlockerNG> IP > IPv6`

   Click `Add`

   Name and Description: `TikTok`

   State: `ON`

   Source: `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/ipv6s`

   Header/Label: `TikTok`

   Action: `Deny Both`

   Update Frequency: `Once a day`

   Save

5. Add ASN Block List

   Navigate to `Firewall > pfBlockerNG> IP > IPv4`

   Click `Add`

   Name and Description: `TikTok_ASN`

   State: `ON`

   Source: `https://raw.githubusercontent.com/M4jx/TikTokBlockList/main/asns`

   Header/Label: `TikTok_ASN`

   Action: `Deny Both`

   Update Frequency: `Once a week`

   Save

6. Reload rules

   Navigate to `Firewall > pfBlockerNG > Update`

   Select 'Force' option: `Reload`

   Select 'Reload' option: `All`

7. [Block Outbound DNS](#block-outbound-dns)
8. [Block DoH/DoT](#block-dohdot)

## Block Outbound DNS

You need to run a local DNS resolver/server to perform DNS queries. If you block outbound DNS without a local DNS resolver, you won't be able to resolve any domains. If you're using pfBlockerNG for DNS blocking or piHole, you are likely already using a local DNS server on your box.

### pfSense

1. Navigate to `Firewall > Rules > Floating`.
2. Add new rule.

   Action: `Block`

   Quick: ✅

   Interface: `WAN`

   Direction: `Out`

   Address Family: `IPv4`

   Protocol: T`CP/UDP`

   Destination Port Range: `DNS (53)`

   Description: `Block Outbound DNS`

## Block DoH/DoT

### pfSense

1. Navigate to `Firewall > pfBlockerNG > DNSBL > DNSBL SafeSearch.`
2. Enable `DoH/DoT/DoQ Blocking`.
3. Select all entries in `DoH/DoT/DoQ Blocking List`
4. Click Save

## Contribution

Feel free to create a pull request to add more data to the list.
