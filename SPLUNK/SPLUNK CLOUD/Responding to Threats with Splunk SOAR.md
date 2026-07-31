

# Responding to Threats with Splunk SOAR
<p> Splunk SOAR empowers security teams to automate repetitive tasks, orchestrate complex workflows, and accelerate incident response. To explore the power of SOAR, you need to understand the building blocks it uses: apps, assets, and playbooks.</p>

- A SOAR App (also known as a connector) is a Python module that bridges between Splunk SOAR and an external service, enabling Splunk SOAR to execute its operations. Apps implement actions that provide a specific functionality, such as a vulnerability assessment or threat intelligence. Apps arelinked to assets and playbooks in one-to-many relationships.
  
- An asset makes an instance of the external service usable that your app bridges to. It is configured with the parameters required to reach and use the instance of the external service, such as its URL and authentication credentials.

- A playbook is a Python script created in the SOAR Visual Playbook Editor that uses actions from apps to automate the process of incident response.Playbooks can gather information and add it to findings or investigations, use logic operations to decide response options, and use actions to mitigate and contain vulnerabilities or threats.

- SOAR Playbook: When a SOAR playbook runs, it can execute actions against one or more external systems, using the asset configurations to establish a connection and perform the desired operations. The playbook can then use the results of the actions to drive further automation or orchestration within the SOAR instance.
  
In this lab, I will install and explore a SOAR App.

## Task 1: Create an ES Playbook to decode userData
<p>Let's create an ES playbook to run the actions we took manually during our investigation to decode the userData and add it to a finding or investigation.
  Download the base64decode Input playbook
  You will create an ES playbook that calls the Input playbook. Input playbooks cannot be called directly from Splunk ES as an automation, they are meant to
  be called from Splunk SOAR or ES playbooks.
</p>
1. From the Splunk App containing your lab guide, locate the downloads menu to the right of the lab guide menu.<br>
2. Select the Downloads page, then locate and download the SOAR Input playbook.<br>
  Copy your investigation reference ID<br>
  Before you start, you need to capture the reference id of your investigation.<br>
1. In the Analyst queue, find your investigation.<br>
2. Open the investigation and locate the reference id in the details panel on the right<br>
3. Copy the investigation's reference id. You will use this to build and test the SOAR playbook.<br>

```
18d99e2d-7f22-4983-86a5-6f0dc74b4379
```

Navigate to
Security Content > SOAR Playbooks
This opens the playbook page on the paired Splunk SOAR server in a new tab. If you see the"Welcome to Splunk SOAR" pop-up window, simply close it.

<img width="1598" height="685" alt="image" src="https://github.com/user-attachments/assets/86999618-17b9-49b8-a59c-1efd6de32363" />

In the pop-up window, select
Enterprise Security Playbook
as the playbook type.
6.Give the new playbook a name:
```
Decode base64 userData
```
7.
Paste the investigation reference id you copied earlier into the search field of the Data Preview panel.

<img width="1699" height="1006" alt="image" src="https://github.com/user-attachments/assets/a0fe8b4b-bfde-4c25-be22-fa3b28d91922" />


<img width="1693" height="1171" alt="image" src="https://github.com/user-attachments/assets/fa8151ba-bb07-4e55-b60d-13b587ebc2e3" />

12.From theAddmenu on the left, select the Playbook block and drag it to the Start block in the VPE editing area to connect it.
13. In the configuration panel on the left, select the Input option. You can scroll and locate, or type the playbook name to filter for the "base64decode_Input"playbook.
14.Select the "base64decode_Input" playbook.
15.In the configuration for Inputs enter the datapath finding:consolidated_findings.userData.
16. From the Add menu on the left, select under "Splunk API" the Enterprise Security API (ES API) block. Drag the block to the
Playbook block in the VPEediting area to connect it.

<img width="1693" height="1171" alt="image" src="https://github.com/user-attachments/assets/05b1bad7-ed1d-486f-8ab9-3417c31cbc9d" />

17. For the action configuration, select the Add finding or investigation note action.
18. For the configuration of the block, enter the following datapath values: id: finding:id
- title: Decoded base64 userData
- content: playbook_base64decode_input_1:playbook_output:decoded_string
- Note: The exact block name playbook_base64decode_input_1 may vary if you renamed the playbook block in the VPE.
- 
<img width="1557" height="1220" alt="image" src="https://github.com/user-attachments/assets/af352b34-5bd9-453e-9487-d45206d69bfa" />

Playbook results after save and run 

<img width="1693" height="1185" alt="image" src="https://github.com/user-attachments/assets/ff42e7b6-6bf2-4789-8ebc-87ed5ba29485" />


