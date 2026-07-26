# Internet Protocol Troubleshooting Commands Lab Report

## Objectives
After completing this lab, I was able to:
- Practice troubleshooting commands
- Identify how these commands can be used in customer scenarios

---

## Part 1: Network Layer (Layer 3) Commands

### Task 21: Using the ping Command

**Scenario:**
The customer had launched an EC2 instance. To test connectivity to and from it, I ran the ping command to test connectivity and ensure that it allowed Internet Control Message Protocol (ICMP) requests on the security level, such as security groups and network ACLs.

**Command Executed:**
```bash
ping 8.8.8.8 -c 5
```
The **-c** option stands for count, and **5** specifies how many requests to send.

**Results Obtained:**

<img width="658" height="242" alt="Screenshot 2026-07-26 070826" src="https://github.com/user-attachments/assets/1141aacf-7af4-4b92-8e68-3546c0e341bf" />


```
[ec2-user@ip-10-0-10-78 ~]$ ping 8.8.8.8 -c 5
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=116 time=5.80 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=116 time=5.80 ms
64 bytes from 8.8.4.8: icmp_seq=3 ttl=116 time=5.84 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=116 time=5.79 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=116 time=6.02 ms

--- 8.8.8.8 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 5.797/5.853/6.024/0.110 ms
[ec2-user@ip-10-0-10-78 ~]$
```

**What I Learned:**
The ping command sent ICMP echo requests from my machine to the target server (8.8.8.8). The server responded with echo replies showing round-trip times. The results showed 0% packet loss with average response times around 5.85ms, indicating good connectivity to the target.

---

### Task 22: Using the traceroute Command

**Scenario:**
The customer was experiencing latency issues and packet loss. They were unsure if it was related to AWS or their internet service provider (ISP). I ran the traceroute command from their AWS resource to the server they were trying to reach to investigate.

**Command Executed:**
```bash
traceroute 8.8.8.8
```

**Results Obtained:**

<img width="837" height="418" alt="Screenshot 2026-07-26 070920" src="https://github.com/user-attachments/assets/3b659238-d8fd-480a-9f98-26a5494c6209" />


```
[ec2-user@ip-10-0-10-78 ~]$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
1 244.5.1.43 (244.5.1.43) 1.262 ms 244.5.1.229 (244.5.1.229) 57.540 ms 244.5.1.239 (244.5.1.239) 2.280 ms
2 240.4.228.3 (240.4.228.3) 0.378 ms 240.4.228.6 (240.4.228.6) 0.433 ms 240.4.228.3 (240.4.228.3) 0.369 ms
3 242.13.182.65 (242.13.182.65) 0.361 ms 242.13.182.195 (242.13.182.195) 0.9 ms
4 240.4.12.32 (240.4.12.32) 7.814 ms 240.4.12.9 (240.4.12.9) 7.658 ms 240.4.12.32 (240.4.12.32) 8.302 ms
5 242.11.43.135 (242.11.43.135) 12.032 ms 242.11.43.1 (242.11.43.1) 7.562 ms
6 240.1.228.8 (240.1.228.8) 6.400 ms 240.1.228.1 (240.1.228.1) 6.956 ms 240.1.228.3 (240.1.228.3) 6.413 ms
7 * 99.83.116.81 (99.83.116.81) 6.150 ms 99.83.117.223 (99.83.117.223) 6.055 ms
8 108.170.255.179 (108.170.255.179) 7.364 ms 142.251.61.155 (142.251.61.155) 7.238 ms *
9 216.239.56.223 (216.239.56.223) 5.895 ms dns.google (8.8.8.8) 6.074 ms 5.997 ms
[ec2-user@ip-10-0-10-78 ~]$
```

**What I Learned:**
The traceroute command reported the path and latency that packets take from my machine to the destination. Each server along the path is called a "hop." I observed that hop 7 showed an asterisk (*), indicating a failed hop. However, subsequent hops continued successfully to the destination. The packet loss at intermediate hops can occur due to LAN or ISP issues, while loss toward the end of the route typically indicates server connection problems.

---

## Part 2: Transport Layer (Layer 4) Commands

### Task 23: Using the netstat Command

**Scenario:**
The company was running a routine security scan and found that one of the ports on a certain subnet was compromised. I ran the netstat command on a local host on that subnet to confirm if the port was listening when it shouldn't be.

**Command Executed:**
```bash
netstat -tp
```

**Results Obtained:**

<img width="1002" height="115" alt="Screenshot 2026-07-26 070950" src="https://github.com/user-attachments/assets/fc6e9760-fcf2-4ee9-8f16-3924c8667d08" />


```
[ec2-user@ip-10-0-0-10-78 ~]$ netstat -tp
(No info could be read for "-p": geteuid()=1000 but you should be root.)
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address    Foreign Address    State    PID/Program name
tcp    0    64 ip-10-0-10-78.us-wei:ssh 165-255-10-110.ip:52975 ESTABLISHED -
[ec2-user@ip-10-0-0-10-78 ~]$
```

**What I Learned:**
The netstat command showed the current established TCP connections that the host was listening on. I noted that the `-tp` option requires root privileges to display PID/Program names, which is why the message about `geteuid()` appeared. Despite this, I could still see established connections. Additional useful variations include:
- `netstat -tlp`: Outputs listening services
- `netstat -ntlp`: Outputs listening services without resolving port numbers

This command provides a snapshot of Layer 4 connectivity, helping narrow down network issues efficiently.

---

### Task 24: Using the telnet Command

**Scenario:**
The customer had a secure web server with custom security group rules and network ACL rules configured. They were concerned that port 80 was open even though their security settings indicated that their security group was blocking this port. I ran telnet to ensure that the connection was refused.

**Command Executed (Installation):**
```bash
sudo yum install telnet -y
```

**Command Executed (Connection Test):**
```bash
telnet www.google.com 80
```

**Results Obtained:**

<img width="1156" height="785" alt="Screenshot 2026-07-26 071225" src="https://github.com/user-attachments/assets/a2c280f7-c62b-4d80-8b87-b796ec934e67" />
 - Installation output
```
[ec2-user@ip-10-0-10-78 ~]$ sudo yum install telnet -y
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core
Resolving Dependencies
--> Running transaction check
---> Package telnet.x86_64 1:0.17-65.amzn2 will be installed
---> Finished Dependency Resolution

Dependencies Resolved

---
Package    Arch    Version    Repository    Size
---
Installing:
telnet    x86_64    1:0.17-65.amzn2    amzn2-core    64 k

Transaction Summary
---
Install 1 Package

Total download size: 64 k
Installed size: 109 k
Downloading packages:
telnet-0.17-65.amzn2.x86_64.rpm    |  64 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : 1:telnet-0.17-65.amzn2.x86_64    1/1
  Verifying : 1:telnet-0.17-65.amzn2.x86_64   1/1

Installed:
telnet.x86_64 1:0.17-65.amzn2
```

<img width="597" height="83" alt="Screenshot 2026-07-26 071247" src="https://github.com/user-attachments/assets/5d473aeb-8c88-4a4c-b3d9-f8c0493de971" />
 - Telnet connection test
```
[ec2-user@ip-10-0-10-78 ~]$ telnet www.google.com 80
Trying 142.251.153.119...
Connected to www.google.com.
Escape character is '^]'.
```

**What I Learned:**
The telnet command confirmed the TCP connection to a web server. I successfully connected to www.google.com on port 80, indicating that there was nothing blocking the connection. The successful connection shows:
- If connection succeeds: Nothing is blocking the connection
- If "connection refused": A firewall or security group is likely blocking it
- If "connection timed out": There is no network route or connectivity

---

## Part 3: Application Layer (Layer 7) Commands

### Task 25: Using the curl Command

**Scenario:**
The customer had an Apache server running and wanted to test if they were getting a successful request (200 OK), indicating that their website was running successfully. I ran a curl request to see if the customer's Apache server returned a 200 OK.

**Command Executed:**
```bash
curl -vlo /dev/null https://aws.com
```

**Results Obtained:**

<img width="1152" height="687" alt="Screenshot 2026-07-26 071426" src="https://github.com/user-attachments/assets/2629122f-2cdb-44fd-9fa3-cbf56cee0fd6" />


```
[ec2-user@ip-10-0-10-78 ~]$ curl -vlo /dev/null https://aws.com

%Total   %Received %Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
Trying 3.169.173.19:443.
* Connected to aws.com (3.169.173.19) port 443
ALPN: curl offers h2,http/1.1
Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!ANULL:!LOW:!RC4:@STRENGTH
[5 bytes data]
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
[512 bytes data]
* CAfile: /etc/pki/tls/certs/ca-bundle.crt
* CPath: none
[5 bytes data]
* TLSv1.2 (IN), TLS handshake, Server hello (2):
[100 bytes data]
* TLSv1.2 (IN), TLS handshake, Certificate (11):
[3786 bytes data]
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
[333 bytes data]
* TLSv1.2 (IN), TLS handshake, Server finished (14):
[4 bytes data]
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
[70 bytes data]
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
[1 bytes data]
* TLSv1.2 (OUT), TLS handshake, Finished (20):
[16 bytes data]
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
[1 bytes data]
* TLSv1.2 (IN), TLS handshake, Finished (20):
[16 bytes data]
* SSL connection using TLSv1.2/ECDHE-RSA-AES128-GCM-SHA256
ALPN: server accepted h2
Server certificate:
```

**What I Learned:**
The curl command transferred data between my machine and the server using HTTPS. The command options I used:
- `-v`: Verbose mode showing what the computer was doing during startup
- `-l`: Header information
- `-o /dev/null`: Sent HTML and CSS responses to null
- `https://aws.com`: The target URL

I could see the TLS handshake process (Client hello, Server hello, Certificate exchange) and the successful SSL connection using TLSv1.2. The curl command is useful for troubleshooting communication from a local device to a server and testing HTTP/HTTPS connectivity.

---

## Summary

Throughout this lab, I practiced using five essential network troubleshooting commands across different OSI layers:

| Layer | Command | Primary Use |
|-------|---------|-------------|
| Layer 3 (Network) | `ping` | Test basic IP connectivity and reachability |
| Layer 3 (Network) | `traceroute` | Trace the path packets take to destination |
| Layer 4 (Transport) | `netstat` | View active TCP connections and listening ports |
| Layer 4 (Transport) | `telnet` | Test TCP connectivity to specific ports |
| Layer 7 (Application) | `curl` | Transfer data and test HTTP/HTTPS connectivity |

These commands provided valuable insights for troubleshooting real-world customer scenarios, from basic connectivity testing to identifying specific points of failure in network paths.
