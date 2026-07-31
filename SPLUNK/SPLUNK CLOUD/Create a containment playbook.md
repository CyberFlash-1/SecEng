# Create a containment playbook
1.
From Splunk ES, download the dummy SOAR app for this lab from the
Downloads
menu in the Splunk App for your Lab guide.
2.
In Splunk SOAR, navigate to the main SOAR drop-down menu and select
Apps
.
3.
Click the
Install App
button.
4.
Locate the SOAR App you downloaded and upload it to SOAR.
5.
Back at the main SOAR App page, select the tab for
Unconfigured Apps
.
6.
In the
Search app names
box, type "lab" to filter the results.
7.
Locate the
awsiam_lab
App and click the
Configure New Asset
button.
8.
In the
Asset configuration
, find the field for "Asset Name" and enter
example-aws-cloud
.
9.
Click the
Save
button.

AWS IAM LAB
<img width="1673" height="638" alt="image" src="https://github.com/user-attachments/assets/e1b741fd-54a5-40b0-af38-2959457a3552" />

Next to App Details, click Edit JSON to view the app.json manifest file. If you scroll through the content, you see the actions and outputs that aresimulated with the App for this lab.

<img width="1686" height="1238" alt="image" src="https://github.com/user-attachments/assets/77773852-9578-404f-aa56-a907b3549fcf" />


<img width="1685" height="800" alt="image" src="https://github.com/user-attachments/assets/e6e33109-50e8-499b-9fca-6c9b44869af6" />



Create a response playbook
Make sure you have your investigation's reference ID available.
1.Navigate to the Splunk SOAR Playbooks page and click the
+ Playbookbutton. The Virtual Playbook Editor (VPE) opens in a new tab.
2.Select Enterprise Security Playbookas the type.
3.
Give the new playbook a name:
Disable IAM User for investigation
.
4.
Paste the investigation reference id you copied earlier into the search field of the
Data Preview
panel as before.
5.
Click the
Save and Run
button to test the playbook with the reference id.
6.
Deal with the pop-up window and the
Block results
on the Data preview panel as before.
7.
From the
Add
menu on the left, select the
Action
block and drag it to the
Start
block.
8.
Select the
awsiam_lab
App that appears on the list of available Apps.
9.
From the list of actions, select
disable_iam_user
.
10.
In the configuration for the action, select
example-aws-cloud
"** from the drop-down.
11.
For Inputs, select the
user_name
field and select
user
within the "consolidated_findings", or paste the value
finding:consolidated_findings.user
into the datapath field.
12.
Click
Done
.
13.
From the
Add
menu on the left, select under "Splunk API" the
Enterprise Security API
(ES API) block. Drag the block to the
Playbook
block in the VPEediting area to connect it.
14.
For the action configuration, select the
add finding or investigation note
action.
15.
For the configuration of the block, enter the following datapath values:
id:
finding:id
title:
Automated AWS IAM Action Run
content:
disable_iam_user_1:action_result.message
16.
Connect the ES API block to the
End
block of the playbook.
17.
Click
Save and Run
to test the playbook.
18.
Verify the playbook executed successfully. You can navigate back to your investigation and see the success and message added to the investigation.



















