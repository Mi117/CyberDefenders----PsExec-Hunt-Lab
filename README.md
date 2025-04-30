# CyberDefenders----PsExec-Hunt-Lab
CyberDefenders — PsExec hunt Lab Walkthrough

INTRO

The CyberDefenders "PsExec Hunt" challenge focuses on the detection and analysis of malicious PsExec activity within an enterprise environment. This walkthoutgh brings to attention usage of PsExec, and how attackers can weaponize this tool for lateral movement. 

SCENARIO

An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.

TOOLS USED

- WIRESHARK [[https://www.wireshark.org/]: network protocol analyzer.

WALKTHROUGH

Q1) To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

Firstly I started by analyzing the traffic and the entieties involved and I could clearly see that there was an abnormal amount of data packets exchanged, coming from the IP address 10.0.0.130 , yet I couldn't really tell if it was the Ip address from which the attacker gained the initial access.

![q1](https://github.com/user-attachments/assets/d0fa7cee-63d4-40af-82cf-3353c7abcadf)

I have then perfomed a research on the PsExec, finding that is a command-line tool from Microsoft's Sysinternal that allows administrators to execute processes on remote systems, and it relies on the SMB _(Server Message Block)_protocol - another important clue to lok for in our .pcap file.

![q1-02](https://github.com/user-attachments/assets/416414a8-a420-48c1-bdcf-3b8010a5ed55)

I then went on by isolating the traffic coming from the suspicious IP address 10.0.0.130, and could clearly see the initiation and completion of the TCP three-way-handshake, signalling that a communication channel was established.

![q1-2](https://github.com/user-attachments/assets/fd3b61c4-31bd-47bb-8940-14e0a8ceb34d)

This definitely confirmed my suspect and confirmed that the attacker IP address is indeed 10.0.0.130.

Q2) To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?

By following the TCP stream of the previous packet (n. 127) we can see the first machine's hostname to which the attacker first pivoted - SALES-PC

![q2](https://github.com/user-attachments/assets/9cf00dd8-fd0b-4531-91df-3794a96e9ff6)

Q3) Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

Following suit with the traffic analysis from the attacker IP, we can clearly see that packet n. 132 contains the username used to authenticate, **ssales**

![q3](https://github.com/user-attachments/assets/d70b249a-3935-4ab7-aacd-7a72a732a55d)

Q4) After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?

To determine the name of the service executable the attacker set up on the target, we can check the packets number of 144 and 145. There is a suspicious file that indicate this malicious activity in this network.

![q4](https://github.com/user-attachments/assets/f54ab8ec-d9c3-41a2-ad17-aaeeadb210a1)

Q5) We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?

To determine Which network share was used by PsExec to install the service on the target machine, we can check the packets number of 138. There is a endpoint that refer to network share.

![q5](https://github.com/user-attachments/assets/3f761c13-4933-4a09-a460-ab665a7a776b)

Q6) We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?

To determine Which network share did PsExec use for communication, we can check the packets number of 134. There is a endpoint that refer to network share. 

![q6](https://github.com/user-attachments/assets/e58ab1e1-5241-475e-90d7-ef3804fba4ad)

Q7) Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?

For answering this particula question I have use the filter **dns.qry.name** : this filter allows to focus the analysis on DNS traffic related to partivular domains or keywords or, in our case, other hosts, as out attacker is attempting to perform lateral movements within the network. When an attacker attempts lateral movement within a network, they often need to discover other **hosts**. A common technique is to perform DNS queries to resolve hostnames of potential targets.
This gives us the name of the other host to which the attacker pivoted his focus: **marketing-PC**.

![q8](https://github.com/user-attachments/assets/df6ae352-f2ed-4984-a2b1-0a395198c3ab)

CONCLUSIONS

Completing the "PsExec Hunt" challenge expanded my capabilities in detecting lateral movement techniques and analyzing their forensic trails. By examining Windows event logs, network traffic, and system artifacts, I successfully traced the attacker's path through the network and reconstructed their methodology. This exercise reinforced the critical importance of comprehensive logging and proactive threat hunting, while developing practical skills in Windows forensics and attack pattern recognition that directly translate to real-world incident response.

I hope you found this walkthrough valuable! If this content helped you, please consider showing your support. Your feedback is invaluable and motivates me to continue supporting your journey in the cybersecurity community. Remember, LET'S ALL BE MORE SECURE TOGETHER! For more insights and updates on cybersecurity analysis, follow me on Substack! [https://substack.com/@atlasprotect?r=1f5xo4&utm_campaign=profile&utm_medium=profile-page]
