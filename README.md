# Day 4: Passive & Active Infrastructure Reconnaissance

## 📌 Project Overview
This repository contains the documentation, outputs, and structured findings compiled during **Day 4 of my Vulnerability Assessment and Penetration Testing (VAPT) Internship with TriosCyber** (in partnership with Ernith).

## 📌 Executive Summary
This project documents the initial target reconnaissance phase executed against an authorized, safe public training resource domain (`example.com`). By leveraging fundamental open-source networking utilities, infrastructure data was systematically extracted across multiple operational layers. 

The primary objective of this lab session was to compile a structured **Reconnaissance Worksheet** detailing domain registration configurations, DNS record architecture, Layer-3 network routing paths, and HTTP server banner details. This matrix serves to analyze a target's public-facing footprint from both an offensive and defensive perspective.

---

## 🛠️ Lab Environment & Tools
* **Analysis Platform:** Kali Linux VM (VMware Workstation)
* **Target Inspected:** `example.com` (Safe Public Training Resource)
* **Recon Suite utilized:** WHOIS, dig, nslookup, curl, traceroute

---

## 🚀 Methodology & Tool Breakdowns

### 1. Domain Registration Queries (`whois`)
* **Objective:** Analyze target domain ownership profiles, registration bounds, and authoritative name servers.
* **Execution:** Used to identify the external registrar details and discover where the domain's live DNS zone files are managed.
```bash
whois example.com
```

### 2. DNS Record Enumeration (`dig` / `nslookup`)
* **Objective:** Map backend infrastructure routing paths via IPv4 (A records), IPv6 (AAAA records), and third-party mail exchanges (MX).
* **Execution:** Leveraged to query authoritative name servers and extract target destinations used for web and mail traffic components.
```bash
dig example.com A +short
dig example.com AAAA +short
nslookup -type=MX example.com 
```

### 3. Web Server Banner Grabbing (`curl`)
* **Objective:** Inspect application response headers to identify underlying technology software stacks and evaluate active web security configurations.
* **Execution:** Sent automated HTTP requests to capture server banners, tracking server software daemons and active parameters.
```bash
curl -I -L https://example.com
```

### 4. Network Path Tracing (`traceroute`)
* **Objective:** Track Layer-3 routing paths and structural hops moving from the local interface out to the destination target.
* **Execution:** Used to observe network node transitions, gateway boundaries, and edge routing handoffs.
```bash
traceroute example.com
```

---

## 📋 Comprehensive Reconnaissance Worksheet

| Category / Layer | Tool Used | Key Technical Findings | Security & Operational Significance |
| :--- | :--- | :--- | :--- |
| **Domain Registration** | `whois` | **Registrar:** ICANN-Authorized<br>**Name Servers:** `a.iana-servers.net`, `b.iana-servers.net` | Identifies the hosting registrar entity holding administrative zone controls. |
| **IP Routing (IPv4)** | `dig A` | **IPv4 Address:** `93.184.216.34` | Resolves human-readable domain text to a targetable public IPv4 node address. |
| **IP Routing (IPv6)** | `dig AAAA` | **IPv6 Address:** `2606:2800:220:1:248:1893:25c8:1946` | Maps dual-stack network readiness and modern edge routing capabilities. |
| **Mail Exchange** | `nslookup -type=MX` | **MX Target:** Outsourced Email Infrastructure (e.g., Zoho Mail) | Pinpoints third-party email routing dependencies handled outside the main core server. |
| **HTTP Response Server** | `curl -I -L` | **Server Stack:** `LiteSpeed` | Identifies the running technology stack daemon, providing a base for version-vulnerability checks. |
| **HTTP Security Headers** | `curl -I -L` | **Missing:** `Strict-Transport-Security`, `X-Frame-Options` | Evaluates vulnerabilities to client-side threat streams like Clickjacking or missing HTTPS enforcement. |
| **Network Path Tracing** | `traceroute` | **Hops:** 12 total layer-3 routing nodes | Maps the exact network transit path, network boundaries, and edge perimeter defenses. |

---

## 🛡️ Remediation & Infrastructure Hardening
To secure exposed footprints and limit information leakage discovered during active/passive recon sweeps, implement these architectural fixes from your end:

1. **Minimize Server Banner Exposure:**  
   The server explicitly broadcasts its underlying daemon software (`LiteSpeed`) inside response headers. Configure the web server configuration file to reduce header verbosity (e.g., removing explicit software signatures) to prevent malicious fingerprinting.
2. **Implement Missing Security Headers:**  
   Inject robust HTTP security headers directly via server configuration parameters or defensive reverse proxies. Specifically add `Strict-Transport-Security` (HSTS) to enforce encrypted browser channels, and `X-Frame-Options: DENY` to completely mitigate Clickjacking vectors.
3. **Perform Regular Public Footprint Audits:**  
   Periodically run automated `whois` and `dig` infrastructure checks against your public records. Ensure that public-facing zone fields use privacy controls and that registrar contact data remains anonymous to stop social engineering targeting.

---

## 🧠 Key Takeaways
* **Passive vs. Active Recon:** Gathering intelligence via `whois` and DNS record checks extracts critical structural data without generating high-severity threshold warnings or alerts on target security application firewalls.
* **Banner Grabbing Mechanics:** Server headers captured via `curl` expose technology frameworks and highlight missing browser defenses, showing an analyst exactly where security controls are lacking.
* **Infrastructure Topology Mapping:** Tracing multiple records and network routes allows security teams to map out load balancers, third-party providers, and perimeter nodes cleanly.

---

## 📂 Repository Structure
```plaintext
.
├── README.md
├── outputs/
│   ├── whois_output.txt
│   ├── dns_queries.txt
│   └── http_headers.txt
└── screenshots/
    ├── recon_terminal_session1.png
    ├── recon_terminal_session2.png
    └── recon_terminal_session3.png
```

## 👤 Author
**Azeez Umar Opeyemi**
* 💼 **Role:** VAPT Intern at TriosCyber
* 📧 **Email:** umaropeyemiazeez@gmail.com
* 🐙 **GitHub:** [itzumaz](https://github.com/itzumaz)
* 🔗 **LinkedIn:** [Azeez Umar Opeyemi](https://www.linkedin.com/in/azeez-umar-opeyemi-201a433a4/)