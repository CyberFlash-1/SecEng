$${{\color{RoyalPurple}\Huge{\textsf{Manage Users and Roles}}}}\$$
---

${{\color{Silver}\large{\textsf{Description\ }}}}\$ <br>
---
In this exercise, you clone the sc_admin role to a new limited_admin role and modify its settings. Next, you modify the default index access permissions for the user and power roles. Then, you create two new users, assign them to roles, and test their respective level of access to Splunk Cloud features. Finally, you use the Cloud Monitoring Console to analyze user activity.

1. Direct your web browser to your splunk>cloud server 
2. Navigate to Settings > Roles.
3. In the settings column for the sc_admin role, click . > Clone.
<div align center>
 <img width="800" height="400" alt="GetImage (8)" src="https://github.com/user-attachments/assets/dda2b428-4908-4301-8359-85066d6c1ff7" />
</div>


---

$${{\color{RoyalPurple}\Huge{\textsf{Assign explicit index access to the user and power roles.}}}}\$$

From splunk>cloud, navigate to: Settings > Roles

11. Click user.
12. Select the Indexes tab.
13. In the Included column, uncheck the checkbox for * (All non-internal indexes)
14. Check the Included checkboxes for the,bcg_online, itops, main and sales indexes as shown:

<div align center>
<img width="800" height="500" alt="GetImage (9)" src="https://github.com/user-attachments/assets/f6a0ea4a-6ec4-468f-89f2-887a8488b97d" />
</div>
---

$${{\color{RoyalPurple}\Huge{\textsf{Add a user account for coryf and assign the limited-admin role to the account}}}}\$$

- Name: coryf
- Email address: coryf@example.com
- Set password: splunk3du
- Confirm password: splunk3du
- Assign to roles limited_admin (remove other roles in the Selected items list)
- Require password change… Uncheck this check box
