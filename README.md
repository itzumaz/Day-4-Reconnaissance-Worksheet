# Day 4: Passive & Active Infrastructure Reconnaissance

## 📌 Project Overview
This repository contains the documentation, outputs, and structured findings compiled during **Day 4 of my Vulnerability Assessment and Penetration Testing (VAPT) Internship with TriosCyber** (in partnership with Ernith).

The objective of this assignment was to perform authorized passive and active intelligence gathering against `trioscyber.com` and its associated routing endpoints. By combining tools like `whois`, `dig`, `nslookup`, `curl`, and `traceroute`, I mapped out the target's public threat matrix and structured the intelligence into a unified Reconnaissance Worksheet.

---

## 🛠️ Lab Environment & Tools
* **Analysis Platform:** Kali Linux VM (VMware Workspace)
* **Target Domain:** `trioscyber.com`
* **Recon Suite:** WHOIS, dig, nslookup, curl, traceroute

---

## 🚀 Tool Execution & Output Summary

### A. Domain Registration Query (`whois`)
Used to gather legal infrastructure mappings and registrar administrative records without touching the web application server directly.
```bash
whois trioscyber.com
```
* **Registrar:** Hostinger, UAB
* **Name Servers:** `://dns-parking.com`, `://dns-parking.com`
* **Domain Status:** `clientTransferProhibited` (Prevents unauthorized domain transfers)

### B. DNS Record Enumeration (`dig` / `nslookup`)
Interrogated public name servers to resolve the domain's backend hosting locations and mail routing map.
```bash
dig trioscyber.com A +short
dig trioscyber.com AAAA +short
nslookup -type=MX trioscyber.com
```
* **IPv4 Endpoints (A Records):** `77.37.83.211`, `77.37.53.12`
* **IPv6 Endpoints (AAAA Records):** `2a02:4780:32:4c09:1567:5bfb:e0e4:16c2`
* **Mail Exchanges (MX Records):** Managed natively via Zoho Mail (`mx.zoho.in`, `mx2.zoho.in`).

### C. HTTP Header Banner Grabbing (`curl`)
Executed a quiet header audit to inspect the active web server software stacks and verify missing protective headers.
```bash
curl -I -L https://trioscyber.com
```
* **HTTP Status Code:** `200 OK`
* **Exposed Server Banner:** `LiteSpeed`
* **Active Security Headers:** `Strict-Transport-Security`, `X-Content-Type-Options: nosniff`

### D. Network Path Tracing (`traceroute`)
Traced the physical path and intermediate router hops connecting the local platform to the destination target address.
```bash
traceroute trioscyber.com
```
* **Local Gateway Hop:** `192.168.145.2` (Internal exit interface)
* **Total Hop Distance:** 30 hops max to destination IP `77.37.83.211`

---

## 📋 Consolidated Reconnaissance Worksheet

| Category | Parameter / Record | Discovered Value / Response | Security Implication |
| :--- | :--- | :--- | :--- |
| **Domain** | Registrar Details | Hostinger, UAB | Identifies core hosting platform and registrar provider. |
| **DNS** | A Records (IPv4) | `77.37.83.211`, `77.37.53.12` | Uncovers public load-balancers or target frontend servers. |
| **DNS** | AAAA Records (IPv6) | `2a02:4780:32:4c09...` | Maps out IPv6 endpoint availability and exposure. |
| **DNS** | MX Records | `*.zoho.in` | Reveals third-party enterprise email infrastructure. |
| **HTTP** | Server Banner | `LiteSpeed` | Technology stack exposure; allows version-specific exploit checking. |
| **Network**| Default Gateway | `192.168.145.2` | Documents local virtualized network environment gateway. |

---

## 🛡️ Remediation & Hardening Recommendations
Based on the raw intelligence gathered in this worksheet, several systemic configurations can be optimized from your end to lower visibility for unauthorized scanners:

* **Obscure Web Application Headers:**  
  The server explicitly leaks its underlying software stack (`LiteSpeed`) via response headers. To mitigate banner grabbing, update the web server configuration file to disable or completely mask software signatures.
* **Inject Missing Missing Security Headers:**  
  While `Strict-Transport-Security` is active, the domain should be hardened further. Add missing HTTP defensive headers to block cross-site scripting (XSS) and frame-injection vulnerabilities:
  * `X-Frame-Options: DENY`
  * `Content-Security-Policy (CSP)`
* **Enforce Strict Registrar-Lock Protocols:**  
  The status `clientTransferProhibited` is a good baseline. Ensure that multi-factor authentication (MFA) is strictly enforced on your Hostinger panel access to completely protect against unauthorized DNS hijacking or malicious domain transfers.
* **Filter ICMP TTL Propagation Expirations:**  
  To hide internal routing topology structures from unauthorized mapping queries, configure internal transit routers or firewalls to drop or limit out-of-band TTL expired packets, disrupting step-by-step `traceroute` discovery tracking.

---

## 🧠 Key Takeaways
* **Silent Information Gathering:** Passive reconnaissance (via WHOIS and DNS) acts as a critical phase, letting analysts discover target frameworks without generating high-severity alert logs on the host's primary firewall.
* **Attack Blueprint Assembly:** Consolidating open banners, infrastructure endpoints, and vendor relationships creates a cohesive blueprint that speeds up future vulnerability scanning phases.

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

---

## 👤 Author
**Azeez Umar Opeyemi**
* 💼 **Role:** VAPT Intern at TriosCyber
* 📧 **Email:** umaropeyemiazeez@gmail.com
* 🐙 **GitHub:** [itzumaz](https://github.com/itzumaz)
* 🔗 **LinkedIn:** [Azeez Umar Opeyemi](https://www.linkedin.com/in/azeez-umar-opeyemi-201a433a4/)