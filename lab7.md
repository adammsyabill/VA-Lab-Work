Lab 7

Q1

Since the pcap file contained a relatively small number of packets, I perform the analysis manually rather than using automated tools.

Upon reviewing the packets, i noticed one particular packet stood out it size was noticeably different from all the others. This anomaly made it a strong candidate for further investigation.

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/dcacd3790910ca734f2d8be028d679243d979b4c/image/Screenshot%202026-06-04%20233448.png)

After isolating the suspicious packet and inspecting its data field, I observed that the content appeared to be encoded. Recognising the pattern, I determined it was Base64 encoding.

To decode it, I used CyberChef a free, browser based tool commonly used in CTF challenges for data transformations. I applied the "From Base64" operation, and the output revealed the flag directly.

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/a83cf594359110a37b1f669b3428e55d21be8b5c/image/Screenshot%202026-06-04%20234509.png)

Answer: SUCTF2023{ai_is_cool}

Q2

After filtering out the ICMP packets which all shared the same file size I focused exclusively on the TCP traffic for deeper analysis.

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/30106aa6967d63e82eddd015f2f45769062d8532/image/Screenshot%202026-06-04%20235353.png)

Examining the TCP stream revealed a successful file transfer. The transferred file was named global_thermonuclear_war.gamerules.txt. Inside the file was a link pointing to a Google Doc titled "Club Tux," which contained a series of unusual symbols that didnt immediately make sense.

![image alt](https://github.com/adammsyabill/VA-LabWork/blob/7da49e7eb6d4947e7d90102c897950f447f2b320/image/Screenshot%202026-06-04%20235850.png)

To identify the cipher, I searched online and found that the symbol set matched the Tic-Tac-Toe Cipher (also known as the Pigpen Cipher variant). I used the dCode website, which has a dedicated Tic-Tac-Toe Cipher decoder tool, and entered the symbols found in the document.

The decoder returned the plaintext, revealing the flag.

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/7572e97736a69b5b0b5543bcf35e4732807aa2a6/image/Screenshot%202026-06-05%20000121.png)

Answer: EXMACHINAAVA

Q3: Interpret an Nmap Output

POST STATE VERSION 2.1/tcp open ftp vsftpd 2.3.4 22/tcp open ssh OpenSSH 5.3p1 80/tcp open http Apache 2.2.8 138/tcp open netbios-smb 445/tcp open microsoft-ds Windows 7 Professional 7601 Service Pack 1

Questions:
1. What can an attacker do with each port?
2. What vulnerabilities are likely present based on the version?

For Port 21 (FTP), an attacker could potentially exploit it to install a backdoor or gain unauthorised access. Beyond that, FTP transmits credentials in plaintext, meaning a network sniffer can easily intercept usernames and passwords. Older versions are known to carry additional vulnerabilities, making this port a prime target for persistent access.

For Port 80 (HTTP), the server is running an outdated version of Apache, which is susceptible to a range of web-based attacks including SQL injection, file inclusion, and service misconfigurations. If exploited successfully, these vulnerabilities can give an attacker a foothold into the underlying system.

Port 138 (NetBIOS) supports Windows file-sharing features and legacy network services. It can be leveraged to silently gather internal system information — such as usernames, shared resources, and system details — without requiring authentication in certain scenarios. This makes it highly valuable for reconnaissance, as it doesn't disrupt the system but provides meaningful intelligence that enhances the effectiveness of further attacks.

For Port 445 (SMB), the Server Message Block service is common in Windows environments. Attackers can enumerate users, access shared folders, and attempt password attacks. SMB has a well-documented history of serious vulnerabilities, including those that enable remote code execution. If exploited, an attacker can achieve full control over the system and potentially propagate the attack across the network.

Questions:
1. Which port carries the highest risk and why?
Port 21 is considered the most dangerous because it allows unauthenticated remote exploitation with minimal complexity. The presence of a known backdoor substantially increases the severity, resulting in a full system compromise affecting confidentiality, integrity, and availability. This vulnerability carries a CVSS score of 9.8 (Critical).

3. What attack path can be built from this?
   
A likely attack chain would proceed as follows:

Step 1: The attacker begins by targeting Port 21 running vsftpd 2.3.4. Due to the known backdoor, they can gain immediate shell access without authentication, providing a reliable entry point into the system.

Step 2: From there, Port 80 (HTTP) running Apache 2.2.8 can be explored next. The attacker analyses the web application for vulnerabilities such as file uploads, command injection, or other weaknesses that could serve as additional escalation vectors.

Step 3: Once access is established, local enumeration begins — gathering information such as user accounts, running services, and network configurations. This helps identify additional vectors and possible privilege escalation paths.

Step 4: The attacker then pivots to Ports 138 and 445 (NetBIOS/SMB) to query the Server Message Block service. From the compromised machine, shared folders can be enumerated, transfer files extracted, and collected usernames tested. Weak or misconfigured shared credentials may allow the attacker to move laterally to other machines on the network.

Step 5: Using the gathered credentials, the attacker targets Port 22 (SSH) via OpenSSH. A brute-force or credential reuse attempt may succeed, allowing the attacker to obtain legitimate login access. This enables the establishment of a more stable and persistent presence compared to the initial exploit.

Step 6: Finally, the attacker consolidates control by escalating privileges (e.g., to root or Administrator), maintaining persistence, and using the compromised system as a launchpad for lateral movement or further network attacks.

3. What should the remediation be?
   
FTP — vsftpd 2.3.4

This version must be removed or replaced immediately as it contains a known backdoor. If FTP cannot be replaced, install a patched and updated version. Disable anonymous login and restrict access using firewall rules. This is the highest priority fix.

SSH — OpenSSH 5.3p1
The OpenSSH service should be hardened to prevent unauthorised access. Update to the latest version, disable password authentication in favour of SSH keys, enforce strong credential and key management policies, change the default port, reduce login limits, and disable root login.

HTTP — Apache 2.2.8
The web server is outdated and vulnerable. Upgrade to a supported version of Apache, apply all security patches, remove unnecessary modules, implement a Web Application Firewall (WAF), secure cookie flags, configure proper SQL injection filters, and close version information exposure.

NetBIOS — Port 138
This legacy service should be restricted or disabled entirely. Disable NetBIOS if not required, block external access via firewall, prevent null session enumeration, and limit sharing to trusted internal networks only.

SMB — Port 445
The Server Message Block service is highly sensitive and must be secured. Disable SMBv1, apply all Windows security patches, enforce strong authentication, restrict access to internal networks only, disable unnecessary shares, enable detailed logging, and monitor for suspicious activity.

Question 4: Identify the OS — TTL=64

TTL stands for Time to Live, a value in the IP header that limits how many hops a packet can travel across a network. It decreases by one at each hop. A TTL value close to 64 typically suggests the packet originated from a Linux or Unix-based system, as 64 is a common default starting value. By observing how much the TTL has decreased, analysts can estimate the number of hops between the source and destination. TTL analysis also helps prevent packets from looping indefinitely. While it can support OS fingerprinting and network analysis, TTL values alone are not definitive proof of OS identity due to the possibility of manipulation or variance in configurations.

Question 5: TTL=255
A TTL of 255 suggests the packet was initialised with a high hop limit, commonly used by network devices such as routers and switches. As the packet traverses the network, TTL decreases by one at each hop. Observing a TTL close to 255 typically indicates the packet source is either a network infrastructure device or a host located very close within the same network. This high initial value allows packets to travel across large networks while also being used in certain protocols for validation and security purposes.

Question 6: TTL=128
A TTL of 128 most likely indicates the packet was generated by a Windows operating system, as 128 is the default initial TTL value used by Windows hosts. The TTL field decreases by one at each hop as the packet traverses the network, helping to prevent routing loops and ensure packets reach their destination. Analysing TTL values supports network analysis, OS fingerprinting, and helps inform decisions about network architecture and security mechanisms. As with other TTL values, 128 alone is not definitive for OS identification due to the potential for manipulation by network devices or security mechanisms.
