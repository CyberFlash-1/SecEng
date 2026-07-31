


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
