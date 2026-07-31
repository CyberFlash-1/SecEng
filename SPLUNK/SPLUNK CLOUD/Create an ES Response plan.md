# Create an ES Response plan

Defining Analysis Phase Tasks
A well-structured response plan guides analysts through the incident lifecycle. For this lab, you will focus on the analysis phase, and actions relevant to yourinvestigation in the lab environment and scenario.
1.
Select the
Analysis
phase on the left. This opens a list of 12 tasks that were copied from the generic incident response plan.
2.
Locate and expand the
Document infected devices
task, then change the title of the task to
Document EC2 instances involved in incident
.
3.
At the bottom of the task, locate and expand the
searches
area.
4.
Click the
+ Searches
button.
5.
Configure a search to find the details for the EC2 instances created as a result of the "RunInstance" event which triggered the alert. You can open a newsearch tab and start with the basic SPL from the detection:

```
index=edu eventName=RunInstances| spath output=instanceType path=requestParameters.instanceType| eval userData=$requestParameters.userData$| eval user=$userIdentity.userName$| eval src=$sourceIPAddress$| search instanceType IN ("p3.2xlarge", "p3.8xlarge", "p3.16xlarge", "g4dn.xlarge", "g4dn.2xlarge", "g4dn.4xlarge", "g4dn.8xlarge", "g5.xlarge", "g5.2xlarge")| where NOT match(sourceIPAddress, "192\\.168\\.\\d+\\.\\d+|10\\.\\d+\\.\\d+\\.\\d+|172\\.(1[6-9]|2[0-9]|3[0-1])\\.\\d+\\.\\d+")
```

<img width="1662" height="1153" alt="image" src="https://github.com/user-attachments/assets/51cd12ca-3306-40ac-8e01-79a97bace2fb" />

Documenting network activity related to the incident
1.
Locate and expand the
Research network logs
task.
2.
At the bottom of the task, locate and expand the
searches
area.
3.
Click the
+ Searches
button.
4.
Configure the following information:
Search Name
: Traffic associated with new EC2 Instances created by event
Search Description
: An alert was issued based on a "RunInstances" event indicating suspicious activities. This search displays the information fortraffic involving all instances created as a result of the event.
Search Syntax
: Replace the "placeholder" values in the SPL example below with applicable IP Addresses uncovered during the investigation and addthe SPL here.

```
index="edu" src_ip IN ("ip address placeholder", "ip address placeholder", "ip address placeholder")| eval src_host=$interface_id$| eval mbs = round('metric_name:bytes' / 1024 / 1024, 2)| sort - dest_port| table src_host src_ip src_port dest_ip dest_port mbs
```




<img width="1653" height="746" alt="image" src="https://github.com/user-attachments/assets/89be06db-c73e-4dd6-be9b-56dbea57b69e" />
