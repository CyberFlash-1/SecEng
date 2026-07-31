
$${{\color{Goldenrod}\Huge{\textsf{  Splunk \ Enterprise Security: \ Investigating \ and \ Responding \ in \ AWS\ \}}}}\$$


$${{\color{Goldenrod}\large{\textsf{Project EC2 Drain\ }}}}\$$

Alarming Indicators of Compromise Detected
- Target: Rabbit Hole Labs, and their key development environments in AWS.
- Threat Actor: Highly opportunistic and financially motivated, codenamed "The ByteBandits."
- Objective: Illicit cryptocurrency mining and resource abuse, leading to significant unexpected costs.
Let's get straight to the facts. The RabbitHole Labs AWS account is in trouble We've detected unusualand expensive resource provisioning, indicating a clear compromise. This isn't about data theft, butabout attackers leveraging the infrastructure to mine cryptocurrency, directly impacting the AWS bill and service availability.


<img width="775" height="548" alt="image" src="https://github.com/user-attachments/assets/d40b16f4-3dee-4032-89ab-1ce35065d9e4" />


### AWS Cloudtrail
- Effective detection hinges on understanding where to find the evidence. For these early attackphases, AWS CloudTrail is paramount. This service provides a complete record of API activity across your AWS accounts. Every action, whether performed through the AWS Management Console, AWS CLI, AWS SDKs, or other AWS services, is logged as an event.
- Event Types: CloudTrail captures events like ConsoleLogin (user sign-ins to the console), and various API calls (e.g., ListUsers, ListAccessKeys, DescribeInstances).
### Key Log Fields for Investigation:
- eventName: Identifies the specific API call or action performed.
- userIdentity.userName: The IAM user or role that performed the action.
- sourceIPAddress: The IP address from which the request originated.
- userAgent: Provides information about the client that made the request (e.g., web browser, AWS CLI version, SDK). A shift from a browser user agent to a CLI user agent for the same user is a significant anomaly.
- eventTime: The timestamp of the event, essential for reconstructing the attack timeline.
- MFAUsed: Indicates whether Multi-Factor Authentication was used for a ConsoleLogin. No MFA is a major red flag.
### AWS CloudWatch
CloudWatch Logs allows you to centralize logs from various sources, including operating systems and applications running on EC2 instances. While not directly for initial access, these logs become critical
after an instance is launched.
```
Why it's crucial: If an attacker launches an EC2 instance, the logs from within
that instance (e.g., syslog, auth.log, application logs, or even specific logs from the
userdata execution) can reveal what the attacker is doing inside the compromised
resource. This provides granular detail about malware installation, command
execution, and network connections initiated from the instance itself
```

### AWS VPC Flow Logs
These logs capture metadata about the IP traffic flowing to and from network interfaces in your Virtual
Private Cloud (VPC). They provide a record of network connections, regardless of whether the traffic
is allowed or denied by security groups or network ACLs.
Why it's crucial: VPC Flow Logs are essential for understanding network communication patterns.
Even in the early stages, if an attacker is probing internal networks from a compromised instance, or if
a newly launched instance immediately attempts to connect to suspicious external IPs (like C2
servers or mining pools), Flow Logs capture this. They act as a network "black box recorder.

### Key Log Fields for Investigation:
srcaddr: The source IP address of the traffic.
dstaddr: The destination IP address of the traffic.
srcport: The source port of the traffic.
dstport: The destination port of the traffic.
action: Whether the traffic was ACCEPTed or REJECTed by security rules.
bytes, packets: The volume of data transferred, which can indicate data exfiltration or sustained
malicious activity.
instance-id: (Often available in enriched Flow Logs) Links the network traffic directly to a specific
EC2 instance



$${{\color{Goldenrod}\large{\textsf{Scenario\ }}}}\$$
The Wonderland SOC has received a critical alert for unusual EC2 instance
provisioning. You need to investigate the finding and help the Wonderland SOC
track down the threat.
Let’s dive in and uncover the truth in the AWS CloudTrail and CloudWatch logs




<img width="1658" height="493" alt="image" src="https://github.com/user-attachments/assets/30b6aa7c-eeb0-400e-b05b-e9b75a98a172" />
```Locate and click the Start investigation button. You are re-directed to the investigation you just opened. 5. Note the investigation ID "ES-00001" Write it down for future reference.```

<img width="1688" height="1214" alt="image" src="https://github.com/user-attachments/assets/90d0625a-403e-4a6e-8648-4355bb466ff3" />
Assigning to myself ans changing the status to in progress 

This is the AWS IAM User, and IP address where the RunInstance request was issued from, triggering the alert.
- User dev-automation 25
- Source: 203.0.113.42



Based on the eventNames in the drill-down search results, other activities this user identity been associated with include:
```index=edu eventSource="ec2.amazonaws.com" eventName="RunInstances" userIdentity.userName="dev-automation" ```
<img width="1321" height="1105" alt="image" src="https://github.com/user-attachments/assets/d306e295-ac27-4a1d-b312-827efd10ee43" />


1. How many instances were created by dev-automation based on the event? 3 Instances
2. What type of instance was created, and what is the family, vCPUs and memory for the instance type? p3.2xlarge
3. What is the security group these instances are added to?sg-0abcdef1234567890
4. Was there any userData passed in the requestParameters?
5. What are the instance ids for instances created?  i-0123456789abcdef2
6. What IP Addresses assigned to the instances? 54.1.2.3, 54.1.2.4,54.1.2.5



From Splunk ES, navigate to Analytics > Security Intelligence > Risk analysis
<img width="1602" height="633" alt="image" src="https://github.com/user-attachments/assets/27705ec7-088d-426d-9f74-3b98037898f8" />

Intermediate Findings 
<img width="1486" height="646" alt="image" src="https://github.com/user-attachments/assets/9cbd76e9-fd79-44e1-b525-1e4d73ddc5c3" />




Now, let's trigger a SOAR action for some event enrichment. You should investigate the IP addresses
identified in the finding that was the source of the API call which triggered the alert.
1. Within your Splunk ES investigation, copy the "Source" IP address from the Additional fields.
2. Go to the Automation tab.
3. You should see two available actions: Run action and Run playbook.
4. Click the Run action button.

<img width="1635" height="826" alt="image" src="https://github.com/user-attachments/assets/1c568b94-07e3-46c7-963c-490ee81a2221" />

6. Select the WHOIS connector, then select the whois ip action.
7. Type, or copy and paste, the IP address you identified in the prior steps.
8. Click the Run action button.
9. Refresh the page if the action remains in status "running" for more than three minutes.

<img width="949" height="583" alt="image" src="https://github.com/user-attachments/assets/7ff92777-057c-4eca-9bcf-7b1080a182a0" />


11. After the action completed, click the json icon to view the results, and expand the result fields by
clicking {[+]}.

<img width="1615" height="1103" alt="image" src="https://github.com/user-attachments/assets/8bf52c9d-2202-43d9-869d-8966d53eb21b" />


13. Reviewing the results shows a message "Error Message: IPv4 address 203.0.113.42 is already
defined as TEST-NET-3 via RFC 5737", which confirms the IP address is part of a reserved range,
often designated for "private use", or in this case, a lab! This confirms for us that this is not a live, active threat IP, because this is a lab. But what about real malicious IPs.


Decode the userData
The userData identified in the RunInstances request should be decoded for more context. Before we
decode this field, let's add it to the "Additional fields" so it shows in this and future findings.
1. From Splunk ES menus, select Configure > All configurations.
2. In the Findings and investigations section, select the Field values for findings link.
  <img width="1360" height="1058" alt="image" src="https://github.com/user-attachments/assets/431f1cea-6b4c-44c5-86b4-04ba396e59d2" />
4. On the Field values for findings page, select the + Field button.
5. For the field and label, enter userData.
6. Select the Save button.
7. Return to your investigation, and confirm that the userData field is now visible in the Additional
fields area.
<img width="1655" height="845" alt="image" src="https://github.com/user-attachments/assets/a836b6e5-0b24-40d6-b9e4-b3a54d7bf58c" />
```
IyEvYmluL2Jhc2ggXG4gYXB0IHVwZGF0ZSBhbmQgYXB0IGluc3RhbGwgXG4gY3VyZCB4bXJpZyB4bXJpZyAtYSB4bXJpZ3Bvb2wuY29tOjMzMzMgLXUgd2FsbGV0X2FkZHJlc3NfMjAyNTEwMjEud29ya2VyX25hbWU
```
8. Copy the value for userData and use a tool of your choice, or 
Cyberchef to decode the data.
What does the userData reveal?











