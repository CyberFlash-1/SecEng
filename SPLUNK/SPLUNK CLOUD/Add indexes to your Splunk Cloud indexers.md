
$${{\color{Teal}\Huge{\textsf{ HOW TO ADD INDEXES TO SPLUNK CLOUD}}}}\$$

---
### Steps
1. From splunk>cloud, click Settings > Indexes to view the list of indexes.
<div align="center">
<img width="700"  alt="GetImage" src="https://github.com/user-attachments/assets/1c21fea3-52a1-4b7c-b1e2-8f8bee331d10" />
</div>
3. Type main in the Search… filter box at the top of the list, and click the main index (not cms_main) to examine how this index is configured.

   - Max raw data size: 0 MB (unlimited)
   - Additional storage: No additional storage
   - Searchable retention (days): 1095 (3 years)

4. Click Cancel to leave the index settings.
6. Click New Index.
7. Create a new index named test to store testing events by using the following settings:
```
Name: test
Index data type: Events (default)
Max raw data size: 1 GB
Additional storage: No additional storage
Searchable retention (days): 7
```
<div align="center">
<img width="700" height="600" alt="GetImage (1)" src="https://github.com/user-attachments/assets/29e94aec-9f47-4c67-bab8-49f3f1d7f005" />
</div>

Click New Index and create a new index named itops with the following settings:
```
Name: itops
Index data type: Events
Max raw data size: 1 GB
Additional storage: No additional storage
Searchable retention (days): 32
```
<div align="center">
<img width="700"  height="600" alt="GetImage (2)" src="https://github.com/user-attachments/assets/4d656310-b90f-417b-a43e-5c43bd5745fd" />
</div>

8. Click New Index and create a new index named securityops with a Max raw data size of 1 GB and Searchable retention of 32 days.
9. Click New Index and create a new index named sales with a Max raw data size of 1 GB and Searchable retention of 32 days.
10. Click New Index and create a new index named bcg_online with a Max raw data size of 1 GB and Searchable retention of 32 days.
11. The next step is to create the same indexes on teh agement management server
    - itops
    - securityops
    - sales
    - bcg_online
  Next is to upload idxtest.cvs

<div align="center">
<img width="800"  height="600" alt="GetImage (3)" src="https://github.com/user-attachments/assets/e5bbb095-57ce-4511-9e6a-6841b63d858f" />
</div>

Index Type: Uploaded File
Filename: idxtest.csv
Source Type: csv
Host: idxtest
Index: test

<div align="center">
<img width="1000"  height="600" alt="GetImage (4)" src="https://github.com/user-attachments/assets/16011def-16fd-429f-8623-bac371ba65cb" />
</div>

Verifying that events exist using 
```
source="idxtest.csv" host="idxtest" index="test" sourcetype="csv" 
```
<div align="center">
<img width="600" height="500" alt="GetImage (5)" src="https://github.com/user-attachments/assets/deb6354f-06af-4c5e-8a7a-4e15a5fcead8" />
</div>

Checking the Cloud Monitoring Console app and verifying that the index: test is listed.
<div align="center">
<img width="800" height="500" alt="GetImage (6)" src="https://github.com/user-attachments/assets/47cdb5ca-6568-4caf-ac68-46f6a0b3f69a" />
</div>
This is all the indexes: split by index 

<div align="center">
<img width="1494" height="600" alt="GetImage (7)" src="https://github.com/user-attachments/assets/64479966-34b9-4b48-8bdd-6f1e8ae42232" />
</div>
