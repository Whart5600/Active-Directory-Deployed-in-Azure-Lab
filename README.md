# Azure-Lab

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

Environments and Technologies Used

Microsoft Azure (Virtual Machines/Compute)
Remote Desktop
Active Directory Domain Services
PowerShell
Operating Systems Used 

Windows Server 2022
Windows 10 (21H2)
Deployment and Configuration Steps



In this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. We will change the DC to a static IP because its offering Active Directory services to the client machine. Client machine will be joined to the domain. We will control the DNS settings on the client machine, the client machine will use the DC as its DNS server. 



<img width="1004" height="858" alt="image" src="https://github.com/user-attachments/assets/eafdf47f-606f-486d-9907-bad30ad484f6" />


DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. At first the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. Now we can ping DC-1 successfully from Client-1



<img width="1044" height="781" alt="image" src="https://github.com/user-attachments/assets/7eba840c-45ea-4a65-b951-69dba9bc1eb7" />


<img width="977" height="510" alt="image" src="https://github.com/user-attachments/assets/745b4834-b64f-4379-a308-29529b428c1e" />


Now we will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, setup a new forest as "mydomain.com" afterwards restart then log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps properly you should be able to run AD Users & Computers as shown below.

<img width="752" height="528" alt="image" src="https://github.com/user-attachments/assets/274ef625-1219-47df-8a1d-740fa8f9e8f7" />






Excellent! We can start creating Organizational Units (OU). Let's first create an OU named _EMPLOYEES. Create another OU named _ADMINS. In order to do that right click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right click, select new and select user and fill out the information for your new user. The user should be named Jane Doe, she is going to be an Admin so her username will be Jane_admin. Lastly add Jane to the domain admins security group. 



<img width="1616" height="574" alt="image" src="https://github.com/user-attachments/assets/f2c9cc87-bc17-47a8-b43e-7699b1fb3875" />






<img width="589" height="308" alt="image" src="https://github.com/user-attachments/assets/7831defb-c2bb-4dd2-a8b0-316d08d100c0" />


From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com) from the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS. 



<img width="1083" height="542" alt="image" src="https://github.com/user-attachments/assets/906d8f34-23a8-49e3-91bf-8b72cd877563" />






<img width="976" height="512" alt="image" src="https://github.com/user-attachments/assets/38c8f22c-03fa-4c26-9fa9-01969314556e" />






We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com





<img width="1195" height="762" alt="image" src="https://github.com/user-attachments/assets/290d586d-96c1-48f2-88da-6b85cee8f7eb" />


Wonderufl Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.





<img width="857" height="661" alt="image" src="https://github.com/user-attachments/assets/22cad847-fa4c-4635-b424-54e4dad78907" />


Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.



<img width="1432" height="978" alt="image" src="https://github.com/user-attachments/assets/b0d035fa-f60d-4d6d-94ae-44f5ae5ca613" />






<img width="397" height="452" alt="image" src="https://github.com/user-attachments/assets/638699cf-7579-4189-bc86-995d3f2de577" />


<img width="546" height="223" alt="image" src="https://github.com/user-attachments/assets/4d359ba9-be2f-4013-98ab-9769187b42b4" />
