# Authorized-Phishing-Simulation-SOC-Lab
An authorized cybersecurity lab documenting phishing awareness simulation, network validation, HTTP logging, and SOC investigation concepts.

 Objective:
The objective of this lab is to simulate a controlled phishing-awareness scenario and observe the network and HTTP activity generated during the test.

The lab focuses on:
- Building a controlled web page in Kali Linux
- Accessing the page from a test Android device
- Capturing the resulting HTTP activity
- Examining the activity from a SOC analyst perspective
- Documenting the findings for future SIEM and detection development


  LAB ENVIRONMENT :
 Kali Linux running in VirtualBox
- Android device used as the controlled test client
- Python HTTP server
- Local network using bridged networking
- Test web page hosted on Kali Linux


-  NETWORK CONFIGURATION:
The Kali machine was configured using bridged networking so that the controlled Android test device could communicate with it directly.
The following output shows the IP address assigned to the Kali machine:

kali ip and route png

┌──(kali㉿kali)-[~]
└─$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether xx:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet 192.168.43.7/24 brd 192.168.43.255 scope global dynamic noprefixroute eth0
       valid_lft XXXXsec preferred_lft XXXXsec
    inet6 XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
                                                                                                                   
┌──(kali㉿kali)-[~]
└─$ ip route
default via 192.168.43.33 dev eth0 proto dhcp src 192.168.43.7 metric 100 
192.168.43.0/24 dev eth0 proto kernel scope link src 192.168.43.7 metric 100


 LAB SETUP:
A dedicated `phishing-lab` directory was created on the Kali machine to contain the files used for the controlled simulation.
A simple test web page was created and hosted locally using Python's built-in HTTP server. The Android device was then used as the controlled test client to access the page through Kali's IP address.


TEST WEB PAGE:
A simple security-awareness test page was created inside the `phishing-lab` directory. The page was intentionally kept non-credential-collecting and was used only to verify controlled client-to-server communication.

The page displayed:
> Security Awareness Simulation

Client Access Evidence:
The controlled Android test device successfully accessed the web page hosted by the Kali machine through `192.168.43.7:8080`.


HTTP SERVER & NETWORK ACTIVITY:
The test page was hosted using Python's built-in HTTP server on port 8080.

The Android test device accessed the page successfully, generating an HTTP GET request that was recorded by the server.

The server returned HTTP status code `200`, confirming that the requested page was successfully delivered.

The server also recorded a request for `/favicon.ico`, which returned `404` because no favicon file was present. This was expected and did not affect the successful page request.


┌──(kali㉿kali)-[~/phishing-lab]
└─$ python3 -m http.server 8080
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
192.168.43.33 - - [27/Sep/2026 17:49:30] "GET / HTTP/1.1" 200 -
192.168.43.33 - - [27/Sep/2026 17:49:32] code 404, message File not found
192.168.43.33 - - [27/Sep/2026 17:49:32] "GET /favicon.ico HTTP/1.1" 404 -
^C
Keyboard interrupt received, exiting.


SOC PERSPECTIVE:
From a SOC perspective, this lab demonstrates how user activity can generate network and application-layer evidence.
The HTTP server log provides useful evidence such as the source address, requested resource, timestamp, HTTP method, and response status code.
In a larger environment, similar events could be collected by a SIEM and correlated with other security telemetry to support detection and investigation.


LIMITATIONS:
This was a controlled lab using a test device and a non-credential-collecting web page.
The initial attempt to use an external tunneling service was unsuccessful, so the exercise was continued entirely within the controlled local network.

No real credentials or third-party accounts were used.


FUTURE DEVELOPMENT:
Future versions of this project will expand the lab into a more complete SOC investigation workflow, including:
- SIEM log collection
- Detection rules
- Alert generation
- Event correlation
- Investigation and triage
- Incident response documentation
- Security awareness analysis



ETHICAL CONSIDERATION:
This project was conducted in an authorized lab environment using my own test equipment.
The simulation was designed for security learning and awareness purposes. No real users, credentials, or third-party accounts were targeted.






 


