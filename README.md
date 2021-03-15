DNS Tunnelling with Iodine

What is a DNS?

A DNS (Domain Name System) is a naming system/ phone book that converts human-readable domain names to their individualized IP addresses. For example, if you type the command "nslookup www.whisperlabs.com", you should be able to query your DNS and get specific IP address. 


What is DNS Tunnelling?

DNS tunneling involves using the DNS protocol to send data to a remote machine or another non-DNS server.
There are several legitimate uses of DNS Tunnelling. Some legitimate uses include: 
•	The log-in mechanism used by hotspot security controls at airports or hotels when trying to connect to the internet 
•	DNS tunneling is also commonly used by antiviruses to look up signatures for files when trying to make software updates.


DNS tunneling security attacks 

Because DNS uses Port 53, it is usually left open and unmonitored to transfer DNS queries.
Rather than the more familiar Transmission Control Protocol (TCP) these queries use User Datagram Protocol (UDP) because of its low-latency, bandwidth, and resource usage compared to TCP-equivalent queries. UDP has no error or flow-control capabilities, nor does it have any integrity checking to ensure the data arrived intact. Hackers often use it for malicious purposes.

1.	Command and Control (C2) attacks
2.	Data Exfiltration
3.	Data Infiltration


Recent DNS Tunnelling attacks

In 2018, Cisco Talos Intelligence Group which is one of the world's largest threat intelligence teams recently discovered a new cybersecurity attack targeting Lebanon and the United Arab Emirates (UAE) .gov domains, as well as a private Lebanese airline company. 
This attack used DNS communication and two malicious websites containing fake job postings to send their targets malicious Microsoft Office documents embedded with malicious macros files. 

The macros accomplished two main tasks:
•	When the Microsoft Office documents were opened, the macro would decode a Portable Executable file that would drop it in:
%UserProfile%\.oracleServices\svshost_serv.doc
•	When the document is closed, the macro would rename the file "svshost_serv.doc" to "svshost_serv.exe." This would then create a scheduled task named "chromium updater v 37.5.0" to execute binary. The scheduled task would be executed immediately and repeatedly every minute.

This created an administration tool and when used by the attacker, could temporarily store files before exfiltrating them to the Command-and-Control server.


DNS Tunnelling Tools

Some common DNS tunnelling tools include:
•	Heyoka
•	Nstx
•	Dnscat2
•	Tuns
•	Iodine


Iodine

Iodine is a software tool that was initially released in 2006 lets you tunnel IPv4 data through a DNS server. It can be used in different situations where internet access is firewalled, but DNS queries are allowed.
Iodine runs on Linux, Mac OS, and Windows.


Iodine Offers

Higher performance
Iodine uses the NULL type that allows the downstream data to be sent without encoding. Each DNS reply can contain over a kilobyte of compressed payload data.

Portability
Iodine runs on several UNIX-based systems as well as Windows. Tunnels can be set up between two hosts no matter their endianness or operating system.

Security
Iodine uses challenge-response login secured by MD5 hash. It also filters out any packets not coming from the IP used when logging in.

Less setup
Iodine handles setting IP addresses on interfaces automatically, and up to 16 users can share one server at the same time. Packet sizes are automatically probed for maximum downstream throughput.


How to Use it
For Iodine to work correctly, the server and client are required to use the same protocol. In most cases, this means also means running the same iodine version.

Server-side
To create a tunnel, you would need control over a real domain. For simplicity we use mydomain.com. You will need a server with a public IP address to run iodined on.
Then, delegate a subdomain to the iodined server. for example, we could use t1.mydomain.com
Then restart your nameserver program. Now any DNS queries for domains ending in t1.mydomain.com will be sent to your iodined server.

Start iodined on your server. The first argument is the IP address inside the tunnel, which can be from any range that you don't use yet. (for example 192.168.99.1), and the second argument is the assigned domain (in this case t1.mydomain.com). Using the flag -f, we can keep iodined running in the foreground, which helps when testing. 
Iodined will open a virtual interface ("tun device"), and will also start listening for DNS queries on UDP port 53. Using the -P flag, we would enter a password for our tunnel after the server is started.

 

Now everything is ready for the client.

Client-side
After Iodine has been successfully set up on the client-side, Iodine takes two arguments. 
The first is the local relaying DNS server (optional).
If you don't specify the first argument, the system's current DNS setting will be used. 
The second is the domain you used (t1.mydomain.com) 
The client's tunnel interface will then get an IP address close to the Ip address of the server and a suitable Maximum Transmission Unit(MTU). 
Enter the same password that was used on the server either as a command-line option or after the client has started. Using the -f flag you can keep the iodine client running in the foreground.

This should look like this:

 

After doing this, you should be able to ping the IP address from either end of the tunnel (the server or the client.) 

How to Prevent DNS Tunneling attacks

Network Traffic Analysis
Network traffic analysis (NTA) is the process of intercepting, recording, and analyzing network traffic communication patterns in order to detect and respond to security threats. 
This usually involves using machine learning techniques to verify information
such as the number of requests made, geographic locations, and domain history to separate normal DNS traffic from malicious behavior. 

Payload Analysis
Payload analysis looks at the contents of DNS requests and responses. Factors like the difference between the size of the request versus the response and unusual hostnames can help identify suspicious activity.
Typically consist of a training phase and a detection phase. The training phase is done with clean traffic so that it represents statistically the usual traffic of the system to develop a pattern of the traffic. On the other hand, the detection phase involves modeling and compared these patterns to determine if they can be classified as dangerous or not

References

https://github.com/yarrick/iodine
https://unit42.paloaltonetworks.com/dns-tunneling-how-dns-can-be-abused-by-malicious-actors/
https://code.kryo.se/iodine/
https://www.cloudfare.com/learning/dns/glossary/what-is-my-ip-address/
https://learn-umbrella.cisco.com/solution-briefs/dns-tunneling
https://blog.talosintelligence.com/2018/11/dnspionage-campaign-targets-middle-east.html
