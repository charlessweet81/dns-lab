<p align="center">
</p>

<h1>An, Local DNS Caching, and CNAME Records</h1>
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

  - Part I: A-Record Exercise
    - Step 1: Log into DC-1
    - Step 2: Log into Client-1
    - Step 3: Ping "mainframe" from Client-1
    - Step 4: Create an A-Record on DC-1
    - Step 5: Ping "mainframe" from Client-1
  - Part II: Local DNS Cache Exercise
    - Step 1: Update "mainframe" A-Record on DC-1
    - Step 2: Ping "mainframe" from Client-1
    - Step 3: View Local DNS Cache
    - Step 4: Flush the DNS Cache
    - Step 5: Verify Empty Cache
    - Step 6: Ping "mainframe" Again
  - Part III: CNAME Record Exercise
    - Create a CNAME Record on DC-1
    - Ping "search" from Client-1
    - Nslookup for "search" on Client-1
    - 

<h2>Exercise Steps</h2>
<h3>Part I: A-Record Exercise</h3>

<h4>Step 1: Log into DC-1</h4>

<img src="https://i.imgur.com/nTMpYVh.png" height="80%" width="80%" alt=""/>

- Open Remote Desktop Connection on your local machine or Azure interface.
- Enter the credentials for mydomain.com\jane_admin (e.g., username: jane_admin, password: [your password]).
- Connect to DC-1.

<h4>Step 2: Log into Client-1</h4>

<img src="https://i.imgur.com/nTMpYVh.png" height="80%" width="80%" alt=""/>

- Open Remote Desktop Connection on your local machine or Azure interface.
- Enter the credentials for mydomain.com\jane_admin (e.g., username: jane_admin, password: [your password]).
- Connect to Client-1.
  
<h4>Step 3: Ping "mainframe" from Client-1</h4>

<img src="https://i.imgur.com/HPGrSp5.png" height="80%" width="80%" alt=""/>

- Open a Command Prompt on Client-1 and type:
  - ping mainframe

Observation: The ping will fail because "mainframe" doesn't have a DNS record.

<h4>Step 4: Create an A-Record on DC-1</h4>

<img src="https://i.imgur.com/hPFLhH7.png" height="80%" width="80%" alt=""/>

- Type 'DNS' in dock search bar to open DNS Manager
- Click DC-1 > Forward Lookup Zones > MyDomain.com
- Right-click and choose New Host (A or AAAA)
- Enter DNS (e.g. 10.0.0.6)
- Click 'Add Host'
 
<h4>Step 5: Ping "mainframe" from Client-1</h4>

<img src="https://i.imgur.com/E9Qm8Ci.png" height="80%" width="80%" alt=""/>

In Powershell, ping 'mainframe'.

<h3>Part II: Local DNS Cache Exercise</h3> 

<h4>Step 1: Ping "mainframe" from Client-1</h4>

<img src="https://i.imgur.com/k3QJGSL.png" height="80%" width="80%" alt=""/>

- Return to DC-1 and modify the "mainframe" DNS record:
  - Change the IP address to 8.8.8.8.
  - Save the updated record.
 
<h4>Step 2: Ping "mainframe" from Client-1</h4>

<img src="https://i.imgur.com/jGjoS8f.png" height="80%" width="80%" alt=""/>

- On Client-1, execute:
  - Copy code
  - ping mainframe

Observation: The ping still resolves to the old IP address (the local DNS cache still holds the previous record).

<h4>Step 3: View Local DNS Cache:</h4>

<img src="https://i.imgur.com/d43QULl.png" height="80%" width="80%" alt=""/>

- Check the local DNS cache on Client-1:
  - ipconfig /displaydns

Observation: The cached record for "mainframe" will show the old IP address.

<h4>Step 4: Flush the DNS Cache</h4>

<img src="https://i.imgur.com/Y9YCaPl.png" height="80%" width="80%" alt=""/>

- Open Powershell as admin
  - ipconfig /flushdns

Observation: The cache is now cleared.

<h4>Step 5: Ping "mainframe" Again</h4>

<img src="https://i.imgur.com/lhLAkx4.png" height="80%" width="80%" alt=""/>

- Attempt another ping to "mainframe":
  - ping mainframe

Observation: The ping now resolves to the updated IP address 8.8.8.8.

<h3>C-Name Record Exercise</h3>

<h4> Step 1: Create a CNAME Record on DC-1</h4>

<img src="https://i.imgur.com/PQa3i7a.png" height="80%" width="80%" alt=""/>

- Return to DC-1.
- Open DNS Manager.
- Navigate to the forward lookup zone.
- Add a CNAME Record:
  - Alias Name: search
  - Target Host: www.google.com
- Save the record.

<h4> Step 2: Ping "search" from Client-1</h4>

<img src="https://i.imgur.com/TxtxgJU.png" height="80%" width="80%" alt=""/>

- Go back to Client-1 and execute:
  - ping search

Observation: The ping resolves to the IP address of www.google.com, as per the CNAME record.

<h4> Step 3: Nslookup for "search" on Client-1</h4>

<img src="https://i.imgur.com/amNn5Sl.png" height="80%" width="80%" alt=""/>

- On Client-1, type:
  - nslookup search

Observation: The result shows the alias "search" pointing to www.google.com.
