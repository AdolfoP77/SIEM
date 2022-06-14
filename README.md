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
<br>
Click on VM and Get IP address, On your HOST Machine click start menu and search for Remeote desktop. (Not shown:intentional failed log in for log demonstration purposes).
![VMLOGGIN1](https://user-images.githubusercontent.com/107056915/173640516-ae55d8e0-4595-4408-a7b0-e95122606a64.png)
Enter Credentials created in Step 1.
![VMLOGGIN2](https://user-images.githubusercontent.com/107056915/173640599-3b676680-4e7f-4cfa-ba4a-d528ce2bc896.png)
![vmloggin3](https://user-images.githubusercontent.com/107056915/173640540-b6ce1265-e5e1-4171-8bdf-9f98cbfb547a.png)
You should be able to log into VM.
![VMLOGGIN4](https://user-images.githubusercontent.com/107056915/173640620-5e7ff6b7-8351-4fc8-8dd4-793d7d6de939.png)
Browse to Event viewer inside the VM, Select windows logs and Security Logs
![vmevent1](https://user-images.githubusercontent.com/107056915/173656726-76598c38-6731-43b3-82ae-3dabfcb16d6a.png)
Select Event 4625, Here you can see the Failed logon we did (not shown) along with reasons and other information regarding the attempt.
![vmloggin5](https://user-images.githubusercontent.com/107056915/173656762-3d39c5fa-ec48-4abd-87a2-9f9dd78f4edf.png)
Next we will disable the Firewall inside of the VM so it is discoverable on the internet faster.
![wf1](https://user-images.githubusercontent.com/107056915/173656770-f9fae2f6-7d99-40ea-acd2-5bdfab2826d3.png)
![wf](https://user-images.githubusercontent.com/107056915/173656783-3a14ea1f-ae5c-48f8-873e-d3d801f9c2c2.png)

<br>

Step 7:
Download Powershell script. 
<br>
https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbjhnT3o5VXlpZ3hHNGQ4RjE0US1DeUlLcG84Z3xBQ3Jtc0ttM3dwMDh1emt4MHpOanYteHJxOW5kUC1uODhqTHpQc1VMTGNzbHNsa2hIeEdUaW1yLUs1Tl9KOXhWS0xLRFl0RjJqcDFpbXhTWXZIR0dudVZuWHAyUk9va2syU2p3QjZKRjBSREhJeVdmT0RDT05Qbw&q=https%3A%2F%2Fgithub.com%2Fjoshmadakor1%2FSentinel-Lab%2Fblob%2Fmain%2FCustom_Security_Log_Exporter.ps1&v=RoZeVbbZ0o0
<br>
You will need to create a ipgeolocation.io and sign up, then paste your own API key to the Powershell script.
<br>
The script will output the data to c:\programdata\

<br>

Step 8:
Create a custom log in Log Analytics Workspace to bring in our custom log.
<br>
Navigate to custom logs and select add custom log
![customlog1](https://user-images.githubusercontent.com/107056915/173660904-11cfe16a-2b87-4ddf-8012-af6ac019d0d6.png)
Inside of VM go to the failed_rdp file and copy the text inside, we will use this data to train the Log Analytics Workspace.
![customlog2](https://user-images.githubusercontent.com/107056915/173660930-3fbdbb0a-faaf-4415-8463-a034990cd7ba.png)
![customlog3](https://user-images.githubusercontent.com/107056915/173660944-3dc23306-6a59-45bb-8759-a7b4532e5f83.png)
Name the custom log failed_rdp.log.
![customlog4](https://user-images.githubusercontent.com/107056915/173660957-ca8c48cc-2951-4fbd-938e-fe16505a8552.png)
![customlog5](https://user-images.githubusercontent.com/107056915/173660966-b22c3f9a-54fe-4180-b610-28e95b4ff302.png)
Map out the path to the file.
![customlog6](https://user-images.githubusercontent.com/107056915/173660983-b5783a33-3964-4603-9cff-ed0757b15fd7.png)
![customlog png7](https://user-images.githubusercontent.com/107056915/173661070-59142e56-c57f-4892-8a08-8fb9ded71c47.png)
Create.
![customlog8](https://user-images.githubusercontent.com/107056915/173661344-69176720-3271-44e5-af18-81e2b4f2bfea.png)
Here we see the Data. Select Run at the top.
![customlog9](https://user-images.githubusercontent.com/107056915/173661118-ace40744-a040-41cf-9aa1-cc1d773df6bd.png)

<br>
Step 9:
Create custom logs/Extract fields from raw custom logs (this teaches Analytics what to pull for data representation on the map).
<br>
Expand log, Right click select Extract fields, this will bring you to this page, Here we wil select the numeric value after Latitude, Write latitude in the text box review on the left, edit if neccesary and select extract. You will do this for map visualization.
![EXTRACTLOG](https://user-images.githubusercontent.com/107056915/173663293-f5d39baa-18a5-4d00-914a-aa2f941b4f22.png)
![EXTRACTLOG1](https://user-images.githubusercontent.com/107056915/173663307-5d29bd1c-b0f6-4e55-ae07-423fc11a8a97.png)
![EXTRACTLOG2](https://user-images.githubusercontent.com/107056915/173663314-b614cb9d-d304-424b-a32d-aa7a1ebe02dd.png)
![EXTRACTLOG3](https://user-images.githubusercontent.com/107056915/173663420-5c9178ca-826e-4dc0-af37-822e3ade7d64.png)
![EXTRACTLOG4](https://user-images.githubusercontent.com/107056915/173663332-b8669e12-ecec-45c3-8407-cf75942170ea.png)
![EXTRACTLOG5](https://user-images.githubusercontent.com/107056915/173663438-94621c76-00ea-4451-97a0-8d671a56e68a.png)
![EXTRACTLOG6](https://user-images.githubusercontent.com/107056915/173663468-9fa0196e-5647-497c-9d9b-66171c5ba1d6.png)
![EXTRACTLOG7](https://user-images.githubusercontent.com/107056915/173663480-8e132fc2-d0d5-41dc-a84c-3659bd48f3cb.png)
![EXTRACTLOG8](https://user-images.githubusercontent.com/107056915/173663487-67f9063e-d426-4eea-9066-6198be3201e6.png)
![EXTRACTLOG9](https://user-images.githubusercontent.com/107056915/173663553-5f8cf05f-33f8-4960-a08f-f4dd2188d115.png)
![EXTRACTLOG10](https://user-images.githubusercontent.com/107056915/173663580-23789aa4-3795-441f-9773-08202c10a60c.png)


We will use ip address and other info gathered in the Event viewer, then foward the info to a internet API (ipgeoloation.io) then this will sent this back to the Analytics workspace, we will create a custom log, and use Sentinel SIEM to plot attacks on a map.

