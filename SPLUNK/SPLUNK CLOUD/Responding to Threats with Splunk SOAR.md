


Splunk SOAR empowers security teams to automate repetitive tasks, orchestrate complex workflows, and accelerate incident response.
To explore the power of SOAR, you need to understand the building blocks it uses: apps, assets, and playbooks.
- A SOAR App (also known as a connector) is a Python module that bridges between Splunk SOAR and an external service, enabling Splunk SOAR to execute its operations. Apps implement actions that provide a specific functionality, such as a vulnerability assessment or threat intelligence. Apps are
linked to assets and playbooks in one-to-many relationships.
- An asset makes an instance of the external service usable that your app bridges to. It is configured with the parameters required to reach and use the instance of the external service, such as its URL and authentication credentials.
- A playbook is a Python script created in the SOAR Visual Playbook Editor that uses actions from apps to automate the process of incident response.
Playbooks can gather information and add it to findings or investigations, use logic operations to decide response options, and use actions to mitigate
and contain vulnerabilities or threats.
- SOAR Playbook
When a SOAR playbook runs, it can execute actions against one or more external systems, using the asset configurations to establish a connection and
perform the desired operations. The playbook can then use the results of the actions to drive further automation or orchestration within the SOAR instance.
In this lab, I will install and explore a SOAR App.

## Task 1: Create an ES Playbook to decode userData
Let's create an ES playbook to run the actions we took manually during our investigation to decode the userData and add it to a finding or investigation.
Download the base64decode Input playbook
You will create an ES playbook that calls the Input playbook. Input playbooks cannot be called directly from Splunk ES as an automation, they are meant to
be called from Splunk SOAR or ES playbooks.
1. From the Splunk App containing your lab guide, locate the downloads menu to the right of the lab guide menu.
2. Select the Downloads page, then locate and download the SOAR Input playbook.
Copy your investigation reference ID
Before you start, you need to capture the reference id of your investigation.
1. In the Analyst queue, find your investigation.
2. Open the investigation and locate the reference id in the details panel on the right
3. Copy the investigation's reference id. You will use this to build and test the SOAR playbook.
