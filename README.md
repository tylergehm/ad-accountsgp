  <img src="https://raw.githubusercontent.com/tylergehm/ad-accountsgp/main/az2.jpg" alt="GitHub banner" style="max-width:100%;height:auto;" />
</p>
</p>

<h1>Active Directory: User & Group Management in Azure</h1>
In the Azure Active Directory User & Group Management project, Group Policies (GPOs) are centralized settings applied via Active Directory to manage user and computer configurations, enforcing security and behavior across the domain. The lab continues with account lockout testing: configuring a GPO to lock accounts after 5 failed attempts, attempting a login with a bad password, then observing the lock out being triggered. Then the account will be unlocked, resetting the password, and confirming a successful login. Next, an account will be disabled in Active Directory, demonstrating the lockout, then re-enabling the account and verifying access. The authentication and lock out events utilize both the domain controller and client machine. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Group Policy Management Console
- Command Line

<h2>Prequisites</h2>
  
- Preparing on-premises Active Directory within Azure VMs
- Configuring on-premises Active Directory within Azure VMs

For additional information on how these virtual machines and Active Directory were set up, visit [Configuring Active Directory within Azure VMs](https://github.com/tylergehm/configure-ad)


<h2>Operating Systems Used </h2>

- Windows Server 2025
- Windows 11 Pro (22H2)
- Windows 11 Home (Host Machine)

<h2>Configuration Steps</h2>

- Step 1 - Configure Group Policy for Account Lockout
- Step 2 - Attempt to log in with bad password
- Step 3 - Observe the lock out and reset password
- Step 4 - Enable and disable an account
- Step 5 - Observe Authentication and Security logs in Event Viewer

<h2>Step 1 - Configure Group Policy for Account Lockout</h2>

This project will be utilizing the on-premise Active Directory with two virtual machines; one acting as the Domain Controller and the other VM acting as a client. The project will begin by configuring a Group Policy within Active Directory. </p>

To see how AD was set up, visit the project [Configuring Active Directory within Azure VMs](https://github.com/tylergehm/configure-ad) </p>

Group Policy in Active Directory is a centralized management feature that allows administrators to enforce security settings, configurations, and user restrictions across domain-joined computers and users from a single console. These instructions guide the configuration of an account lockout policy using a Group Policy Object (GPO), defining the number of failed login attempts before an account locks (threshold), how long it stays locked (duration), and when the failed attempt counter resets—enhancing security against brute-force attacks. By creating or editing a GPO in the Group Policy Management Console, applying it to an Organizational Unit or domain, and forcing a policy update with gpupdate /force, administrators can consistently enforce these lockout rules across the network.</p>

<img width="456" height="272" alt="image" src="https://github.com/user-attachments/assets/b28b1efc-c0f7-4f28-b5b7-234559662d2d" /> </p>


The project begins by running the command "gpmc.msc" on the Domain Controller VM. This will open up the Group Policy Management Console. </p>

<img width="1077" height="719" alt="image" src="https://github.com/user-attachments/assets/a6cd0e44-26a0-4a0a-a753-b25dc8af9f2f" /> </p>

Inside the Group Policy Management Console, click the drop down arrows of Forest > Domains > mydomain.com to find the Default Domain Policy. Right-click on this and click on "Edit..." </p>

<img width="1262" height="869" alt="image" src="https://github.com/user-attachments/assets/701e2277-db08-4bc7-a748-99746f02e005" /> </p>

In the Group Policy Management Editor, expand the following: Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy. Open up Account Lockout Policy and double-click on "Lockout Threshold". </p>

<img width="521" height="634" alt="image" src="https://github.com/user-attachments/assets/831150e4-b3c8-47a4-baff-e361aca04490" />

Change the invalid login attempts to a value of 5. Then click Apply and OK. </p>

<img width="1082" height="727" alt="image" src="https://github.com/user-attachments/assets/47d75323-d3c8-406f-ad75-017d872bd625" /> </p>

The Default Domain Policy now shows that there will be an account lock out after 5 attempts. This group policy will automatically become updated after 90 minutes. </p>

<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/932bc5eb-e04f-49a0-9262-54d5c21009a1" />

The 90-minute waiting period for the group policy update can be bypassed by using a command. The command line was opened up and the command "gpupdate /force" was implemented. The group policy update was successful. </p>

The command gpupdate /force immediately refreshes Group Policy settings on the local computer by forcing both the Computer Configuration and User Configuration portions of all applicable GPOs to reapply, regardless of whether they’ve changed since the last update. Normally, Group Policy refreshes automatically every 90 minutes (with a random offset), but /force bypasses this wait, pulling the latest policies from the domain controller right away—ensuring that newly configured settings, like account lockout thresholds or password policies, take effect instantly for testing or urgent enforcement. </p>

<h2>Step 2 - Attempt to log in with bad password</h2>

Now that the Group Policy has been established, an attempt to log in with a bad password will be done with one of the user accounts in the domain. </p>

<img width="790" height="347" alt="image" src="https://github.com/user-attachments/assets/4fe3af3b-1b9d-411c-844b-1b3a5e080a06" /> </p>

After 5 failed attempts with an incorrect password, the user account has been locked. </p>

<h2>Step 3 - Observe the lock out and reset password</h2>

<img width="947" height="660" alt="image" src="https://github.com/user-attachments/assets/2d2f2b0e-ec6f-4d5b-9c29-ae508e3cb4ad" />

Using the Domain Controller VM, open up the "Active Directory Users and Computers" application. Inside the application, find the user account that was locked out within the Domain. Right-click on the user's name and click "Reset Password..." </p>

<img width="937" height="661" alt="image" src="https://github.com/user-attachments/assets/c1abddc1-8223-4726-85dc-1abd556a317a" />

Inside the Reset Password pop up, check the box that says "Unlock the user's account". Then click "OK". </p>

This pop up box is also where it could be selected to force a password change at the next logon. </p>

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/cd7e6999-fe74-44b4-b8bf-bb56a55e737d" /> </p>

The user was successfully able to log back into their account. </p>

<h2>Step 4 - Enable and disable an account</h2>

<img width="938" height="656" alt="image" src="https://github.com/user-attachments/assets/c448c563-7f4f-4887-a931-47a0a4817a52" />

To disable an account, open the "Active Directory Users and Computers" application. Find the user account within the domain that is to be disabled. Right click on the account name and click "Disable Account". </p>

<img width="347" height="214" alt="image" src="https://github.com/user-attachments/assets/11499794-502a-4371-9496-388bbb1f2e4e" /> </p>

The account has been disabled. </p>

<img width="797" height="343" alt="image" src="https://github.com/user-attachments/assets/aab07b73-9682-4435-a4c5-d9cc41a0756b" /> </p>

When the user attempted to log in to the account, the pop up message appeared stating that the account was disabled. </p>

<img width="935" height="657" alt="image" src="https://github.com/user-attachments/assets/7524b053-909a-4e3e-a076-2b9c226715e0" />

To enable the account, return to the "Active Directory Users and Computers" application on the Domain Controller VM. Right click on the account name and click "Enable Account". </p>

<img width="346" height="214" alt="image" src="https://github.com/user-attachments/assets/d637ccb2-7795-4800-a3ba-48328726c685" /> </p>

The account has been successfully enabled. </p>

<img width="1910" height="1079" alt="image" src="https://github.com/user-attachments/assets/a69659a9-ee06-4470-b891-9c226c140215" /> </p>

The user was succesfully able to log back into their account. </p>

<h2>Step 5 - Observe Authentication and Security logs in Event Viewer</h2>

<img width="456" height="272" alt="image" src="https://github.com/user-attachments/assets/3a675e24-c565-4e38-bd2a-c4d96b7b1e7b" /> </p>

To begin observing the logs, open the run command in the DC VM and type "eventvwr.msc" to open the Event Viewer. </p>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3cd55139-c269-4e68-8c1e-b2c49e33a4b4" />

Once the Event Viewer application opens up; click the down arrow next to "Windows Logs" then click "Security". This showcases all of the event logs related to security that have occured on the network of the domain. </p>

<img width="1916" height="1016" alt="image" src="https://github.com/user-attachments/assets/27145af6-2a3a-405d-bcf7-4472f3e539af" /> </p>

Here can be seen the log in failure when the user account was disabled previously in the project. </p>

<h2>Conclusion</h2>

This Azure-based Active Directory lab effectively demonstrates enterprise-grade user and group management by configuring account lockout policies via Group Policy, enforcing security through failed login thresholds, and testing real-world scenarios such as account disabling and log analysis. By leveraging tools like gpupdate /force, Active Directory Users and Computers, and Event Viewer, the project illustrates how administrators can centrally control authentication behavior, respond to security events, and maintain audit trails across domain-joined systems. These hands-on practices reinforce foundational IT security principles—balancing usability with protection against brute-force attacks and unauthorized access—preparing learners to manage identity and access in production Windows environments with confidence and precision.


