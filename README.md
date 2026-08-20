# Pentest Lab: Footprinting & Network Scanning — Week 2

![Status](https://img.shields.io/badge/status-completed-brightgreen)
![Category](https://img.shields.io/badge/category-Cybersecurity-blue)
![Program](https://img.shields.io/badge/program-Networkwalks%20Internship-orange)

A hands-on cybersecurity lab documenting **footprinting/reconnaissance** and **network scanning** activities performed during Week 2 of a Cybersecurity & Ethical Hacking internship at Networkwalks. This repository captures the tools, commands, findings, and risk analysis produced while footprinting a live domain, running OSINT against a large public target, and scanning a local LAN.

## ⚠️ Liability Disclaimer

All activities in this repository were performed **only** on systems and domains for which explicit written permission was secured, or on infrastructure owned by the author (local LAN). All material here is for **education and research purposes only**. Do not use anything in this repository to break the law.

The instructor, the authors, and Networkwalks are not responsible for how this knowledge is used. Every action taken with this material is the sole responsibility of the user. Misuse can lead to criminal charges, heavy fines, loss of employment, and a permanent criminal record. In most countries, unauthorized access to computer systems is a crime even when no damage occurs.

## 📋 Project Overview

| Field | Details |
|---|---|
| Pentester | Olawale Sogbesan (Cybersecurity Professional) |
| Program / Batch | B082 – Networkwalks |
| Modules completed | W2-PM1 (Multiple Kali Tools), W2-PM4 (theHarvester), W2-PM5 (Zenmap Scanning) |
| Targets | networkwalks.com (written permission secured) · microsoft.com (public OSINT) · Local LAN (own network) |
| Phases covered | Phase 1: Reconnaissance & Footprinting · Phase 2: Scanning & Network Discovery |
| Phases pending | Phase 3–5 (in progress) |

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux & Windows | Operating systems used for reconnaissance activities |
| WHOIS | Find domain registration details (owner, dates, name servers) |
| WhatWeb | Fingerprint web technologies (server, CMS, plugins, IP) |
| nslookup | Resolve the domain name to its IP address using DNS |
| curl -I | Read the HTTP response headers of the website |
| wafw00f | Detect whether a Web Application Firewall protects the site |
| dnsrecon | Enumerate all DNS records (NS, MX, SPF, TXT, SRV) |
| Zenmap (Nmap GUI) | Scan the local subnet to find live hosts, IPs, and MAC addresses |
| theHarvester | Gather emails, sub-domains, hosts, employee names, open ports, and banners from public sources |
| Windows CMD | Local IP and MAC address identification |

## 🔍 Activities Performed

### 1. Footprinting & Reconnaissance (networkwalks.com)

Reconnaissance was performed against the `networkwalks.com` domain using six Kali Linux tools, each collecting a different category of information:

- **WHOIS** — obtained publicly available domain registration information and the domain's name servers, revealing insight into registration and hosting infrastructure.
- **WhatWeb** — identified the technology stack: **WordPress 7.0.4** and **WP Download Manager 3.3.58**, along with other exposed metadata.
- **nslookup** — resolved the domain to IP address `192.232.216.135`.
- **curl -I** — inspected HTTP response headers and revealed the exposed WordPress REST API endpoint `/wp-json/`.
- **wafw00f** — detected a Web Application Firewall: **ModSecurity (SpiderLabs)**.
- **dnsrecon** — enumerated DNS records including name servers, mail servers, SPF/TXT records, service records, and DNS software information.

### 2. OSINT with theHarvester (microsoft.com)

Passive footprinting was performed against `microsoft.com` using theHarvester to gather emails, sub-domains, hosts, employee names, open ports, and banners from public sources such as search engines, PGP key servers, and the SHODAN database.

### 3. Network Scanning with Zenmap (Local LAN)

Network discovery was performed on the local LAN:

1. Used Windows `ipconfig` to identify the local IP address and subnet.
2. Entered the subnet into Zenmap and ran a **Ping Scan** to identify active hosts.
3. Identified live hosts (example results): `192.168.4.1` and `192.168.18.5`, plus four MAC addresses.
4. Opened the Topology section in Zenmap, enabled the legend, and exported the network topology as PDF.

## 📊 Key Findings & Risk Summary

The full risk register with evidence, impact, and severity ratings is documented in [`docs/risk-analysis.md`](docs/risk-analysis.md). Headline findings:

- **Critical** — Massive subdomain exposure on microsoft.com: 9,484 hosts enumerated, including internal-looking names (`redmond.corp.microsoft.com`) and pre-production environments.
- **Critical** — Multiple staging/dev/test-labeled hosts discovered (`-ppe`, `-dev`, `-uat`, `-test`, `-sdf`, `-int` patterns), which are typically less hardened than production.
- **Medium** — Web technology (WordPress + plugin versions) and DNS infrastructure details exposed on networkwalks.com.
- **Medium** — 122 public IP addresses identified across Azure, Akamai, Cloudflare, and Microsoft-owned ranges.
- **Low** — Server IP, HTTP headers, WAF identity, and 3 employee email addresses exposed.

> These are observations from information-gathering and host-discovery exercises, **not confirmed vulnerabilities**. No exploitation or vulnerability validation was performed. Further authorized testing would be required to confirm actual exploitability.

## ✅ Recommendations

1. Regularly review publicly exposed web technology, CMS, and plugin information.
2. Keep CMS platforms and plugins updated against current security advisories.
3. Review HTTP response headers to avoid leaking unnecessary technical details.
4. Periodically audit DNS records to ensure only required information is public.
5. Keep the WAF (ModSecurity) properly configured, tuned, and monitored.
6. Perform regular internal network discovery scans.
7. Investigate any unknown or unexpected devices found during scanning.
8. Maintain up-to-date network topology and device documentation.
9. Always perform reconnaissance and scanning within an authorized scope.

## 🧠 Conclusion

This lab demonstrated how an attacker moves from gathering public information (footprinting) to mapping live hosts on a network (scanning). Reconnaissance tools like WHOIS, WhatWeb, nslookup, curl, wafw00f, and dnsrecon each reveal a different layer of an organization's exposed footprint, while Zenmap illustrates how quickly an internal network's topology and live hosts can be mapped. The exercise reinforced that even without exploitation, careful analysis of public information and network responses can reveal significant risk — and that all such testing must remain strictly within an authorized scope.
