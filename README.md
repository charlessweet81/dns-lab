<p align="center">
</p>

<h1>DNS Lab</h1>
<p>
This tutorial walks you through the process of setting up a Domain Controller (DC) and a Client in Microsoft Azure, showcasing how to configure network connections and test functionality in a controlled virtual environment. By deploying a Windows Server as a Domain Controller (DC-1) and a Windows 10 machine as a client (Client-1), we’ll explore how to configure private networks, assign static IPs, and test connectivity between virtual machines.

Through this step-by-step guide, you’ll gain practical experience in:

- Establishing a domain environment in Azure.
- Configuring DNS settings and testing network connectivity.
- Troubleshooting and ensuring seamless communication between virtual machines.

Let's dive in. 

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10

<h2>Overview</h2>

  - Part I: Configuring Active Directory Group Policy 
    - Step 1: Open Group Policy Management Console (GPMC) 
    - Step 2: Create/Edit Group Policy Object (GPO)
    - Step 3: Navigate to the Account Lockout Policy Settings 
    - Step 4: Configure Account Lockout Policy Settings
    - Step 5: Force Update Group Policy
    - Step 6: Observe Lockout via Remote Desktop
    - Step 7: Unlock the Account
    - Step 8: Reset the Password
  - Part II: Disabling & Re-enabling Accounts
    - Step 1: Step 1: Disable the Account
    - Step 2: Re-enable the Account

<h2>Steps</h2>
<h3>Part I: Configuring Active Directory Group Policy</h3>

<h4>Step 1: Open Group Policy Management Console (GPMC) </h4>

<img src="https://i.imgur.com/6EhRwko.png" height="80%" width="80%" alt=""/>

- Open Group Policy Management Console (GPMC):
  - Log in to a domain controller with administrative privileges.
  - Press Win + R, type gpmc.msc, and hit Enter.

<h4>Step 2: Create or Edit a Group Policy Object (GPO):</h4>

<img src="https://i.imgur.com/TqDhhN8.png" height="80%" width="80%" alt=""/>

- Navigate to your domain in the GPMC.
- Right-click on the domain or an Organizational Unit (OU) where you want to apply the policy and select Create a GPO in this domain, and Link it here... or edit an existing GPO.


<h4>Step 3: Navigate to the Account Lockout Policy Settings </h4>

<img src="https://i.imgur.com/EDn6w7S.png" height="80%" width="80%" alt=""/>

- Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Account Policies -> Account Lockout Policy

<h4>Step 4: Configure Account Lockout Policy Settings </h4>

<img src="https://i.imgur.com/Jkx5zKj.png" height="80%" width="80%" alt=""/>

- Configure the following settings:
  - Account lockout threshold: Set to 5 invalid login attempts.
  - Account lockout duration: Set to 30 minutes (or your preference).
  - Reset account lockout counter after: Set to 10 minutes.
  - Save and close the GPO editor.
- Force the policy update:
  - Run gpupdate /force on the Domain Controller and the client machine.
 
<h4>Step 5: Force Update Group Policy</h4>

<img src="https://i.imgur.com/1i90Xk4.png" height="80%" width="80%" alt=""/>

- You can wait ~90 minutes for the group policy to update itself or manually force the group policy to update.
  - Login to Client-1 as an admin
  - Open Command Prompt and type 'gpudate /force'
 
<h4> Step 6: Observe Lockout via Remote Desktop</h4>
<img src="https://i.imgur.com/WNhQXTJ.png" height="80%" width="80%" alt=""/>

- Try logging in with an incorrect password enough times to trigger lockout

<h4>Step 6: Observe the Ticket Lockout in Active Directory</h4>

<img src="https://i.imgur.com/WNhQXTJ.png" height="80%" width="80%" alt=""/>

- In ADUC, navigate to the locked account:
- Locate the user (e.g., johndoe) > Double-click the account.
- Observe that the account is locked (status shown in the account properties).
- Note the "Account is locked out" checkbox.

<h4>Step 7: Unlock the Account</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- In ADUC, unlock the account:
  - Right-click the user > Click Properties > Go to the Account tab.
  - Uncheck Account is locked out and click OK.
 
<h4>Step 8: Reset the Password</h4>

<img src="https://i.imgur.com/Z8WZjHH.png" height="80%" width="80%" alt=""/>

- Reset the user’s password:
- Right-click the user in ADUC > Click Reset Password.
- Enter a new password and click OK.

<h3> Part II: Enabling & Disabling Accounts </h3>
<h4>Step 1: Disable the Account</h4>

<img src="https://i.imgur.com/F3DQ7yv.png" height="80%" width="80%" alt=""/>

- In ADUC, disable the account:
  - Right-click the user > Click Disable Account.
- Test login:
  - Attempt to log in with the disabled account on a client machine.
  - Observe the error message: "Your account has been disabled. Please contact your administrator."

<h4>Step 2: Re-enable the Account</h4>

<img src="https://i.imgur.com/fhNbn9G.png" height="80%" width="80%" alt=""/>

- In ADUC, re-enable the account:
  - Right-click the user > Click Enable Account.
- Test login:
  - Log in with the account to confirm it works.
