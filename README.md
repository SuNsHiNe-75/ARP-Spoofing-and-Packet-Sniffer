#### Introduction
This is the final project for the course "Theory and Practice of Wireless and Mobile Networks Security," which I developed in collaboration with another classmate.

- OS Environment: **KALI Linux**.
- Packages used: **nmap** (for finding local network IP).
- Implementation language: **Python**.

#### ARP Spoofing
The implementation of ARP spoofing is mainly done through the scapy library. First, to deceive the victim's computer into sending packets to the attacker's computer instead of the router, we use the `ARP()` function. Inside this function, we specify the victim's IP and MAC address, and change the source IP address to the router's IP. This way, when the victim's computer wants to send requests to the Internet, the packet intended for the router is redirected to us due to the forged ARP packet we sent. Additionally, we perform ARP spoofing on the router by specifying the router's IP and MAC address, and changing the source IP address to the victim's IP, making the router think that the victim's IP corresponds to the attacker's MAC address, thus allowing us to intercept packets and conduct man-in-the-middle attacks, such as packet analysis.

#### Packet Sniffer
Through ARP Spoofing, we have redirected the victim's packets to our hacker's computer. Therefore, we need to implement a "Packet Sniffer" to analyze the packets and extract the data and content of interest.

Using the built-in "`sniff`" function in scapy, we capture packets targeting the interface (`eth0`) connected to the Internet in Kali. We first attempt to print the packet contents, but find them scrambled. The solution is to introduce the `scapy_http` library and use the function `packet.haslayer(http.HTTPRequest)` to filter out HTTP packets (which are unencrypted). After observing the filtered content, we further filter out the information in the `[Raw]` layer's `[load]` field (e.g., input from a test website where usernames and passwords are uploaded in "`POST`" form and appear in this layer and its subfields). Next, we try to "build a keyword table" to filter out the content we want to see in the field (such as usernames, passwords, etc.) to avoid unwanted information. Finally, we filter and capture the "`Host`" and "`Path`" of the visited website using the same method and print them out. The Packet Sniffer is completed.
