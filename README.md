# SIEM
Sentinel Siem Honey Pot Lab
<br>
In this Lab, I setup Azure Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot. We will observe live attacks (RDP Brute Force) from all around the world. We will use a custom PowerShell script to look up the attackers Geolocation information and plot it on the Azure Sentinel Map.
(I will be using a powershell script provided by Educator Josh Madakor).

Step 1:
Create Azure Subscription. (You will receive a $200 credit, this will be enough to complete this lab. Make sure to delete the resources once finished to avoid unwanted charges.)

<br>

Step 2:
Create Virtual Machine.
<br>
Navigate to Virtual Machines by using the search bar at the top of the page.
![VM1](https://user-images.githubusercontent.com/107056915/173632168-2186999d-24ae-489e-8687-aee168b2cae0.png)
Once in the Virtual Machine Section select Create at the top left.
![VM2](https://user-images.githubusercontent.com/107056915/173632177-6d117440-f468-4531-b135-de1bf83c41f1.png)
You should see a drop down menu, select Azure Virtual Machine,
![VM3](https://user-images.githubusercontent.com/107056915/173632180-0b6d25b3-26d5-41a8-8132-0c8ec3c8934c.png)
Under the Resource Group select "Create new" and name it Honeypotlab.
![VM4](https://user-images.githubusercontent.com/107056915/173632187-705b11d6-53bf-49cc-b8a5-000a71b12e66.png)
Name the Virtual Machine and select the highlighted options.
![VM5](https://user-images.githubusercontent.com/107056915/173632385-e0567063-1b9d-4d03-a4f3-e578b21721b2.png)
Create a username and password (this will be used to remote access the Virtual Machine later.
![vm6](https://user-images.githubusercontent.com/107056915/173632312-c42c1125-5d54-4507-b717-48a943ed6c21.png)
![vm7](https://user-images.githubusercontent.com/107056915/173633014-cd628a78-c2cd-426a-aea6-746c0a6cf0a7.png)
Select Advanced and Create new in the NIC Network Security Group Section (This will act like a firewall)
![VM8](https://user-images.githubusercontent.com/107056915/173632438-5da94323-e3dd-4bc9-95f8-cca1178144f6.png)
Delete the default rule and "add an inbound rule" We will create a new rule, this will allow all traffic. We want our VM to be very discoverable for Lab purposes only.
![VM9](https://user-images.githubusercontent.com/107056915/173632447-e5d5cea2-337d-4530-b5cb-2c4b666a0926.png)
![VM10](https://user-images.githubusercontent.com/107056915/173632461-5c6bc95d-c75d-452f-a44a-d5e78e323872.png)
![VM11](https://user-images.githubusercontent.com/107056915/173632468-512282bb-8dbe-4f81-befc-1c598018c511.png)
Select Review and create.
![VM12](https://user-images.githubusercontent.com/107056915/173632475-e0ba1d2b-ef54-4aac-a018-58627d3daf4b.png)
![VM13](https://user-images.githubusercontent.com/107056915/173632492-ba3be2bd-fd02-4b00-b797-7c069eb16584.png)
Our VM is complete.
![VM14](https://user-images.githubusercontent.com/107056915/173632506-c4cd8039-0f11-4e27-b23b-e82c7486d4b9.png)

<br>

Step 3:
Log Analytics.
We will ingest logs from the VM, and create our own custom log that contains Geographics information to identify where the attacks on the VM are coming from.
<br>
Browse to the Log Analytics workspace by using the search bar.
![VM15](https://user-images.githubusercontent.com/107056915/173635916-714abf58-ca0d-4cf6-af3f-9b52d8175889.png)
Select Create log analytics workspace.
![VM16](https://user-images.githubusercontent.com/107056915/173635937-f2580768-54f4-4255-a422-8b433f668ac8.png)
Select our resource "Honeypotlab".
![law1](https://user-images.githubusercontent.com/107056915/173635956-b0598de6-5952-4f2e-b4a6-1c4ce47f0098.png)
Select Create.
![law2](https://user-images.githubusercontent.com/107056915/173635962-3eff1d2c-9978-4d2a-aeca-ce574d54e6f8.png)

<br>

Step 4:
Enable gathering logs in Security Center.
<br>
Browse to Microsoft Defender for Cloud, select Environment Settings at the bottom left.
![sec1](https://user-images.githubusercontent.com/107056915/173637642-06ee879c-a175-4f29-aab6-21dc2d01b535.png)
![sec2](https://user-images.githubusercontent.com/107056915/173637659-eb1b995a-6307-431b-b602-8855814dc27e.png)
Disable SQL Servers
![sec3](https://user-images.githubusercontent.com/107056915/173637671-b22cb80a-3dfc-4c77-b169-835f960b75c2.png)
Select all events and Save.
![sec4](https://user-images.githubusercontent.com/107056915/173637681-b1a725c6-6d7f-416d-826d-e8d119f3a0e0.png)

<br>

Step 4:
Connect Log Analytics to VM
<br>
Browse back to Log Analytics Workspace, select our resource.
![log1](https://user-images.githubusercontent.com/107056915/173638484-d17fdd87-4fb7-40a6-b279-e97e745c6b97.png)
Select Virtual Machines.
![log2](https://user-images.githubusercontent.com/107056915/173638508-46423073-b390-4ad5-ad11-b6ff07f2f669.png)
Select Law-honeypot1.
![log3](https://user-images.githubusercontent.com/107056915/173638513-3e314986-7ac8-4815-9ea5-4ce2bc67db9f.png)
Select Connect.
![log4](https://user-images.githubusercontent.com/107056915/173638527-71766c4f-e5b9-4451-975c-ac859c5695b9.png)

<br>

Step 5:
Set up Azure Sentinel, this is the SIEM we will use to visualize attack data.
<br>
Navigate to Microsoft Sentinel.
![SIEM2](https://user-images.githubusercontent.com/107056915/173639664-2d9780a7-0ec7-4869-a626-fe2e40725c08.png)
Select Create Microsoft Sentinel
![SIEM2](https://user-images.githubusercontent.com/107056915/173639743-1b6c70da-da11-4e4f-b919-0bd8e50d5ae8.png)
Select Law-honeypot1 and select add.
![SIEM3](https://user-images.githubusercontent.com/107056915/173639671-36c00fb6-25a9-42fe-9aa2-62728a70de45.png)

<br>

Step 6: Log into VM
Click on VM and Get IP address.




