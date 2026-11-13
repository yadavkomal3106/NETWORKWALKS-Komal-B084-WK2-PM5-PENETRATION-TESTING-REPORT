
# Week 2 – Penetration Testing: Footprinting, OSINT & Network Scanning

**Cybersecurity & Ethical Hacking Internship | Networkwalks**

---

## 1. Project Overview

This repository documents the practical work completed during **Week 2** of my Cybersecurity and Ethical Hacking internship at Networkwalks.

The primary objective of this week's activities was to understand how cybersecurity professionals gather information about a target before conducting deeper security assessments. The work focused on three key areas:

1. **Footprinting and Reconnaissance:** Collecting publicly available information about a domain, including registration details, IP addresses, web technologies, HTTP headers, WAF information, and DNS records.
2. **Passive OSINT:** Using theHarvester to explore publicly available information associated with a domain through external data sources.
3. **Network Scanning and Discovery:** Using Zenmap (Nmap's graphical interface) to identify active hosts on my own local network and generate a network topology.

These exercises helped me understand how information gathering and network discovery contribute to the initial stages of a penetration testing engagement.

## 2. Objectives

The main objectives of this week's practical activities were:

- Understand the importance of reconnaissance in penetration testing.
- Learn how to collect domain registration and DNS information.
- Identify web technologies and services exposed by a website.
- Examine HTTP response headers and understand the information they reveal.
- Understand the purpose of Web Application Firewall (WAF) detection.
- Explore publicly available information using passive OSINT techniques.
- Identify local network configuration, including IP address, subnet mask, and default gateway.
- Discover active devices on a local network using Zenmap.
- Understand how network topology helps visualize connected devices.
- Document observations, potential security implications, and recommendations.

## 3. Scope and Authorization

All activities were performed for educational purposes within the following scope:

| Target | Activity | Scope |
|---|---|---|
| `networkwalks.com` | Footprinting and reconnaissance | Written permission secured |
| `microsoft.com` | Passive OSINT using theHarvester | Public sources only |
| My own local network | Network discovery using Zenmap | Personally owned/authorized network |

**Important:** The theHarvester activity was limited to passive information gathering through public sources. It did not involve scanning or directly testing Microsoft's servers.

No exploitation, unauthorized access, or vulnerability validation was performed during these exercises.

## 4. Tools and Technologies Used

The following tools were used during the practical activities:

| Tool | Purpose |
|---|---|
| Kali Linux | Operating system used for reconnaissance and security tools |
| WHOIS | Retrieves publicly available domain registration information |
| WhatWeb | Identifies website technologies, web servers, CMS platforms, and related components |
| Nslookup | Resolves domain names into IP addresses using DNS |
| cURL | Inspects HTTP response headers and server responses |
| Wafw00f | Identifies the presence and type of Web Application Firewall |
| DNSRecon | Enumerates DNS records such as A, MX, NS, SOA, TXT, and SRV |
| theHarvester | Collects publicly available OSINT information from supported sources |
| Zenmap | Graphical interface for Nmap used for network discovery and visualization |
| Bash/Linux commands | Used to inspect local network configuration and routing information |
| Windows | Host operating system used during the local network exercise |

## 5. Activities Performed

### 5.1 Footprinting and Reconnaissance

The first activity involved performing footprinting and reconnaissance against `networkwalks.com` using six Kali Linux tools.

The purpose was to understand what information about a website and its infrastructure could be collected through publicly accessible services.

**A. WHOIS Lookup**

Command:

```bash
whois networkwalks.com
```

Purpose:
- Retrieve domain registration information.
- Identify the domain registrar.
- Check domain creation, update, and expiration dates.
- Identify the registered name servers.
- Review domain status and DNSSEC information.

Observation:

The WHOIS lookup returned registration details, including GoDaddy as the registrar, HostGator name servers, domain status information, and an unsigned DNSSEC status.

**B. Website Technology Fingerprinting – WhatWeb**

Command:

```bash
whatweb networkwalks.com
```

Purpose:
- Identify the technologies used by a website.
- Detect the web server and CMS.
- Identify frameworks, plugins, and other visible components.
- Observe redirects and server-related information.

Observation:

WhatWeb identified Apache as the web server and detected WordPress, WP Download Manager, jQuery, and Bootstrap. It also returned information about the website's public-facing infrastructure.

**C. DNS Lookup – Nslookup**

Command:

```bash
nslookup networkwalks.com
```

Purpose:
- Resolve a domain name into its corresponding IP address.
- Understand how DNS translates domain names into network addresses.
- Verify the IP information returned by other reconnaissance tools.

Observation:

The DNS lookup resolved the domain to an IP address. The result was consistent with the address identified during the WhatWeb scan.

**D. HTTP Header Inspection – cURL**

Command:

```bash
curl -I https://networkwalks.com
```

Purpose:
- Retrieve HTTP response headers without downloading the complete webpage.
- Identify the server response status.
- Examine server and caching information.
- Observe exposed technical details and security-related headers.

Observation:

The server returned an HTTP 200 OK response. The headers revealed Apache server information, a WordPress REST API link, and caching-related details.

The inspection also showed that certain security headers, including HSTS, X-Frame-Options, and Content-Security-Policy, were not observed in the response examined.

**E. Web Application Firewall Detection – Wafw00f**

Command:

```bash
wafw00f networkwalks.com
```

Purpose:
- Check whether a Web Application Firewall is protecting a website.
- Identify the WAF technology when detectable.
- Understand the role of WAFs in filtering suspicious web traffic.

Observation:

Wafw00f identified ModSecurity (SpiderLabs) as the WAF technology associated with the website.

**F. DNS Enumeration – DNSRecon**

Command:

```bash
dnsrecon -d networkwalks.com
```

Purpose:
- Enumerate publicly available DNS records.
- Identify name servers and mail servers.
- Retrieve A, MX, NS, SOA, TXT, and SRV records.
- Examine DNS-related infrastructure information.

Observation:

DNSRecon returned multiple DNS records, including name server, mail server, A, TXT, and SRV records. It also exposed BIND version information and reported no answer to its DNSSEC query.

### 5.2 Passive OSINT Using theHarvester

The second activity focused on Open-Source Intelligence (OSINT) using **theHarvester version 4.11.1**.

The tool was used against `microsoft.com` as a demonstration target to understand how publicly available information can be collected from external sources.

The activity was passive: it queried supported search engines and public data sources rather than scanning the target's own servers.

**First Run – Baidu Source**

Command:

```bash
theHarvester -d microsoft.com -l 1000 -b baidu
```

Purpose:
- Search the selected public source for information associated with the domain.
- Understand how theHarvester collects information from search engines.

Observation:

The scan completed but did not return IP addresses, email addresses, people, or hosts in this run.

**Second Run – All Supported Sources**

Command:

```bash
theHarvester -d microsoft.com -l 50 -b all
```

Purpose:
- Query all supported sources available in the tool.
- Understand how different OSINT providers contribute to information gathering.

Observation:

Several sources displayed missing API key messages because the required API credentials were not configured in the tool's API key configuration file:

```text
/etc/theHarvester/api-keys.yaml
```

This exercise helped me understand that some OSINT integrations require API keys and that the results depend on the availability and configuration of individual data sources.

### 5.3 Network Scanning and Discovery Using Zenmap

The third activity involved identifying active devices on my own local network using **Zenmap**, the graphical interface of Nmap.

The practical focused on understanding local network configuration, discovering live hosts, identifying IP and MAC addresses, and generating a network topology.

**Step 1 – Identifying Network Configuration**

Commands:

```bash
ip -4 addr
ip route
```

Purpose:
- Identify the active IPv4 address.
- Check the subnet mask.
- Identify the default gateway.
- Determine the local network range.

Observed configuration:

| Network Detail | Value |
|---|---|
| Active IPv4 Address | `10.0.0.2` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `10.0.0.1` |
| Local Network Range | `10.0.0.0/24` |

**Step 2 – Performing a Ping Scan**

The identified subnet was entered as the target in Zenmap, and the Ping Scan profile was selected.

Equivalent Nmap command:

```bash
nmap -sn 10.0.0.0/24
```

Purpose:
- Discover live hosts on the local subnet.
- Identify active IP addresses.
- Collect available MAC address information.
- Understand which devices are responding to network discovery.

Observation:

The scan checked 256 IP addresses and completed in approximately 2.99 seconds.

The detailed scan results in the report identified two live hosts:

- `10.0.0.1` – Default gateway.
- `10.0.0.2` – My own Windows PC.

Nmap displayed the MAC address manufacturer as unknown for the identified devices.

**Step 3 – Network Topology Generation**

After completing the scan, I opened the Topology tab in Zenmap, enabled the legend, and saved the network topology in PDF format.

Purpose:
- Visualize the discovered network hosts.
- Understand the relationship between discovered devices.
- Practice documenting network discovery results.

The generated topology and Zenmap screenshots are included in the evidence section.

## 6. Key Observations and Potential Security Implications

The exercises produced several observations that demonstrate why reconnaissance is important in security assessments.

| Observation | Potential Security Implication |
|---|---|
| Publicly identifiable web technologies and versions | May help an attacker identify components that require further security review |
| Publicly resolvable server IP address | Reveals information about the web service's network location |
| HTTP headers expose technical details | May assist technology fingerprinting and further enumeration |
| WAF technology is identifiable | Reveals information about the website's security architecture |
| DNS records expose infrastructure details | Can help build a broader infrastructure profile |
| DNSSEC reported as unsigned | DNS responses do not receive DNSSEC validation through this configuration |
| Multiple live hosts discovered on the local network | Unknown devices should be identified and verified by the network owner |

**Note:** These are observations and potential security implications, not confirmed vulnerabilities. Further authorized security testing would be required to establish whether any issue is exploitable or creates an actual security weakness.

## 7. Recommendations

Based on the observations collected during these activities, the following improvements were identified:

1. **Review exposed technology information:** Regularly review publicly visible information about web servers, CMS platforms, plugins, and frameworks.
2. **Maintain updated software:** Keep web technologies, CMS platforms, and plugins updated and review them against relevant security advisories.
3. **Review HTTP security headers:** Evaluate the use of suitable security headers, including HSTS, X-Frame-Options, and Content-Security-Policy.
4. **Review DNS configuration:** Periodically examine publicly exposed DNS records and avoid unnecessary disclosure of software version information.
5. **Enable DNSSEC and review email authentication:** Consider DNSSEC deployment and review SPF configuration to ensure legitimate mail sources are properly covered.
6. **Configure and monitor the WAF:** Keep the WAF enabled, updated, and appropriately configured.
7. **Review publicly discoverable information:** Regularly examine publicly available email addresses, hosts, and subdomains.
8. **Perform regular internal network discovery:** Identify active devices on organizational networks.
9. **Investigate unknown devices:** Verify unexpected devices and confirm whether they are authorized.
10. **Maintain network documentation:** Keep network topology and device records updated.
11. **Conduct authorized security assessments:** Ensure all reconnaissance, scanning, and testing activities remain within an approved scope.

## 8. Skills and Learning Outcomes

Through these practical exercises, I gained an understanding of:

- The role of footprinting and reconnaissance in penetration testing.
- Domain registration and DNS information gathering.
- Website technology fingerprinting.
- HTTP response header analysis.
- Web Application Firewall detection.
- DNS enumeration and record analysis.
- Passive OSINT techniques and the use of external data sources.
- API key requirements in OSINT tools.
- Local IP configuration and subnet identification.
- Host discovery using Nmap and Zenmap.
- Network topology generation and interpretation.
- Identifying potential security implications from reconnaissance findings.
- Documenting technical observations and recommendations.
- The importance of authorization and ethical boundaries in cybersecurity.

## 9. Repository Structure

The repository is organized to keep the main documentation and supporting evidence easy to access.

```text
week2-penetration-testing/
│
├── README.md
│
├── evidence/
│   └── Penetration_Testing_Report_W2-PM.pdf
│
└── screenshots/
    ├── 01-whois-lookup.png
    ├── 02-whatweb-scan.png
    ├── 03-nslookup.png
    ├── 04-http-headers.png
    ├── 05-waf-detection.png
    ├── 06-dns-enumeration.png
    ├── 07-harvester-baidu.png
    ├── 08-harvester-all-sources.png
    ├── 09-local-network-configuration.png
    ├── 10-zenmap-ping-scan.png
    ├── 11-network-topology-pdf.png
    └── 12-zenmap-topology-view.png
```

The screenshots correspond to the evidence figures included in the original internship report. File names may be adjusted to match the actual uploaded image files.

## 10. Documentation and Evidence

The complete Week 2 practical report is available in:

`evidence/Penetration_Testing_Report_W2-PM.pdf`

It contains:
- Detailed explanations of the activities performed.
- Commands used during the practical exercises.
- Observations and results.
- Risk analysis and recommendations.
- Screenshots documenting the practical work.

The `screenshots/` directory contains individual evidence images for easier review.

## 11. Important Disclaimer

This repository is intended strictly for educational and cybersecurity learning purposes.

All activities were performed on systems and networks for which permission was obtained or that were personally owned. The theHarvester exercise against `microsoft.com` was limited to passive OSINT using public sources.

No exploitation, unauthorized access, or vulnerability validation was performed as part of this week's activities.

The observations documented here should not be interpreted as proof of exploitable vulnerabilities. Always obtain appropriate authorization before performing reconnaissance, scanning, or security testing on any system or network.

## 12. Author

**Komal**  
Cybersecurity & Ethical Hacking Intern  
Program/Batch: B084B – Networkwalks

**Focus Areas:**  
Cybersecurity | Ethical Hacking | Penetration Testing | Footprinting | OSINT | Network Scanning

---

*Week 2 Internship Documentation – Footprinting, OSINT & Network Scanning*
