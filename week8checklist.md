 **Description: Creating a technical log for week 7 checklist** 
•	setting up of technical log through excel sheet







**Description: Creating a google cloud console** 
This was done in the week 5 & 6. I request my credits and provide my Google (Gmail) email address to the instructor. I then Followed these short steps to complete this task:
•	First, I need to have e-mail account and follow the provided link to get Google Cloud Platform (CGP) credit and fill in the form.
•	Next, I need to verify in my Fleming’s email account by clicking on the verification link. 
•	Still on Fleming’s email account, I need to check (spam) folder for another email now containing the credit code string. I can then follow the steps to apply the credit to my personal Gmail account.
•	Once completed, this should create a new GCP billing project My First Project and assign the academic credits to this project.








**Description: Creating a project in google cloud platform** 
•	First step is to log in through the console.cloud.google.com by putting up the e-mail which has google credit. 
•	To create a project (if not automatically created) then click on the project list and click New Project. 
•	By clicking on the New Project link top-right of the window. The new project window will appear. 
•	Enter a project name for your content. Set an account as an individual one, so no organization will be set.








**Description: Creating a virtual machine**
•	Open google cloud console and enable CGP Compute Engine API (if not already enabled). Once enabled open the Compute Engine console to start creating the new virtual machine. For this click on the navigation menu and then click on Compute Engine, it will show list of sub menu and then click on VM instances. 
•	Once in the compute engine console, initiate the Create Instance tool using the button. 
•	In next step we will make some changes to the defaults for a new virtual machine before proceeding. First, name the virtual machine. The recommended name is arcgisserver-geom99, but this can be set to anything meeting GCP’s requirements without concern. 
•	While we are not going to make changes, it is important to understand where this virtual machine will be created. The cloud is not a mythical place—it always has a location, and in this case, we will be creating this virtual machine on a server in Iowa. 
•	For machine configuration we will use the defaults. This sets the “size” of the virtual machine: we can allocate more virtual CPUs, or more ram by changing the series or machine family (but will cost more). This can be changed with the VM not running if needed using the GCP console. The costs increase significantly with more CPUs and ram! For this task the e2-medium is enough, but in a business or production environment use Esri’s requirements (8gb RAM). 
•	For boot disk, choose a Windows-based image with ArcGIS Server pre-installed. Click ""Change"" to locate the instructor-provided image. 
•	Select ""Custom Images"" and switch the project to the one specified by instructor (ArcGIS Server for 2023). Go to ""All"" and select Shawn’s ArcGIS Server project entry. 
•	Once the project is chosen, find the custom image in the dropdown list. If there are multiple, select the one with the latest version/date. 
•	Now enable the Firewall to allow both HTTP and HTTPS traffic. These allow the web server preinstalled to work. 
•	Now hit the create option. 
•	Once complete, we will see the Status a green checkmark, and now we can get started to configure the instance to connect and log in. Find the Connect column on the VM and click the down arrow next to RDP. Select the Set Windows password. 
•	Change the username to be student. This is a specific account pre-configured for this course. Although we could enter any username and GCP will create that user as an administrator in our new VM. This command will reset the password and show you that generated password. Enter “student” and click SET. Be sure to save the password since it will show up for once for the particular IP address. 
•	Now that the VM has a password set that WE know, a change to a firewall rule is necessary to make it work from Fleming computers. We use remote desktop/RDP to connect to the desktop of the remote machine. The default for RDP is it to use TCP port 3389, but that TCP port is blocked by Fleming IT for security reasons. This VM image has been specially configured to listen to 444 for remote desktop instead of 3389.
•	For firewall rule, click on create firewall rule it will pop up a window, then name flemingrdp444. (Although the name can be different). Next, under the targets dialog select the All instances in the network option. This means this rule will apply to any VM on your account. After, find the Source filter and select IPv4 ranges. Enter where you would like to log into the Virtual Machine’s Windows desktop from (choose the most restrictive ideally, but if unsure or having issues select any computer).
•	Any Fleming Campus computer: 142.237.0.0/16 
•	A specific computer network, like at home: Enter your public IP number. An example is 24.242.25.53 (just google whatsmyip to see yours). 
•	Any computer on the internet (no restrictions): 0.0.0.0/0 
•	Next, set the port to be 444. This again is not the normal RDP port (remote desktop protocol), the default is 3389 . But to facilitate on-campus students, this port default has changed to be 444 for this custom image virtual machine. 
•	Now click Create to make this rule and apply it to all VMs on our project.







**Description: Setting a GCP Firewall Rule to allow ArcGIS Server Management Ports** 
•	ArcGIS Server management has two ports, 6443 and 6080, both of which are not open by default Google Cloud Platform firewall rules. Before any request to these ports is permitted from outside of the VM, they must be enabled as open.
•	In GCP’s console open Firewall under the main category VPC network and click Create Firewall Rule. • Name your firewall rule arcgisserveradmin (any name will be accepted, but for instructor support we named as above). 
•	Next, under Targets set it to All instances in the network. This means the firewall rule applies to all VMs in your project, so this is not specific to any one VM.
•	In the Source filter and select IPv4 ranges. Enter the IP address where you would like to run ArcGIS Pro from (choose the most restrictive ideally, but if unsure or having issues select the 0.0.0.0/0 option). 
•	(Best for working from the college) Any Fleming Campus computer: 142.237.0.0/16
• (most restricted/for working from home) A specific computer network, like at home: Enter your public IP number. An example is 24.242.25.53 (just google whatsmyip to see yours). Note this number should never start with 192.168.x.x or 10.x.x.x where x is any number from 0 through 255 as those are special internal-only numbers.
•	Any computer on the internet (no restrictions—not recommended): 0.0.0.0/0 You can come back and modify the rule and change the address later if your IP or situation changes, like logging in from home after being on campus.
•	Now set the TCP ports to open as 6443 and optionally 6080 
•	Click Save and now the rule has been created in our account. It will apply to all VMs . 
•	There is another firewall built into Windows itself. This Windows firewall must also be configured to allow TCP 6443 and 6080 to pass through from outside the network. This is a permanent setting in windows that needs to be completed once per virtual machine. Log into the remote desktop on the started Windows Server and follow these steps. 
•	This must be completed on the server itself, not on your local computer. 
•	In the Start menu on the server type firewall and select the Windows Defender Firewall with Advanced Security option. 
•	Click on the Inbound Rules and select New Rule. (If the dialog that opens looks different, select Advanced Options in the firewall to provide the full settings as shown below). 
•	Select Port for the rule type so we set what will be permitted through the firewall from the public side. Click NEXT, Select TCP and enter the two ports we are permitting through for ArcGIS Server administration, 6443 and optionally 6080. Click NEXT, the default of Allow the connection is correct. Click Next, the rule will apply for all options, Domain, Private and Public. Click Next, name your rule ArcGIS Server Admin Ports to be consistent and for instructor support. Click Finish to create and enable this rule.







**Description: Shutting down or starting Virtual Machine on GCP** 
•	Open the Google Cloud Platform (CGP) Compute Engine’s VM instances console.
•	Under the list of instances find the one you wish to start and confirm it is not currently running. The status should show a stopped icon. This means it is not running. 
•	On the rightmost side click the vertical … to open the control menu. Click Start / Resume on the menu that appears to start up the stopped VM. 
•	Confirm you wish to start the VM and give it a moment to initiate the start-up. The status will turn to a green checkbox and an external IP will be assigned.







**Description: Initiation of Remote Desktop to Virtual Machine**
•	We have to find the running VM External IP number from the GCP Compute Engine console. 
•	On Windows desktop locally, start the Remote Desktop Connection tool by clicking Start and typing Remote Desktop Connection (or mstsc for the short name). 
•	 The Remote Desktop connection interface will appear. Enter the IP address that the current virtual machine started on (again, this changes each time so be sure to check what yours is!). Add :444 at the end to tell what port the connection will be made. 
•	The system will then ask you for your credentials. Note this dialog will be different depending on your computers’ network. You may need to select More Choices to specify a username. 
•	Enter Student and then the password you wrote down earlier when creating the virtual machine. • Once entered, click OK and then the connection will be made and, if the server can be found and 444 port accessed, will prompt with this. Click Yes. 
•	Now we will have the remote desktop window open . This is the computer we created located in Iowa.








**Description: Publishing Canada service to Arcgis server on GCP Virtual machine**
•	I create a folder in the name of gisworkspace in the system folder in local computer, and then inside the gisworkspace folder, I then create a sub folder in the name of Canada and inside the sub folder I pasted it out the data which I copied from my remote desktop provided by my instructor. 
•	In my remote desktop, I went to the system folder and find the folder name gisworkspace and inside I pasted it out the data in the folder name Canada. Just to match the folder location of the data in my remote desktop as well as the local computer. 
•	I log into my arcgis pro and then create a project, then add canada’s shapefile, I went to the connection from Insert tab and then choose server, click on new arcgis server and then I pasted it out my external IP address from virtual machine, for example: https://35.192.183.147:6443/arcgis in the URL section, provided the username and password. Finally, it creates my arcgis server on the arcgis pro. 
•	Right click on the server and choose publish, name your map, write description and choose reference data instead of copying data, analyze it and register your data if not ,when registering data, it will pop window on that you have to give a name and the Publisher folder path and server folder path as well, there will be small tick box , select tick box and made same as publisher folder And server folder. Finally hit publish. 
•	After publishing make sure to check is the data is on server or not as well as the data is reference or copied.
•	References: https://35.192.183.147/arcgis/rest/services/Geom99/yuahmaCanada/MapServer 
•	And make sure to check on the remote desktop services directory. 
•	Very Important point to remember, whenever you shut down your virtual machine, then it will generate new external IP address, copy the new IP address and replace the old Address and make sure you also log in to the remote desktop. (Think like you are waking up the system)

