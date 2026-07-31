# Create a containment playbook

1. From Splunk ES, download the dummy SOAR app for this lab from the **Downloads** menu in the Splunk App for your Lab guide.
2. In Splunk SOAR, navigate to the main SOAR drop-down menu and select **Apps**.
3. Click the **Install App** button.
4. Locate the SOAR App you downloaded and upload it to SOAR.
5. Back at the main SOAR App page, select the tab for **Unconfigured Apps**.
6. In the **Search app names** box, type "lab" to filter the results.
7. Locate the **awsiam_lab** App and click the **Configure New Asset** button.
8. In the **Asset configuration**, find the field for "Asset Name" and enter **example-aws-cloud**.
9. Click the **Save** button.

**AWS IAM LAB**

<img width="1673" height="638" alt="image" src="https://github.com/user-attachments/assets/e1b741fd-54a5-40b0-af38-2959457a3552" />

Next to App Details, click Edit JSON to view the app.json manifest file. If you scroll through the content, you see the actions and outputs that are simulated with the App for this lab.

<img width="1686" height="1238" alt="image" src="https://github.com/user-attachments/assets/77773852-9578-404f-aa56-a907b3549fcf" />

<img width="1685" height="800" alt="image" src="https://github.com/user-attachments/assets/e6e33109-50e8-499b-9fca-6c9b44869af6" />

## Create a response playbook

Make sure you have your investigation's reference ID available.

1. Navigate to the Splunk SOAR Playbooks page and click the **+ Playbook** button. The Virtual Playbook Editor (VPE) opens in a new tab.
2. Select **Enterprise Security Playbook** as the type.

# Creating the "Disable IAM User for Investigation" Playbook

**3. Name the playbook**
Enter `Disable IAM User for investigation` as the playbook name.

**4. Add the reference ID**
Paste the investigation reference ID you copied earlier into the search field of the **Data Preview** panel, as before.

**5. Test with the reference ID**
Click **Save and Run** to test the playbook using the reference ID.

**6. Handle the results**
Deal with the pop-up window and review the **Block results** in the Data Preview panel, as before.

**7. Add the Action block**
From the **Add** menu on the left, select the **Action** block and drag it onto the **Start** block.

**8. Select the App**
Choose the **awsiam_lab** App from the list of available Apps.

**9. Select the action**
From the list of actions, choose **disable_iam_user**.

**10. Configure the asset**
In the action configuration, select **"example-aws-cloud"** from the drop-down menu.

**11. Set the Inputs**
For Inputs, select the **user_name** field and choose **user** within the `consolidated_findings` — or paste the following directly into the datapath field:

```
finding:consolidated_findings.user
```

**12. Confirm**
Click **Done**.

**13. Add the ES API block**
From the **Add** menu on the left, under **Splunk API**, select the **Enterprise Security API (ES API)** block. Drag it into the VPE editing area and connect it to the **Playbook** block.

**14. Choose the ES API action**
For the action configuration, select **add finding or investigation note**.

**15. Configure the datapath values**
Enter the following:

| Field | Datapath Value |
|---|---|
| **id** | `finding:id` |
| **title** | `Automated AWS IAM Action Run` |
| **content** | `disable_iam_user_1:action_result.message` |

**16. Connect to End**
Connect the ES API block to the **End** block of the playbook.

**17. Test the full playbook**
Click **Save and Run** to test the complete playbook.

**18. Verify success**
Navigate back to your investigation and confirm that the success status and message were added to the investigation.




