# Enable-RDP-on-Bulk-Computers-via-Group-Policy-
A simple step-by-step guide for Active Directory domain environments. 
Before You Start 
•	Computers must be joined to your Active Directory domain. 
•	You need Domain Admin (or GPO edit) rights and the Group Policy Management console (GPMC). 
•	Put the target computers in an OU (e.g., Workstations) so the GPO only applies to them. 
<img width="776" height="355" alt="image" src="https://github.com/user-attachments/assets/4278f25b-50cd-4e26-91e9-bf92da10e0a3" />
Step 1: Create the GPO 
1.	On a domain controller or admin PC, press Win + R, type gpmc.msc, press Enter. 
2.	Right-click the OU containing the target computers and choose Create a GPO in this domain, and Link it here. 
3.	Name it Enable RDP, then right-click it and choose Edit. 
<img width="980" height="339" alt="image" src="https://github.com/user-attachments/assets/98f0cd64-d7db-4897-a110-d2c29062c6e1" />
Step 2: Allow Remote Desktop Connections 
Go to: 
Computer Configuration 
-	Policies 
 -	Administrative Templates 
   -	Windows Components 
    -	Remote Desktop Services 
     -	Remote Desktop Session Host 
            -	Connections 
               -	Allow users to connect remotely by using Remote Desktop Services 
Double-click Allow users to connect remotely by using Remote Desktop Services, select Enabled, and click OK. 
Optional (more secure): In the same folder, enable Require user authentication for remote connections by using Network Level Authentication.
 <img width="980" height="315" alt="image" src="https://github.com/user-attachments/assets/0e5cbcb3-d708-4f0c-b5d4-49e48d96dd87" />
<img width="994" height="417" alt="image" src="https://github.com/user-attachments/assets/46c93ce9-998f-4902-99f6-d99ad838d3e7" />
Step 3: Open the Firewall for RDP 
Go to: 
Computer Configuration  - Policies 
-	Windows Settings 
-	Security Settings 
-	Windows Defender Firewall with Advanced Security 
-	Inbound Rules Enable: 
 
Remote Desktop - User Mode (TCP-In) 
Remote Desktop - User Mode (UDP-In) 
Right-click Inbound Rules > New Rule > Predefined > select Remote Desktop > Next > keep all rules checked > Allow the connection > Finish. 
<img width="980" height="357" alt="image" src="https://github.com/user-attachments/assets/9b845974-3c8d-4cf0-a484-55677a9bdcb4" />
Alternative: Under Administrative Templates > Network > Network Connections > Windows Defender Firewall > Domain Profile, enable Windows Defender Firewall: Allow inbound Remote Desktop exceptions. 
Step 4: Give Users RDP Permission (Optional) 
 <img width="980" height="354" alt="image" src="https://github.com/user-attachments/assets/cab4439d-19db-4e22-bd93-580a0a45f85a" />
 <img width="777" height="396" alt="image" src="https://github.com/user-attachments/assets/b4b55696-4647-4965-ade8-b427220ff9d3" />
Step 5: Apply and Verify 
1. On a target computer, open Command Prompt as admin and run: gpupdate /force 
<img width="596" height="386" alt="image" src="https://github.com/user-attachments/assets/76a364af-e8ea-4af5-9b69-49a29888976c" />
<img width="570" height="583" alt="image" src="https://github.com/user-attachments/assets/d6905988-bb3e-4ed8-a5ea-36eb1c363ff5" />
<img width="727" height="477" alt="image" src="https://github.com/user-attachments/assets/73bb08a2-0e23-49f6-a5c9-4bf3c82cd005" />
Quick Troubleshooting
Problem 	Fix 
GPO not applying 	Check computer is in the linked OU; run gpupdate /force and restart 
Connection refused 	Verify firewall rule and that the GPO setting shows Enabled in gpresult 
Access denied 	Add user to Remote Desktop Users group 
Wrong scope 	Check Security Filtering includes Domain Computers / Authenticated 
Users

Security Tips 
•	Keep Network Level Authentication enabled. 
•	Never expose port 3389 directly to the internet; use a VPN or RD Gateway. 
•	Limit RDP access to specific security groups and use strong passwords/MFA. 
•	Test on a small pilot OU before rolling out to all computers. 




