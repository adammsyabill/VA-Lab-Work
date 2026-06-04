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


