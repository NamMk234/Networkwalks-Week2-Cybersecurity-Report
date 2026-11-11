# Networkwalks-Week2-Cybersecurity-Report

Week 2 cybersecurity report: Kali footprinting and Zenmap network scanning



\# 🤖 NETWORKWALKS WEEK 2 CYBERSECURITY REPORT

\## Footprinting \& Network Scanning Phases



!\[Kali Linux](https://img.shields.io/badge/Kali-Linux-557C94?logo=kalilinux\&logoColor=white)

!\[Nmap](https://img.shields.io/badge/Nmap-Zenmap-4682B4)

!\[Cybersecurity](https://img.shields.io/badge/Cybersecurity-Ethical%20Hacking-red)

!\[Networkwalks](https://img.shields.io/badge/Networkwalks-Week%202-darkred)



\*\*W2-PM FINAL | CYBERSECURITY \& ETHICAL HACKING | NETWORKWALKS\*\*



\---



\## 🚧 Project Overview



| Field | Details |

|---|---|

| Pentester | Zamzam Mohamed |

| Program / Batch | Networkwalks – B084 |

| Date | 10 November 2026 |

| Modules Completed | W2-PM1 – Footprinting with Multiple Kali Linux Tools; W2-PM5 – Network Scanning with Zenmap |

| Targets | `networkwalks.com` (assigned lab target) and my own home Wi-Fi network (`192.168.1.0/24`) |

| Phases Covered | Phase 1: Reconnaissance \& Footprinting; Phase 2: Scanning \& Network Discovery |



\---



\## ⚠️ 1. Disclaimer



All activities documented in this report were conducted within the scope of the assigned Networkwalks lab target and my own home network.



This report is intended for educational and research purposes only. The information presented should not be used to conduct unauthorized reconnaissance, scanning, or other activities against systems or networks.



\*\*Always obtain proper authorization before performing cybersecurity testing.\*\*



\---



\## ☄️ 2. Introduction



This report documents the practical activities completed during Week 2 of my Networkwalks Cybersecurity \& Ethical Hacking internship.



The project focused on two important phases of the penetration testing process:



1\. \*\*Footprinting and Reconnaissance:\*\* Gathering publicly available information about the `networkwalks.com` domain using six Kali Linux tools.

2\. \*\*Scanning and Network Discovery:\*\* Identifying live hosts on my own local network using Zenmap, the graphical interface for Nmap.



For each activity, I documented the commands used, the observed results, supporting screenshots, and the security relevance of the findings.



\### Project Environments



| Phase | Environment |

|---|---|

| Footprinting and Reconnaissance | Kali Linux running in VirtualBox |

| Network Scanning | Zenmap on my Windows host machine |



\### 🎯 Project Objectives



\- Understand the purpose of footprinting and reconnaissance.

\- Collect publicly available domain registration and DNS information.

\- Identify technologies and hosting information associated with a website.

\- Inspect HTTP response headers.

\- Check for the presence of a Web Application Firewall (WAF).

\- Discover live hosts on a local network using Zenmap.

\- Identify IP and MAC addresses of discovered devices.

\- Generate and save a network topology.

\- Document findings, potential risks, and security recommendations.



\---



\## ⚒️ 3. Tools and Technologies Used



| Tool / Command | Purpose |

|---|---|

| Kali Linux | Operating system used for reconnaissance activities |

| Windows 11 | Host operating system used for Zenmap scanning and `ipconfig` |

| `whois` | Retrieves public domain registration information |

| `whatweb` | Identifies web technologies, server information, and CMS details |

| `nslookup` | Resolves domain names to IP addresses |

| `curl -I` | Retrieves HTTP response headers |

| `wafw00f` | Checks for the presence of a Web Application Firewall |

| `dnsrecon` | Enumerates DNS records, including NS, MX, SPF, TXT, and SRV records |

| Zenmap (Nmap GUI) | Discovers live hosts and identifies network information |

| `ipconfig` / `ipconfig /all` | Displays local network configuration, including IP and MAC addresses |



\---



\## 🛡️ 4. Footprinting and Reconnaissance (W2-PM1)



\### 4.1 Background



Footprinting is an information-gathering phase in cybersecurity testing. It involves collecting details about a target before conducting further security assessments.



This information may include domain registration details, IP addresses, hosting providers, web technologies, DNS records, mail servers, and security technologies.



During this lab, I performed footprinting on the assigned domain, \*\*`networkwalks.com`\*\*, using six Kali Linux tools.



\### 4.2 Tools and Findings



\#### 4.2.1 WHOIS



\*\*Purpose:\*\* Retrieve publicly available domain registration information.



\*\*Command:\*\*



```bash

whois networkwalks.com

```



!\[WHOIS Result](images/01\_whois.png)



\*\*Key Findings\*\*



\- Registrar: GoDaddy.com, LLC.

\- Domain creation date: 6 November 2019.

\- Last updated: 12 November 2025.

\- Expiry date: 6 November 2027.

\- Name servers: `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM` (the registrar record also lists `NS29/NS30.DOMAINCONTROL.COM`).

\- Registrant information is protected by a privacy service (Domains By Proxy, LLC).

\- Domain status locks include `clientTransferProhibited`, `clientUpdateProhibited`, `clientRenewProhibited`, and `clientDeleteProhibited`.

\- DNSSEC status: unsigned.



\*\*Security Relevance\*\*



WHOIS information can help build an initial profile of a domain. Registration details, name servers, and publicly listed contacts may provide useful information during reconnaissance.



\*\*Positive Observation:\*\* WHOIS privacy helps protect the registrant's personal information, while domain status locks provide additional protection against certain unauthorized domain changes.



\---



\#### 4.2.2 WhatWeb



\*\*Purpose:\*\* Identify technologies and software associated with the target website.



\*\*Command:\*\*



```bash

whatweb networkwalks.com

```



!\[WhatWeb Result](images/02\_whatweb.png)



\*\*Key Findings\*\*



\- HTTP redirects to HTTPS (301 redirect).

\- Web server: Apache.

\- CMS: WordPress 7.1.4.

\- Plugin: WordPress Download Manager 3.3.76.

\- JavaScript library: jQuery 3.7.1.

\- Server IP address: `192.232.216.135` (United States).

\- Email address identified: `info@networkwalks.com`.

\- Cookie `\_\_wpdm\_client` is marked HttpOnly.

\- Page title: Networkwalks Academy.



\*\*Security Relevance\*\*



Identifying the CMS and plugin versions can help security professionals check whether known vulnerabilities have been reported for those versions.



The exposed IP address and email address also provide information that may be useful during further reconnaissance.



\*\*Additional Observation:\*\* WhatWeb also reported `Bootstrap\[7.1.4]`. Since this does not appear to be a valid Bootstrap version, it may be a false positive. This demonstrates the importance of verifying automated tool results rather than accepting every finding without cross-checking.



\---



\#### 4.2.3 Nslookup



\*\*Purpose:\*\* Resolve the target domain name to its associated IP address.



\*\*Command:\*\*



```bash

nslookup networkwalks.com

```



!\[Nslookup Result](images/03\_nslookup.png)



\*\*Key Findings\*\*



\- DNS server used: `8.8.8.8` (Google Public DNS).

\- Domain `networkwalks.com` resolves to `192.232.216.135`.

\- The result was returned as a non-authoritative answer.

\- The resolved IP address matches the address identified by WhatWeb.



\*\*Security Relevance\*\*



DNS resolution provides information about the IP address associated with a domain. This helps security professionals understand how a domain maps to its hosting infrastructure.



\---



\#### 4.2.4 cURL



\*\*Purpose:\*\* Retrieve and inspect the HTTP response headers returned by the website.



\*\*Command:\*\*



```bash

curl -I https://networkwalks.com

```



!\[cURL Result](images/04\_curl.png)



\*\*Key Findings\*\*



\- HTTP status: `200 OK`.

\- Protocol: HTTP/2.

\- Server: Apache.

\- The `link` response header exposes the WordPress REST API path `/wp-json/`.

\- Cookie `\_\_wpdm\_client` includes the `Secure` and `HttpOnly` attributes.

\- Cache-related headers include `x-nginx-cache` and `x-endurance-cache-level`.

\- A `permissions-policy` header is present.

\- The following headers were not observed in the response: `Strict-Transport-Security` (HSTS), `X-Frame-Options`, and `Content-Security-Policy`.



\*\*Security Relevance\*\*



HTTP response headers can reveal information about a website's server, technologies, and configuration.



The WordPress REST API path may be useful for further authorized assessment. The absence of certain security headers is a configuration-hardening observation, not proof of a vulnerability.



\---



\#### 4.2.5 WAFW00F



\*\*Purpose:\*\* Identify whether a Web Application Firewall (WAF) is detected on the target website.



\*\*Command:\*\*



```bash

wafw00f networkwalks.com

```



!\[WAFW00F Result](images/05\_wafw00f.png)



\*\*Key Findings\*\*



\- Tool version: WAFW00F v2.3.1.

\- Number of requests sent: 2.

\- Detected WAF: ModSecurity (SpiderLabs).



\*\*Security Relevance\*\*



A WAF helps protect web applications by inspecting incoming HTTP requests and applying security rules to potentially malicious traffic.



\*\*Positive Observation:\*\* The tool detected ModSecurity. However, identifying a WAF does not establish how effectively it is configured or how well it protects the application.



\---



\#### 4.2.6 DNSRecon



\*\*Purpose:\*\* Enumerate DNS records associated with the target domain.



\*\*Command:\*\*



```bash

dnsrecon -d networkwalks.com

```



!\[DNSRecon Result](images/06\_dnsrecon.png)



\*\*Key Findings\*\*



\- DNSSEC is not configured.

\- Name servers:

&#x20; - `ns6135.hostgator.com` – `50.87.144.87`

&#x20; - `ns6136.hostgator.com` – `192.232.216.131`

\- Mail server (MX): `mail.networkwalks.com`, resolving to `192.232.216.135`.

\- SPF record: `v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com \~all`.

\- The SPF record uses the `\~all` softfail qualifier.

\- A Google site-verification TXT record is present.

\- Eight SRV records (`\_autodiscover.\_tcp`) point to `cpanelemaildiscovery.cpanel.net` on port 443, indicating the use of cPanel-related email discovery services.



\*\*Security Relevance\*\*



DNS records provide useful information about a domain's hosting, email infrastructure, and related services.



The SPF `\~all` qualifier indicates a softfail policy for sources that do not match the SPF record. DNSSEC being unsigned is also worth documenting as a DNS security configuration observation.



\---



\### 4.3 Footprinting Summary



| Category | Finding |

|---|---|

| Website IP address | `192.232.216.135` |

| Hosting provider | HostGator |

| Domain registrar | GoDaddy |

| WHOIS privacy | Enabled |

| CMS / Plugin | WordPress 7.1.4 / WordPress Download Manager 3.3.76 |

| Web server | Apache |

| Detected WAF | ModSecurity (SpiderLabs) |

| Mail / DNS | cPanel-related records, SPF softfail, DNSSEC unsigned |



The WhatWeb, Nslookup, and DNSRecon results reported the same website IP address, `192.232.216.135`. Cross-checking results from multiple tools helped improve confidence in this finding.



\---



\## 🔎 5. Network Scanning with Zenmap (W2-PM5)



\### 5.1 Background



Zenmap is the official graphical user interface for Nmap, an open-source network discovery and security auditing tool.



It provides a graphical way to run Nmap scans, view results, and save scan profiles. In this lab, I used Zenmap to discover live devices on my own home Wi-Fi network.



\### 5.2 Objectives



1\. Install Zenmap on my Windows PC.

2\. Identify my local IP address and subnet.

3\. Discover live hosts on the local network.

4\. Identify discovered devices' IP and MAC addresses.

5\. Generate and save the network topology as a PDF.



\### 5.3 Procedure



\*\*Task 1 – Install Zenmap\*\*



Downloaded the Windows installer from the official Nmap website. I retained the default installation components, including Nmap, Npcap, and the Zenmap GUI, and completed the installation.



\*\*Task 2 – Identify Local Network Information\*\*



Used Command Prompt to retrieve the local network configuration.



Commands:



```cmd

ipconfig

ipconfig /all

```



The following network details were identified:



| Item | Value |

|---|---|

| My PC IPv4 address | `192.168.1.20` |

| Subnet mask | `255.255.255.0 (/24)` |

| Subnet scanned | `192.168.1.0/24` |

| Default gateway | `192.168.1.1` |



\*\*Note:\*\* My PC's IP address changed from `192.168.1.18` to `192.168.1.20` between scans because the router assigns addresses through DHCP. The results documented below correspond to one scan, using the network configuration recorded at the same time.



\*\*Tasks 3–6 – Discover Live Hosts\*\*



Selected the \*\*Ping Scan\*\* profile in Zenmap and ran the scan with administrator privileges to help display MAC address information.



\*\*Command:\*\*



```bash

nmap -sn 192.168.1.0/24

```



!\[Zenmap Scan Result](images/07\_zenmap\_scan.png)



\### 5.4 Results



\*\*Task 4: How many hosts were live?\*\*



The scan identified \*\*6 live hosts\*\*: five other devices and my own Windows PC.



The scan covered 256 addresses and completed in 11.51 seconds.



\*\*Tasks 5 and 6: IP and MAC Address Identification\*\*



| # | IP Address | MAC Address (Partially Hidden) | Vendor (Nmap) | Likely Device |

|---|---|---|---|---|

| 1 | `192.168.1.1` | `98:C7:A4:XX:XX:XX` | Shenzhen HS Fiber Communication Equipment | Router / default gateway |

| 2 | `192.168.1.2` | `CE:93:38:XX:XX:XX` | Unknown | Unidentified device; possibly using a private MAC |

| 3 | `192.168.1.3` | `8C:44:BB:XX:XX:XX` | Shen Zhen Tenda Technology | Tenda network device |

| 4 | `192.168.1.8` | `AC:F1:08:XX:XX:XX` | LG Innotek | Unidentified device |

| 5 | `192.168.1.16` | `26:71:F0:XX:XX:XX` | Unknown | Unidentified device; possibly using a private MAC |

| 6 | `192.168.1.20` | `FA:1A:2A:XX:XX:XX` | N/A (own PC) | My Windows PC |



\*\*Observations\*\*



\- MAC addresses have been partially hidden to protect the privacy of devices on the local network.

\- The device descriptions are estimates based on the available vendor information and are not confirmed device identities.

\- Some MAC addresses did not return a recognizable vendor. One possible explanation is the use of private or randomized MAC addresses.

\- Nmap did not display a MAC address for the machine running the scan, so I retrieved my PC's MAC address using `ipconfig /all`.

\- Other scans performed on the same day identified five and seven hosts. This showed that the number of live devices can change as devices connect to and disconnect from the network.



\### 5.5 Network Topology (Task 7)



The final task involved generating a visual representation of the discovered network.



The topology displayed my PC (localhost) at the centre, with the discovered live hosts around it. I opened the \*\*Legend\*\* to understand the symbols and used \*\*Save Graphic\*\* to export the topology as a PDF.



!\[Network Topology and Legend](images/08\_topology\_legend.png)



\*\*Saved file:\*\* `topology-192.168.1.0-24.pdf`



\*\*Important Observation:\*\* The command `nmap -sn` performs host discovery. It does not perform a port scan, so the resulting topology does not provide port-level details.



\---



\## 🎯 6. Risk Analysis and Potential Impact



The following table summarizes the observations made during the lab and their potential security implications.



| # | Risk / Finding | Evidence / Observation | Potential Impact | Preliminary Risk Level |

|---|---|---|---|---|

| 1 | Web technologies and software versions exposed | WhatWeb identified WordPress and plugin versions | Could help attackers research known vulnerabilities affecting the identified versions | 🟡 Medium |

| 2 | Server IP address identifiable | Nslookup resolved the domain to `192.232.216.135` | Provides information about the website's hosting infrastructure | 🟢 Low |

| 3 | HTTP headers and API endpoint exposed | cURL identified `/wp-json/`; certain hardening headers were not observed | May assist technology fingerprinting and further enumeration | 🟢 Low |

| 4 | WAF technology identifiable | WAFW00F detected ModSecurity (SpiderLabs) | Reveals information about the website's security infrastructure | 🟢 Low |

| 5 | DNS and mail infrastructure exposed | DNSRecon identified MX, SPF, SRV, and cPanel-related records | Helps build a profile of the domain's infrastructure | 🟡 Medium |

| 6 | SPF softfail and unsigned DNSSEC | SPF record ends with `\~all`; DNSSEC is unsigned | Indicates email authentication and DNS security configuration areas that may warrant review | 🟢 Low |

| 7 | Multiple live devices identified on the local network | Zenmap discovered six live hosts on `192.168.1.0/24` | Unrecognized devices may require investigation by the network owner | 🟡 Medium |



\*\*Risk Rating Key:\*\* 🔴 Critical | 🟡 Medium | 🟢 Low



\*\*Assessment Note:\*\* These ratings are preliminary assessments of the observations, not confirmed vulnerabilities. The activities involved information gathering and host discovery only. No exploitation or vulnerability testing was performed. Additional authorized testing would be necessary to confirm whether any actual security weaknesses exist.



\---



\## 🛡️ 7. Recommendations



Based on the observations made during the practical activities, the following measures are recommended:



1\. \*\*Review publicly exposed technology information:\*\* Assess whether the website reveals unnecessary details about its CMS, plugins, and server configuration.

2\. \*\*Keep software updated:\*\* Regularly update WordPress, plugins, and themes, and monitor relevant security advisories.

3\. \*\*Review security headers:\*\* Consider implementing appropriate headers such as HSTS, X-Frame-Options, and Content-Security-Policy.

4\. \*\*Review REST API exposure:\*\* Ensure that publicly accessible WordPress REST API endpoints are limited to what the website requires.

5\. \*\*Strengthen email authentication:\*\* Review the SPF policy and consider implementing or reviewing DKIM and DMARC.

6\. \*\*Review DNSSEC configuration:\*\* Consider enabling DNSSEC where supported and appropriate.

7\. \*\*Maintain the WAF:\*\* Keep ModSecurity enabled, updated, and properly configured.

8\. \*\*Monitor local network devices:\*\* Periodically review connected devices and investigate unfamiliar hosts.

9\. \*\*Maintain network documentation:\*\* Keep an updated record of authorized devices, IP addresses, and MAC addresses.

10\. \*\*Follow authorization requirements:\*\* Conduct reconnaissance and scanning only on systems and networks for which permission has been granted.



\---



\## 📊 8. Security Assessment Summary



| Activity | Status |

|---|---|

| 🔎 Reconnaissance and Footprinting | ✅ Completed |

| 🌐 Network Discovery | ✅ Completed |

| 👨‍💻 Host Identification | ✅ Completed |

| 🗺️ Network Topology Generation | ✅ Completed |

| 💥 Exploitation | Not Performed |



\---



\## 🐞 9. Challenges and Learning Experience



During the practical activities, I encountered several challenges and gained useful experience:



\- \*\*Changing IP address:\*\* My PC's IP address changed from `192.168.1.18` to `192.168.1.20` between scans due to DHCP. I learned the importance of recording network configuration and scan results at the same time.

\- \*\*Different host counts:\*\* Scans performed on the same network returned five, six, and seven hosts. This helped me understand that network discovery results represent a snapshot of the devices connected at that particular time.

\- \*\*Unknown MAC address vendors:\*\* Some devices did not return recognizable vendor information. I learned that private or randomized MAC addresses are one possible reason.

\- \*\*Verifying tool output:\*\* WhatWeb reported a Bootstrap version that did not appear valid. This reinforced the importance of cross-checking automated tool results.

\- \*\*Documenting evidence:\*\* I learned the value of capturing clear screenshots and organizing evidence throughout the practical work to support the final technical report.



\---



\## 🔐 10. Security and Ethical Considerations



\- All activities were conducted for educational cybersecurity training.

\- Reconnaissance was performed on the assigned lab target.

\- Network scanning was restricted to my own home network.

\- No exploitation was performed.

\- Findings are documented as observations and should not be treated as confirmed vulnerabilities without further authorized testing.

\- MAC addresses have been partially masked to protect the privacy of devices on the local network.



\---



\## 🔚 11. Conclusion



During Week 2 of my Networkwalks Cybersecurity \& Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance, and network scanning.



Using Kali Linux tools such as `whois`, `whatweb`, `nslookup`, `curl`, `wafw00f`, and `dnsrecon`, I gathered information about the assigned domain, including its registration details, hosting information, IP address, web technologies, WAF, and DNS and mail records.



I also used Zenmap on my own home network to discover live hosts, identify their IP and MAC addresses, and generate a network topology.



One of the most important lessons from this project was understanding how much information can be gathered during the initial stages of a security assessment. I also learned the importance of verifying tool results, documenting evidence, understanding potential risks, and respecting authorization boundaries.



Overall, this practical project strengthened my understanding of reconnaissance and network discovery and gave me an opportunity to apply theoretical cybersecurity concepts in a hands-on environment.



\---



\## 📁 Repository Contents



The repository contains the main technical report, supporting screenshots, and the generated network topology PDF.



| File / Folder | Description |

|---|---|

| `README.md` | Main project report |

| `topology-192.168.1.0-24.pdf` | Network topology generated using Zenmap |

| `images/` | Folder containing the eight practical screenshots listed below |



\### Screenshots



\- `01\_whois.png` – WHOIS lookup results

\- `02\_whatweb.png` – WhatWeb fingerprinting results

\- `03\_nslookup.png` – DNS lookup results

\- `04\_curl.png` – HTTP response headers

\- `05\_wafw00f.png` – WAF detection results

\- `06\_dnsrecon.png` – DNS enumeration results

\- `07\_zenmap\_scan.png` – Zenmap network scan results

\- `08\_topology\_legend.png` – Network topology and legend



\---



\## 📌 Project Information



| Project Details | Information |

|---|---|

| Program | Networkwalks Cybersecurity Program |

| Batch | B084 |

| Week | 02 |

| Project | Footprinting and Network Scanning |

| Repository | GitHub |



\---



\## 👤 Author



\*\*Zamzam Mohamed\*\*  

Cybersecurity Intern | Networkwalks | B084



LinkedIn: \[View my LinkedIn Profile](https://www.linkedin.com/in/zamzam-mohamed-67b840332/)



\*Week 2 – Cybersecurity Practical Project\*

