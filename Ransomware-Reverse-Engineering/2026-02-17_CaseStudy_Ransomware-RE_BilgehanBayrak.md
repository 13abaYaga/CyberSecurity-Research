# Case Study: Deep Analysis of WannaCry Attack and SMBv1 Vulnerability

---

## Customer Profile

**Industry**: Healthcare Sector
**Organization Size**: 5,000+ employees, Geographically Distributed Multi-Hospital Network
**Environment**: The organization's infrastructure is heavily reliant on legacy Windows systems (Win 7/XP) and unpatched servers due to strict compliance rules and legacy software dependencies. These systems manage critical medical equipment, including MRI machines and patient monitors. The network architecture is largely flat, lacking proper segmentation between critical medical devices and administrative workstations.

---

## The Challenge

The customer reported a critical incident where computers across the entire hospital network suddenly locked up on a Friday afternoon. Screens displayed a menacing red ransom note ("Oops, your files have been encrypted!"), demanding payment to decrypt files. Emergency services came to an immediate halt, radiology imaging systems became unusable, and access to vital patient records was completely cut off.

**Rapid Propagation**: The infection started from a single compromised machine and spread to the entire network and branch offices within minutes via VPN tunnels, leveraging worm-like capabilities to move laterally without human intervention.
**Critical Systems**: The attack devastated not only administrative IT systems but also Windows-based embedded systems. Critical patient care devices, such as MRI machines and patient monitoring units, were rendered inoperable.
**The Critical Question**: The CISO urgently needed to answer: "How is this worm spreading so fast across our network, and what can we do to stop it immediately without physically disconnecting every machine and disrupting critical care?"

---

## Engagement Scope

**Starting Position**: An infected, isolated Windows 7 terminal and network traffic logs (PCAP) captured during the initial outbreak were received for forensic analysis.
**Objective**: To conduct a deep-dive analysis of the malware's propagation mechanism (worm behavior) and identify any potential weaknesses or an immediate stop mechanism (killswitch) through reverse engineering.
**Constraint**: Intervention must minimize downtime to ensure the continuity of critical healthcare services, meaning a complete network shutdown was not a viable option.

---

## Adversary Tactics & Technical Analysis

Our reverse engineering of the recovered binary revealed a sophisticated multi-stage execution model designed to ensure persistence and maximum propagation before detection.

### 1. Persistence & Service Creation
Upon execution, the malware installs itself as a service named `mssecsvc2.0` (Microsoft Security Center (2.0) Service) to masquerade as a legitimate system process. This service is configured to start automatically, ensuring it runs even after a reboot.

- **Dropped File**: `C:\WINDOWS\tasksche.exe` (The encryptor payload)
- **Service Name**: `mssecsvc2.0` (The worm propagator)
- **Mutex**: `Global\MsWinZonesCacheCounterMutexA` (Used to prevent multiple infections on the same host)

### 2. AV Evasion & Exploitation Techniques
The malware evaded the organization's legacy antivirus solutions through several techniques:
- **Trusted Protocol Abuse**: By leveraging SMB (Server Message Block), it utilized a protocol essential for file sharing which was whitelisted in internal firewalls.
- **In-Memory Injection**: The EternalBlue exploit injects shellcode directly into the kernel's non-paged pool memory, executing with SYSTEM privileges without initially dropping a binary to disk for the AV to scan.
- **Service Masquerading**: Naming the service `mssecsvc2.0` tricked basic monitoring tools that only look for distinctively suspicious names.

### 3. Lateral Movement Mechanism
The worm module contains two distinct scanning threads:
- **Local Network**: It calculates the subnet mask of the infected machine and scans every private IP in the range (e.g., `192.168.1.x`) for open Port 445.
- **Public Internet**: It generates random public IP addresses to spread globally.

---

## Key Findings

### Finding 1: EternalBlue Exploit (MS17-010) and SMBv1

Detailed network traffic (Wireshark) and memory analysis revealed that the malware was aggressively spreading via port 445 (SMB). The malware utilized the **EternalBlue** exploit, a weaponized tool leaked by the NSA, to trigger a specific buffer overflow vulnerability in the SMBv1 service on target systems. This allowed the attackers to achieve unauthorized Remote Code Execution (RCE) with SYSTEM privileges.

![Figure 1: Network analysis of EternalBlue (SMB) traffic in Wireshark](./images/Ransomware-RE_01_Wireshark.png)

**Vulnerability**: Windows SMBv1 Remote Code Execution (CVE-2017-0144).
**Exploitation**: Corrupting the target's kernel pool via specially crafted, malformed SMB packets.
**Impact**: Rapid, automated compromise of all unpatched Windows systems on the network without any user interaction.
**Detection Status**: Existing IPS systems failed to detect and block the attack due to a lack of updated signatures for this specific exploit.

### Finding 2: The Killswitch

During static analysis (IDA Pro), a peculiar and unexpected HTTP request was observed before the malware's potentially destructive actions ensued. At the very beginning of the code execution, it attempted to connect to `http://www[.]iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea[.]com`. The logic revealed a simple check: If it could successfully connect to this address (meaning the domain is registered and active), the malware would terminate itself (Exit), assuming it was in a sandbox. If it could not connect, it would proceed to encrypt files and spread.

![Figure 2: Killswitch control mechanism and URL in IDA Pro](./images/Ransomware-RE_02_Killswitch_IDA.png)

**Vulnerability**: A simple logical flaw intended by the authors as an Anti-Sandbox technique to evade analysis.
**Exploitation**: Registering this unregistered domain name (DNS Sinkhole).
**Impact**: Instant cessation of the global spread of the malware as instances worldwide connected to the domain and shut down.
**Detection Status**: The HTTP GET request was observed in proxy logs but was not flagged as malicious by threat intelligence feeds at the time.

### Finding 3: WanaDecrypt0r Encryption

The malware used AES-128 (CBC mode) to encrypt user files, generating a unique, random key for each file. These keys were then encrypted with a hardcoded RSA-2048 public key and appended to the file header.

![Figure 3: Encryption routines found in the code](./images/Ransomware-RE_03_Encryption.png)

The user interface, displaying the countdown timer and payment instructions, operated via `tasksche.exe`.

![Figure 4: WanaDecrypt0r ransom note user interface](./images/Ransomware-RE_04_Ransom_Note.png)

**Vulnerability**: Use of strong, correctly implemented cryptography (No implementation vulnerability found to allow free decryption).
**Exploitation**: Appending .WNCRY extension to encrypted files.
**Impact**: Data becoming permanently inaccessible and operational capability halting completely.

### Security Gap Analysis: Why Defenses Failed

From a cybersecurity consultancy perspective, the rapid spread was not just due to the malware's sophistication, but primarily due to fundamental architectural and configuration gaps.

| Security Layer | Implemented Control | Failure Mode |
| :--- | :--- | :--- |
| **Perimeter Firewall** | Port 445 Blocked (External) | **Failed**: The infection likely started from a VPN user or a single breached endpoint, bypassing the edge firewall. |
| **Internal Firewall** | None (Flat Network) | **Critical Failure**: Once inside, there was no segmentation (VLANs/ACLs) to stop the worm from reaching the MRI machines. |
| **Endpoint Protection** | Legacy Antivirus (Signature-based) | **Failed**: The AV database was outdated, and the "EternalBlue" in-memory exploit does not trigger file-based scanning. |
| **Patch Management** | Manual & Delayed | **Root Cause**: The patch (MS17-010) was released 2 months prior but not applied to critical systems due to "uptime" concerns. |

**Consultant's Verdict**: The organization relied too heavily on "Perimeter Security" (Eggshell defense). The lack of **Defense-in-Depth** (Internal Segmentation + Virtual Patching) allowed a single compromised laptop to take down the entire hospital network.

### Kill Chain & Lateral Movement Scenarios

We analyzed two scenarios to demonstrate the impact of network segmentation.

**Scenario A: The Reality (Flat Network)**
1.  **Initial Access**: Automated exploit attempt via exposed SMB (Port 445) services.
2.  **Propagation**: The main "Worm" thread scans the local subnet (`/24`) and immediately identifies nearby MRI machines and servers.
3.  **Lateral Movement**: Within milliseconds, exploits are launched. 50 devices are infected in under 1 minute.
4.  **Action on Objectives**: `tasksche.exe` executes, encrypting patient DBs.

**Scenario B: The "What If" (Segmented Network)**
1.  **Initial Access**: Same laptop is infected.
2.  **Propagation**: The worm scans for Port 445.
3.  **Containment**: Switches (VLAN ACLs) BLOCK traffic between "Admin Workstations" and "Medical Devices".
4.  **Outcome**: Infection is limited to 1 VLAN (Finance Dept). Medical services continue uninterrupted.

---

## Outcome

**Immediate Remediation**:
-   The identified Killswitch domain was immediately registered (costing only $10.69), and all traffic was redirected to a controlled sinkhole server. This action effectively stopped the malware's spread globally.
-   Port 445 was blocked from the external world on all edge Firewalls to prevent further infiltration.

**Strategic Improvements**:
-   The critical MS17-010 security patch was urgently tested and applied to all systems across the organization.
-   Legacy Windows XP/7 systems were segmented into isolated VLANs, and their internet access was strictly restricted.
-   The insecure SMBv1 protocol was disabled across the entire network via Group Policy Object (GPO).

---

## Business Impact

Thanks to the swift discovery of the Killswitch and the activation of the domain, the encryption of another 2,500 machines on the customer's network was prevented. This simple yet critical reverse engineering finding prevented the total collapse of hospital operations, averting millions of dollars in damages, potential loss of life, and irreversible reputational damage. The board subsequently recognized patch management as a critical business continuity process rather than a mere IT task.

---

## Conclusion

The WannaCry case painfully reminded the industry of the critical importance of "Patch Management" and "Network Segmentation". Furthermore, by demonstrating how a massive, global cyber attack could be stopped with a simple string analysis (Killswitch URL), it definitively proved the immense value of Static Analysis and Reverse Engineering in modern Incident Response processes.
