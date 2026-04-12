PwnTillDawn 10.150.150.12

First we need to scan the entire subnetwork to get to the reachable IP address by using ping scan because it is lightweight.

    sudo namp -sn 10.150.150.10/24
![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/446baf15debd32cd09199b48af5b884acce942c8/image/Screenshot%202026-04-12%20200001.png)

Next, we begin by enumerating the open ports on the target machine using a standard Nmap scan. At this stage, it’s useful to also perform service version detection and run common scripts to gather more detailed information about the target.

-sC (Default Scripts) Runs a set of default NSE (Nmap Scripting Engine) scripts Helps identify: Misconfigurations Basic vulnerabilities Service information

-sV (Service Version Detection) Attempts to determine: Service type (e.g., Apache, vsftpd) Version number

-Pn (No Ping) Skips host discovery (no ICMP ping) Treats the target as alive

     nmap -sC -sV -Pn 10.150.150.12

![image alt](


  


