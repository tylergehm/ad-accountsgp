  <img src="https://raw.githubusercontent.com/tylergehm/ad-accountsgp/main/az2.jpg" alt="GitHub banner" style="max-width:100%;height:auto;" />
</p>
</p>

<h1>Active Directory: User & Group Management in Azure</h1>
In the Azure Active Directory User & Group Management project, Group Policies (GPOs) are centralized settings applied via Active Directory to manage user and computer configurations, enforcing security and behavior across the domain. The lab continues with account lockout testing: configuring a GPO to lock accounts after 5 failed attempts, attempt login with a bad password, then observe the lock out being triggered. Then the account will be unlocked, reset the password, and confirm successful login. Next, an account will be disabled in Active Directory, demonstrating the lockout, then re-enabling the account and verifying access. The authentication and lock out events on both the domain controller and client machine. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Group Policy Management Console
- Command Line

<h2>Prequisites</h2>
  
- Preparing on-premises Active Directory within Azure VMs
- Configuring on-premises Active Directory within Azure VMs

For additional information on how these virtual machines were set up, visit [Configuring Active Directory within Azure VMs](https://github.com/tylergehm/configure-ad)


<h2>Operating Systems Used </h2>

- Windows Server 2025
- Windows 11 Pro (22H2)
- Windows 11 Home (Host Machine)

<h2>Configuration Steps</h2>

- Step 1 - Configure Group Policy for Account Lockout
- Step 2 - Attempt to log in with bad password
- Step 3 - Observe the lock out and reset password
- Step 4 - Enable and disable an account
- Step 5 - Observe logs in Event Viewer

<h2>Step 1 - Configure Group Policy for Account Lockout</h2>
