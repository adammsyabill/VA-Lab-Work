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
