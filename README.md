<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory: User & Group Management in Azure</h1>
This tutorial demonstrates how to set up and manage an on-premises-style Active Directory environment in Microsoft Azure. It focuses on managing user accounts and configuring Group Policies (GPOs) to simulate real-world enterprise scenarios.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Group Policy Management Console
- Command Line

<h2>Prequisites</h2>
  
- Preparing on-premises Active Directory within Azure VMs
- Configuring on-premises Active Directory within Azure VMs

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- 🔐 Dealing with Account Lockouts
- ⚙️ Configuring Group Policy for Lockouts
- 🔓 Unlocking and Resetting Accounts
- 🚫 Enabling and Disabling User Accounts
- 📊 Observing Security Logs (DC & Client)

<h2>Configuring an account lockout policy</h2>

<h4>Open the Group Policy Management Console (GPMC)</h4>

<p>First, log in to DC-1 (Domain Controller) using the mydomain.com\jane_admin account
  
Click Start, type gpmc.msc, and hit Enter.
This will open up the Group Policy Management Console, which is where we'll manage all the GPOs for the domain.</p>


https://github.com/user-attachments/assets/0625a0f4-93c3-4fc4-b53a-eb982b2b9cd2

<h4>Create or Edit a Group Policy Object (GPO)</h4>

<p>Inside GPMC, we’re either gonna edit or create a new GPO, but since we already have one set up, we’re just gonna edit the existing GPO for this.</p>

<img width="753" alt="Screenshot 2025-07-06 at 7 15 59 PM" src="https://github.com/user-attachments/assets/348021b8-fc54-494a-96c8-9cb091393abe" />

<h4>Navigate to the Account Lockout Policy Settings</h4>

<p>In the Group Policy Management, right-click the Default Domain Policy and hit Edit then:
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.</p>

https://github.com/user-attachments/assets/9020574e-6405-4775-be9e-60b1cd458bad

<H4>Configure Account Lockout Policy Settings</H4>

<P>You’ll see three main settings we need to configure:

Account Lockout Duration – This is how long the account stays locked before it unlocks on its own.
Just double-click it, check "Define this policy setting", and set it to something like 30 minutes.

Account Lockout Threshold – This is how many wrong password attempts it takes to trigger a lockout.
Double-click, define the setting, and set it to 3 attempts.

Reset Account Lockout Counter After – This resets the failed login count back to zero if no more bad attempts happen.
Set this one to 15 minutes after defining it.

We’ll also enable administrator account lockout just to make sure even privileged accounts are protected. This adds an extra layer of security in case someone tries to brute-force the admin login.

</P>

<p>Once you set the lockout duration, it’ll automatically fill in the other values like the threshold and the reset timer based on what you enter, you can still change them manually if you want, but it dials them in by default.</p>


https://github.com/user-attachments/assets/8be82208-beaa-4b53-be25-44efc7b7825b

<h4>Update Group Policy</h4>

<p>We’re gonna log in as admin and manually update the Group Policy on Client-1 so the new GPO takes effect right away otherwise, you’d have to wait around 90 minutes for it to apply on its own.</p>

<p>So log in as jane_admin in Client-1 and open up the comman-line and type in gpupdate /force</p>



https://github.com/user-attachments/assets/20cd7d81-4dd0-4f7e-8ef2-89c6458ec585


<p>So now we’re gonna pick one of the 10,000 users we created in the previous project, and test out the account lockout policy we just set. We’ll try logging in with the wrong password a few times and observe how the policy kicks in once the threshold is hit.</p>


https://github.com/user-attachments/assets/56041bcd-93f5-465e-b909-9cc66f9c55d5

<h4>Observe that the account has been locked out</h4>
  
<p>So now we’re gonna try logging into client-1 with the wrong password three times and see what happens. This should trigger the Account Lockout Policy we set earlier, and we’ll be able to observe the account getting locked out once it hits the threshold.</p>


https://github.com/user-attachments/assets/969c8582-4c09-441c-8b19-73bcc29ada69

<h4>Unlock the account</h4>

<p>Now we’re gonna unlock the account that got locked out from the failed login attempts. We’ll do this from the Domain Controller using Active Directory Users and Computers.</p>



https://github.com/user-attachments/assets/ae1e8221-4b09-445b-902a-f4a0364626f1

<h4>Password Reset</h4>

<p>Now we’re gonna reset the password for that user. Still in Active Directory Users and Computers, just right-click the account, hit Reset Password, and set a new one.</p>


https://github.com/user-attachments/assets/cd212717-d116-48d6-96dd-07dd226616f8

<h4>Log in the unlocked account</h4>

<p>Now we’re gonna try logging in again using the unlocked user account with the new password, just to make sure everything’s working properly.</p>


https://github.com/user-attachments/assets/ffef0146-f9a5-42ad-9e32-8ee56dd75a13

<h4>Managing User Access: Enable & Disable Accounts</h4>

<p>We can also enable or disable user accounts within Active Directory Users and Computers. This is useful for temporarily restricting access without deleting the account.

To do this:

Right-click the user account, then select Disable Account to deactivate it.

To re-enable it, right-click the disabled account and choose Enable Account.

This action applies immediately and can be used for access control or account management during maintenance or investigations.</p>


https://github.com/user-attachments/assets/0ba07340-a9a9-4186-b711-79501413acae

<p>Now if we log out of tuv.duj and try to reconnect, it shouldn’t work since the account is disabled. This is how we can confirm that the disable account setting is active and working.</p>


https://github.com/user-attachments/assets/0183b4b0-8415-47b7-9fe4-b8c7c11cfacc

<p>Now we can go ahead and enable the account again, and then try logging in to make sure access is restored and everything’s working like it should.</p>


https://github.com/user-attachments/assets/9e072f04-c839-4835-a201-78db26ebab35

<h4>Observing Logs</h4>

<p>So go ahead and run eventvwr.msc as an administrator on Client-1. This will open up Event Viewer, where we can dig into the Security logs and check for any failed login attempts or lockout events tied to the user we tested earlier.</p>

<p>If you right-click on "Security" in Event Viewer and click "Find", you can search for the username — in this case, tuv.duj. That’ll pull up all the logs tied to that account, making it easier to spot things like failed logins, lockouts, or successful logins.</p>



https://github.com/user-attachments/assets/1996b164-c6ef-490f-bcaa-dcf0e5746aa3


<h2>Conclusion</h2>

<p>This wraps up the project on account lockout policies, user access control, and log monitoring in Active Directory. We walked through setting up lockout thresholds, testing failed logins, unlocking and resetting accounts, and even enabling/disabling users. On top of that, we used Event Viewer to track everything in real time from both the domain controller and the client. This lab gave a solid look at how account security works in a real environment and laid the groundwork for more advanced user and group policy management.

</p>
