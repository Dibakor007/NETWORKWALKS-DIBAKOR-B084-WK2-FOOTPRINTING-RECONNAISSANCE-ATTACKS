<div align="center">

# 🛡️ PENETRATION TESTING REPORT
## FOOTPRINTING & NETWORK SCANNING PHASES
### **W2-PM-FINAL \| CYBERSECURITY \| NETWORKWALKS**

<br>

[![Cybersecurity](https://img.shields.io/badge/Cybersecurity-000000?style=for-the-badge&logo=securityscorecard&logoColor=white)](#)
[![Penetration Testing](https://img.shields.io/badge/Penetration_Testing-E53935?style=for-the-badge&logo=kalilinux&logoColor=white)](#)
[![Reconnaissance](https://img.shields.io/badge/Reconnaissance-0288D1?style=for-the-badge&logo=target&logoColor=white)](#)
[![Footprinting](https://img.shields.io/badge/Footprinting-FF6F00?style=for-the-badge&logo=safari&logoColor=white)](#)
[![Nmap](https://img.shields.io/badge/Nmap-004D40?style=for-the-badge&logo=gnubash&logoColor=white)](#)
[![Zenmap](https://img.shields.io/badge/Zenmap-512DA8?style=for-the-badge&logo=linux&logoColor=white)](#)
[![Networkwalks](https://img.shields.io/badge/Networkwalks-C2185B?style=for-the-badge&logo=cisco&logoColor=white)](#)
[![OSINT](https://img.shields.io/badge/OSINT-00796B?style=for-the-badge&logo=googlechrome&logoColor=white)](#)
[![Ethical Hacking](https://img.shields.io/badge/Ethical_Hacking-212121?style=for-the-badge&logo=kalilinux&logoColor=00E676)](#)

</div>

---

| **Field** | **Detail** |
| :--- | :--- |
| **Pentester Name (Cybersecurity Professional)** | **Dibakor Pramanik** |
| **GitHub Profile** | [@Dibakor007](https://github.com/Dibakor007) |
| **Program / Batch** | **B084-Networkwalks** |
| **Date** | 10 November 2026 |
| **Modules Completed** | **W2-PM1** (Multiple Kali Tools) & **W2-PM5** (Zenmap Scanning) |
| **Client / Target** | 1. Networkwalks (`networkwalks.com`) – Secured written permission already<br>2. My own local lab / LAN Network |
| **Permission secured from client?** | **Yes** (NW-LOA-B084-017) |
| **Phases Covered** | • **Phase 1:** Reconnaissance & Footprinting (Completed)<br>• **Phase 2:** Scanning & Network Discovery (Completed)<br>• **Phase 3–5:** In Progress |

---

## 1. Liability Disclaimer

> [!WARNING]
> I have performed these activities only on the systems and devices where I had secured written permission or the devices/systems that I own myself. All these materials are for educational and research purposes only. Do not use anything from here to break the law. The instructor, the authors, and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job, and a permanent record. In most countries, unauthorized access is a crime even when nothing is damaged.

---

## 2. Introduction

This report covers footprinting the **`networkwalks.com`** domain using multiple Kali Linux tools (**W2-PM1**) and scanning the target network environment with **Zenmap** and **Nmap** (**W2-PM5**). One module covers the footprinting phase and the other covers the scanning phase, so together they demonstrate how an attacker or ethical hacker moves from gathering public intelligence to mapping active hosts and running services on a network. 

This work represents the **Week 2** deliverable of my ongoing Cybersecurity Internship program at Networkwalks. All commands were executed within a Kali Linux virtual machine (footprinting) and Zenmap GUI / CLI (scanning). Every step documented below includes:
- The exact command executed
- The observed result
- Visual screenshot evidence
- A technical analysis of why the finding matters from an attacker's and defender's perspective

---

## 3. Tools Used

The table below lists each tool utilized during the execution of this project and its operational purpose:

| **Tool** | **Purpose** |
| :--- | :--- |
| **Kali Linux** | Security operating system utilized for running penetration testing and reconnaissance tools. |
| **WHOIS** | Query domain registration details, administrative contacts, registrar info, and name servers. |
| **WhatWeb** | Fingerprint web technologies, web server version, CMS platform, plugins, and server IP. |
| **nslookup** | Query Domain Name System (DNS) servers to resolve target domain names to IP addresses. |
| **cURL (`curl -I`)** | Retrieve and inspect HTTP/HTTPS response headers and server banner information. |
| **wafw00f** | Detect and fingerprint Web Application Firewalls (WAF) protecting the web application. |
| **DNSRecon** | Perform advanced DNS enumeration (SOA, NS, MX, TXT, SPF, SRV, and zone records). |
| **Zenmap (Nmap GUI)** | Perform network scanning, discover live hosts, identify open ports, map topology, and detect OS. |
| **Nmap (CLI)** | Validate service versions (`-sV`) and run low-level network discovery scans via terminal. |

---

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance (W2-PM1)

Reconnaissance was performed against the `networkwalks.com` domain using six core Kali Linux utilities: **WHOIS**, **WhatWeb**, **Nslookup**, **cURL**, **Wafw00f**, and **DNSRecon**. Each tool was utilized to harvest distinct layers of intelligence:

1. **WHOIS Query:**  
   First, I used `whois networkwalks.com` to obtain publicly available domain registration details and identify authoritative name servers. The output confirmed that the domain is registered through **GoDaddy.com, LLC** (IANA ID: `146`), registered until **November 2027**, and uses **HostGator** name servers (`NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM`). Domain status codes include `clientTransferProhibited`, `clientUpdateProhibited`, `clientRenewProhibited`, and `clientDeleteProhibited`.
   
   ![WHOIS Evidence](screenshots/01_pm1_whois.png)

2. **WhatWeb Technology Fingerprinting:**  
   Next, I executed `whatweb networkwalks.com` to analyze the web application stack. The scan revealed that the website redirects (`301 Moved Permanently`) to HTTPS (`200 OK`) and is hosted on IP **`192.232.216.135`** (United States). The underlying stack was identified as **WordPress 7.1.4**, running on an **Apache** web server, utilizing **WordPress Download Manager 3.3.76**, **Bootstrap 7.1.4**, and **JQuery**, with an administrative contact email exposed (`info@networkwalks.com`).
   
   ![WhatWeb Evidence](screenshots/02_pm1_whatweb.png)

3. **Nslookup DNS Resolution:**  
   Using `nslookup networkwalks.com`, I queried the DNS system to verify address resolution. The target domain resolved directly to IPv4 address **`192.232.216.135`** via the local DNS resolver (`192.168.0.1:53`).
   
   ![Nslookup Evidence](screenshots/03_pm1_nslookup.png)

4. **cURL HTTP Response Header Inspection:**  
   I executed `curl -I https://networkwalks.com` to examine the HTTP/2 response headers. The response returned `HTTP/2 200`, confirmed an **Apache** server banner, identified an active WordPress REST API endpoint (`link: <https://networkwalks.com/wp-json/>; rel="https://api.w.org/"`), and displayed reverse proxy caching headers (`x-nginx-cache: WordPress`, `x-endurance-cache-level: 0`).
   
   ![cURL Evidence](screenshots/04_pm1_curl_headers.png)

5. **Wafw00f Firewall Detection:**  
   To evaluate perimeter protections, I launched `wafw00f https://networkwalks.com`. After sending two fingerprint probes, Wafw00f confirmed that the website is protected behind **ModSecurity (SpiderLabs)** Web Application Firewall.
   
   ![Wafw00f Evidence](screenshots/05_pm1_wafw00f.png)

6. **DNSRecon Advanced DNS Enumeration:**  
   Finally, I ran `dnsrecon -d networkwalks.com` to perform full zone and record enumeration. The tool successfully retrieved:
   - **SOA Record:** `ns6135.hostgator.com` (`50.87.144.87`)
   - **NS Records:** `ns6135.hostgator.com`, `ns6136.hostgator.com` (`192.232.216.131`)
   - **Bind Version:** Exposed as `9.16.23-RH`
   - **MX Record:** `mail.networkwalks.com` (`192.232.216.135`)
   - **A Record:** `networkwalks.com` ➔ `192.232.216.135`
   - **TXT Records:** Google site verification code and SPF policy (`v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all`)
   - **SRV Records:** 8 cPanel Auto-discovery records (`_autodiscover._tcp.networkwalks.com`) pointing to `cpanelemaildiscovery.cpanel.net:443`
   
   ![DNSRecon Evidence](screenshots/06_pm1_dnsrecon.png)

---

### 4.2 Network Scanning with Zenmap (W2-PM5)

For the network scanning practical, I utilized **Zenmap** (the official GUI frontend for Nmap) and the **Nmap CLI** to execute active network discovery, port enumeration, service version detection, and operating system fingerprinting against the target infrastructure (`networkwalks.com` / `192.232.216.135`):

1. **Intense Scan Execution:**  
   I initiated an **Intense scan** using the command `nmap -T4 -A -v networkwalks.com`. Zenmap actively probed the top 1,000 TCP ports, completed SYN stealth scans, initiated OS detection, and engaged default NSE scripts.
   
   ![Zenmap Intense Scan](screenshots/07_pm5_zenmap_intense_scan.png)

2. **Open Ports & Services Discovery:**  
   The scan successfully discovered **15 open ports**, **6 filtered ports**, and **979 closed ports**:

   | **Port** | **Protocol** | **State** | **Service** | **Version / Banner** |
   | :---: | :---: | :---: | :---: | :--- |
   | **21** | tcp | open | ftp | Pure-FTPd (with SSL/TLS) |
   | **22** | tcp | open | ssh | OpenSSH 9.9 (protocol 2.0) |
   | **25** | tcp | open | smtp | Exim smtpd 4.100.1 |
   | **26** | tcp | open | smtp | Exim smtpd 4.100.1 |
   | **53** | tcp | open | domain | ISC BIND 9.16.23 (RedHat Linux) |
   | **80** | tcp | open | http | Apache httpd |
   | **110** | tcp | open | pop3 | Dovecot pop3d |
   | **143** | tcp | open | imap | Dovecot imapd |
   | **443** | tcp | open | ssl/http | Apache httpd |
   | **465** | tcp | open | ssl/smtp | Exim smtpd 4.100.1 |
   | **587** | tcp | open | smtp | Exim smtpd 4.100.1 |
   | **993** | tcp | open | ssl/imap | Dovecot imapd |
   | **995** | tcp | open | ssl/pop3 | Dovecot pop3d |
   | **2222** | tcp | open | ssh | OpenSSH 9.9 (protocol 2.0) |
   | **3306** | tcp | open | mysql | MySQL 5.7.44-48 |
   | **1719, 1720, 2001, 5060, 5061, 49152** | tcp | filtered | voip/sip/dc | Filtered perimeter ports |

   ![Zenmap Ports & Services](screenshots/08_pm5_zenmap_ports_services.png)

3. **Network Topology Mapping:**  
   I opened the **Topology** section in Zenmap, which visually rendered the routing path and connection from the scanning workstation (`localhost`) to the destination target node (`networkwalks.com` / `192.232.216.135`).
   
   ![Zenmap Topology](screenshots/09_pm5_zenmap_topology.png)

4. **Host Viewer & Operating System Detection:**  
   Inspecting the **Host Viewer** modal in Zenmap, the system evaluated open port responses against Nmap’s OS signature database, returning an 86% fingerprint match for underlying Linux / RedHat kernel architectures.
   
   ![Zenmap Host Viewer](screenshots/10_pm5_zenmap_host_viewer.png)

5. **Host Details Summary:**  
   The **Host Details** tab in Zenmap summarized the target state: State **Up**, 15 open ports, 6 filtered ports, 979 closed ports, and reverse DNS (PTR) mapped to `192-232-216-135.unifiedlayer.com`.
   
   ![Zenmap Host Details](screenshots/11_pm5_zenmap_host_details.png)

6. **CLI Service Scan Validation (`nmap -sV`):**  
   To independently corroborate the graphical Zenmap findings, I executed a dedicated version detection scan via terminal:
   ```bash
   nmap -sV networkwalks.com -T4
   ```
   The terminal output confirmed identical daemon versions: Pure-FTPd, OpenSSH 9.9, Exim 4.100.1, ISC BIND 9.16.23, Apache, Dovecot, and MySQL 5.7.44-48 running on HostGator Linux infrastructure (`gator3068.hostgator.com`).
   
   ![Nmap CLI Service Scan](screenshots/12_pm5_nmap_cli_service_scan.png)

---

## 5. Risk Analysis / Impact

Based on the intelligence and network response data collected during the footprinting and network scanning activities, I identified the following potential security risks:

| **#** | **Risk / Finding** | **Evidence / Observation** | **Potential Impact** | **Risk Level** |
| :---: | :--- | :--- | :--- | :---: |
| **1** | **Direct Public Exposure of Database Service** | Zenmap & Nmap identified Port 3306/tcp open running MySQL 5.7.44-48 | Database port is reachable over the public internet. Attackers can attempt direct brute-force authentication or exploit database engine vulnerabilities. | **● High** |
| **2** | **Web technology information exposed** | WhatWeb identified WordPress 7.1.4 and WP Download Manager 3.3.76 | Attackers may use exposed technology and plugin version information to identify software requiring further security review or unpatched CVEs. | **● Medium** |
| **3** | **Multiple live ports and services visible** | Zenmap identified 15 open ports (FTP, SSH, SMTP, POP3, IMAP, MySQL, DNS, HTTP/S) | Increases the target's overall attack surface; unencrypted legacy services (Port 21, 110, 143) expose data to transit interception. | **● Medium** |
| **4** | **DNS infrastructure information exposed** | DNSRecon identified DNS, BIND 9.16.23-RH, mail servers, and SRV records | Detailed DNS enumeration helps an attacker construct a comprehensive infrastructure profile and map internal cPanel routing. | **● Medium** |
| **5** | **Server IP address identifiable** | Nslookup and WhatWeb resolved the domain to `192.232.216.135` | Provides exact network location of the web host, enabling direct IP-level scanning that may bypass CDN or DNS-level protections. | **● Low** |
| **6** | **HTTP technical information exposed** | cURL returned HTTP response headers and exposed `/wp-json/` | Assists technology fingerprinting, reveals reverse proxy caching architecture, and exposes WordPress API discovery endpoints. | **● Low** |
| **7** | **WAF technology identifiable** | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the perimeter security architecture, enabling attackers to craft specific WAF-bypass techniques. | **● Low** |

### Risk Level Key:
- **● High**
- **● Medium**
- **● Low**

> [!NOTE]
> The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities. The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these two modules. Therefore, the presence of information such as a software version, IP address, or DNS record does not by itself mean that the system is vulnerable. Further authorized security testing would be required to confirm any actual vulnerability.

---

## 6. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

1. **Restrict Database Public Accessibility:**  
   MySQL (Port 3306) should not be exposed to the public Internet. Bind MySQL exclusively to `localhost` (`127.0.0.1`) or place it behind a private firewall / VPN with strict IP allowlisting.

2. **Review Publicly Exposed Technology Information:**  
   Organizations should regularly review what information about their web technologies, CMS, and plugins is publicly visible.

3. **Keep Software Updated:**  
   CMS platforms, plugins, and server daemons (Apache, OpenSSH, Exim, BIND, Dovecot, MySQL) should be regularly updated and reviewed against current security advisories.

4. **Disable or Restrict Unencrypted Services:**  
   Decommission cleartext transmission protocols (plain FTP on port 21, POP3 on port 110, and IMAP on port 143) and enforce TLS-only equivalents (FTPS/SFTP, POP3S port 995, IMAPS port 993).

5. **Review HTTP Headers:**  
   HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed. Implement modern security headers including `Strict-Transport-Security` (HSTS), `X-Frame-Options`, and `Content-Security-Policy` (CSP).

6. **Review DNS Records Regularly:**  
   DNS records should be checked periodically to ensure that only required information and services are publicly exposed. Suppress BIND version disclosure by configuring `version "none";` in `named.conf`.

7. **Properly Configure and Monitor the WAF:**  
   Keep the WAF (ModSecurity) enabled, regularly updated, and tuned with current Core Rule Sets (CRS) to detect and block automated application attacks.

8. **Perform Regular Network Discovery:**  
   Organizations should periodically scan their own network perimeter and subnets using Nmap/Zenmap to identify newly exposed services or unauthorized listening ports.

9. **Investigate Unknown Devices and Open Ports:**  
   Any unexpected port or device discovered during regular scanning should be promptly investigated and verified by the security team.

10. **Maintain Up-to-Date Network Documentation:**  
    Network topology, firewall rules, and active host inventories should be documented and updated regularly.

11. **Perform Security Testing Exclusively with Authorization:**  
    Reconnaissance and scanning must always be performed against systems and networks where appropriate written authorization has been formally secured.

---

## 7. Conclusion

During Week 2 of my Cybersecurity Internship at Networkwalks, I completed practical activities covering **footprinting, reconnaissance, and network scanning**.

In the **footprinting activity (W2-PM1)**, I used six Kali Linux tools to collect intelligence about the target domain:
- **WHOIS** provided domain registration, ownership, and name server allocations.
- **WhatWeb** fingerprinted the underlying web technologies (WordPress, Apache, Bootstrap).
- **Nslookup** confirmed direct DNS resolution to the host IP.
- **cURL** inspected HTTP response headers, reverse proxy caching, and REST endpoints.
- **Wafw00f** verified the presence of ModSecurity WAF.
- **DNSRecon** uncovered comprehensive DNS records (SOA, MX, SPF, SRV, and BIND versions).

In the **network scanning activity (W2-PM5)**, I used **Zenmap** and the **Nmap CLI** to evaluate the target environment:
- Executed intense port discovery across 1,000 TCP ports.
- Cataloged 15 active listening services (including FTP, SSH, Mail, DNS, Web, and MySQL).
- Generated visual network topology mapping from the scanner to the destination.
- Analyzed host details, reverse DNS PTR mappings, and OS fingerprint signatures.
- Validated all graphical findings using command-line service scans (`nmap -sV`).

These exercises demonstrated that **information gathering is the foundational phase of cybersecurity**. Even before attempting to test or evaluate a system for vulnerabilities, a security professional can learn a significant amount about an environment by carefully analyzing publicly available data and network responses. 

I also learned that **technical findings must be documented clearly and responsibly**. A high-quality cybersecurity report explains what was performed, what was discovered, what the observation means, what risk it creates, and what can be done to remediate that risk. Finally, I reaffirmed that reconnaissance and scanning must always be conducted strictly within an authorized scope under a valid Letter of Authorization (LOA).

---

## 8. Evidences Collected

All 12 visual evidences captured during the practical lab execution are cataloged below:

| # | Evidence File | Phase / Practical | Command / Activity |
| :---: | :--- | :---: | :--- |
| **01** | [`screenshots/01_pm1_whois.png`](screenshots/01_pm1_whois.png) | W2-PM1 Footprinting | `whois networkwalks.com` |
| **02** | [`screenshots/02_pm1_whatweb.png`](screenshots/02_pm1_whatweb.png) | W2-PM1 Footprinting | `whatweb networkwalks.com` |
| **03** | [`screenshots/03_pm1_nslookup.png`](screenshots/03_pm1_nslookup.png) | W2-PM1 Footprinting | `nslookup networkwalks.com` |
| **04** | [`screenshots/04_pm1_curl_headers.png`](screenshots/04_pm1_curl_headers.png) | W2-PM1 Footprinting | `curl -I https://networkwalks.com` |
| **05** | [`screenshots/05_pm1_wafw00f.png`](screenshots/05_pm1_wafw00f.png) | W2-PM1 Footprinting | `wafw00f https://networkwalks.com` |
| **06** | [`screenshots/06_pm1_dnsrecon.png`](screenshots/06_pm1_dnsrecon.png) | W2-PM1 Footprinting | `dnsrecon -d networkwalks.com` |
| **07** | [`screenshots/07_pm5_zenmap_intense_scan.png`](screenshots/07_pm5_zenmap_intense_scan.png) | W2-PM5 Network Scanning | Zenmap: `nmap -T4 -A -v networkwalks.com` |
| **08** | [`screenshots/08_pm5_zenmap_ports_services.png`](screenshots/08_pm5_zenmap_ports_services.png) | W2-PM5 Network Scanning | Zenmap: Ports/Hosts 15 Open Ports Table |
| **09** | [`screenshots/09_pm5_zenmap_topology.png`](screenshots/09_pm5_zenmap_topology.png) | W2-PM5 Network Scanning | Zenmap: Network Topology View |
| **10** | [`screenshots/10_pm5_zenmap_host_viewer.png`](screenshots/10_pm5_zenmap_host_viewer.png) | W2-PM5 Network Scanning | Zenmap: Host Viewer & OS Detection |
| **11** | [`screenshots/11_pm5_zenmap_host_details.png`](screenshots/11_pm5_zenmap_host_details.png) | W2-PM5 Network Scanning | Zenmap: Host Details & Port Counters |
| **12** | [`screenshots/12_pm5_nmap_cli_service_scan.png`](screenshots/12_pm5_nmap_cli_service_scan.png) | W2-PM5 Network Scanning | Terminal: `nmap -sV networkwalks.com -T4` |

---

<div align="center">

**— End of Penetration Testing Report —**

<br>

👤 **Author:** **Dibakor Pramanik**  
Cybersecurity Analyst \| Batch B084  
GitHub Profile: [https://github.com/Dibakor007](https://github.com/Dibakor007)

📌 **Project Information:**  
Program Name: **Cybersecurity Internship Program at Networkwalks** \| Week: **02**  
Repository: [`NETWORKWALKS-DIBAKOR-PRAMANIK-B084-WK2-FOOTPRINTING-RECONNAISSANCE-ATTACKS`](https://github.com/Dibakor007/NETWORKWALKS-DIBAKOR-PRAMANIK-B084-WK2-FOOTPRINTING-RECONNAISSANCE-ATTACKS)

</div>
