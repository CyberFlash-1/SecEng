# Create an ES Response Plan

## Defining Analysis Phase Tasks

A well-structured response plan guides analysts through the incident lifecycle. For this lab, you will focus on the analysis phase, and actions relevant to your investigation in the lab environment and scenario.

1. Select the **Analysis** phase on the left. This opens a list of 12 tasks that were copied from the generic incident response plan.
2. Locate and expand the **Document infected devices** task, then change the title of the task to **Document EC2 instances involved in incident**.
3. At the bottom of the task, locate and expand the **searches** area.
4. Click the **+ Searches** button.
5. Configure a search to find the details for the EC2 instances created as a result of the "RunInstance" event which triggered the alert. You can open a new search tab and start with the basic SPL from the detection:

```spl
index=edu eventName=RunInstances
| spath output=instanceType path=requestParameters.instanceType
| eval userData=$requestParameters.userData$
| eval user=$userIdentity.userName$
| eval src=$sourceIPAddress$
| search instanceType IN ("p3.2xlarge", "p3.8xlarge", "p3.16xlarge", "g4dn.xlarge", "g4dn.2xlarge", "g4dn.4xlarge", "g4dn.8xlarge", "g5.xlarge", "g5.2xlarge")
| where NOT match(sourceIPAddress, "192\\.168\\.\\d+\\.\\d+|10\\.\\d+\\.\\d+\\.\\d+|172\\.(1[6-9]|2[0-9]|3[0-1])\\.\\d+\\.\\d+")
```

<img width="1662" height="1153" alt="image" src="https://github.com/user-attachments/assets/51cd12ca-3306-40ac-8e01-79a97bace2fb" />

## Documenting Network Activity Related to the Incident

1. Locate and expand the **Research network logs** task.
2. At the bottom of the task, locate and expand the **searches** area.
3. Click the **+ Searches** button.
4. Configure the following information:
   - **Search Name**: Traffic associated with new EC2 Instances created by event
   - **Search Description**: An alert was issued based on a "RunInstances" event indicating suspicious activities. This search displays the information for traffic involving all instances created as a result of the event.
   - **Search Syntax**: Replace the "placeholder" values in the SPL example below with applicable IP Addresses uncovered during the investigation and add the SPL here.

```spl
index="edu" src_ip IN ("ip address placeholder", "ip address placeholder", "ip address placeholder")
| eval src_host=$interface_id$
| eval mbs = round('metric_name:bytes' / 1024 / 1024, 2)
| sort - dest_port
| table src_host src_ip src_port dest_ip dest_port mbs
```

<img width="1653" height="746" alt="image" src="https://github.com/user-attachments/assets/89be06db-c73e-4dd6-be9b-56dbea57b69e" />

## Decode EC2 UserData

Let's add a task and associate the ES playbook you created.

1. Click the **+ Task** button at the bottom of the page.
2. For the task name, enter **Examine userData for EC2 instances**. Enter a short description, for example: *Examine userData supplied in "RunInstance" event to understand the instance's launch script*.
3. Locate and expand the **Playbooks** area.
4. Click the **+ Playbooks** button.
5. Select the **Decode base64 userData** playbook you created earlier in the lab.
6. Click the **Submit** button.
7. Select the **Require note on task completion** checkbox before collapsing the task.
8. Locate and toggle the **Unpublished** switch to **Published**.
9. Click the **Save Changes** button at the bottom-right area of the page.

## Task 4: Assign and Use Your ES Response Plan

1. Navigate back to your investigation in Mission Control.
2. Select the **Response** tab.
3. Click the **+ Response** button.
4. Select your **AWS EC2 Cryptojacking Response** response plan.
5. Click the **Submit** button.
6. Select **Administrator** from the **Assigned to** drop-down. The response plan is now assigned, and open within the response tab of your investigation.
7. Click the **Save** button.
8. Locate the **Analysis** phase on the left and select it.
9. You can now navigate to the tasks and execute the searches or the playbook you added from here.

## Task 5: Incident Summary Review

### Project EC2 Drain

Review the attack timeline and details.

**Compromised User:** dev-automation (AKIAIOSFODNN7EXAMPLE)

**Attacker IP:** 203.0.113.42

**Attack Timeline:**

- **13:49:55Z** – Initial ConsoleLogin for dev_automation user.
- **14:00:05Z – 14:00:25Z** – Reconnaissance (List/Describe calls) by dev-automation from 203.0.113.42.
- **14:01:00Z – 14:01:30Z** – Setup of infrastructure: CreateSecurityGroup (sg-0abcdef1234567890) with permissive ingress, CreateKeyPair (miner-key-20251021).
- **14:02:00Z** – RunInstances (3 x p3.2xlarge) with userData to install crypto miner. Instances: i-0123456789abcdef0, i-0123456789abcdef1, i-0123456789abcdef2.
- **14:02:30Z** – AssociateAddress to assign public IPs.
- **~14:03:00Z – 14:30:00Z** – Active cryptocurrency mining confirmed by cloud-init-output.log showing xmrig execution and high outbound VPC Flow Log traffic to 1.2.3.4:3333 and 5.6.7.8:4444 from the launched instances.
- **14:30:00Z – 14:32:00Z** – Attacker cleanup attempts: TerminateInstances, RevokeSecurityGroupIngress, DeleteSecurityGroup, DeleteKeyPair, and finally DeleteAccessKey for AKIAIOSFODNN7EXAMPLE.

**Impact:** Unauthorized resource consumption, significant AWS billing, compromised dev-automation credentials.

**TTPs:** Initial Access (Compromised Credentials), Reconnaissance, Resource Provisioning, Execution, Defense Evasion (Cleanup).

### Post-Incident Analysis & Prevention

While the immediate incident is contained, a critical part of incident response is learning from the event to prevent future occurrences. Review the following questions and consider your answers. Once you're ready, you can review the answers provided and compare them to your own ideas and analysis.

- **Root Cause Analysis:** Based on your investigation, what is the most likely root cause of this incident? (Hint: How did the attacker gain initial access?)
- **Preventative Measures:** What steps could RabbitHole Labs take to prevent a similar incident in the future? Consider IAM best practices, network security, and monitoring.

