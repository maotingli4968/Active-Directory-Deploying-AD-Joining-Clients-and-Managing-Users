<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial shows how to deploy on-premises active directory within azure computer.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>List of Prerequisites</h2>

- Ensure you have two virtual machines already deployed in Azure:
  - <b>DC-1</b>: Windows Server 2022
  - <b>Client-1</b>: Windows 10 Pro
- Both VMs must be <b>powered on</b> in the Azure Portal.
- <b>Client-1 DNS</b> must already be configured to point to <b>DC-1’s private IP address</b>.




<h2>Installation Steps</h2>

<h2>Install Active Directory on DC-1</h2>

- Log into <b>DC-1</b> and install <b>Active Directory Domain Services (AD DS)</b> through <b>Server Manager</b>.
- Once the role is installed, promote the server to a <b>Domain Controller</b> by creating a new forest called <b>mydomain.com</b>.
- This configures <b>DC-1</b> as the central authority for authentication and directory services in your environment.
- After the server reboots, log back in using the new domain account: <b>mydomain.com\labuser</b>.

  <img width="975" height="708" alt="image" src="https://github.com/user-attachments/assets/933431f5-b9e6-4ec0-bd3c-702c6aea654e" />


  <img width="975" height="718" alt="image" src="https://github.com/user-attachments/assets/742ab97a-b41c-4ab9-b61f-6f0aa7899b88" />


<h2>Create Domain Admin User</h2>

- After AD is set up, open <b>Active Directory Users and Computers (ADUC)</b> on <b>DC-1</b>.
- Create an Organizational Unit (OU) named <b>_EMPLOYEES</b>.
- Create another OU named <b>_ADMINS</b>.
- Inside <b>_ADMINS</b>, create a new user account:
  - Name: <b>Jane Doe</b>
  - Username: <b>jane_admin</b>
  - Password: <b>CyberLab123!</b>
- Add <b>jane_admin</b> to the <b>Domain Admins</b> security group.
- From this point forward, log in as <b>mydomain.com\jane_admin</b> for all administrative tasks.


<img width="975" height="691" alt="image" src="https://github.com/user-attachments/assets/6f722c12-2f15-4660-89d5-4b040dfd46a8" />

<img width="975" height="426" alt="image" src="https://github.com/user-attachments/assets/0afe9682-f9f5-4c84-8f51-c7e21f0b73f3" />

<h2>Join Client-1 to Domain</h2>

- Move on to <b>Client-1</b>.
- In the Azure Portal, confirm its <b>DNS</b> is set to <b>DC-1’s private IP address</b>.
- Restart the VM, then log into <b>Client-1</b> as the local admin (<b>labuser</b>).
- Join <b>Client-1</b> to the domain <b>mydomain.com</b> (this will require another restart).
- Afterward, verify the machine appears in <b>ADUC</b> from <b>DC-1</b>.
- Create a new OU called <b>_CLIENTS</b> and move <b>Client-1</b> into it.

  <img width="975" height="758" alt="image" src="https://github.com/user-attachments/assets/a8ba7970-9135-4441-87e8-5b9153386157" />


   <h2>Set up Remote Desktop for Domain Users</h2>

- With <b>Client-1</b> now domain-joined, enable access for domain users.
- Log into <b>Client-1</b> as <b>mydomain.com\jane_admin</b>.
- Open <b>System Properties</b> → <b>Remote Desktop</b>.
- Add the <b>Domain Users</b> group to the list of allowed users.
- This allows standard domain users to log into <b>windows-vm</b> through <b>RDP</b> (in production, this is typically controlled through <b>Group Policy</b>).

  <img width="894" height="827" alt="image" src="https://github.com/user-attachments/assets/57be889d-3ede-4fec-b729-68008851ef31" />


  <h2>Create Additional Users with PowerShell Script</h2>

- Log back into <b>DC-1</b> as <b>jane_admin</b>.
- Open <b>PowerShell ISE</b> as <b>Administrator</b>.
- Create a new script file, paste the provided script, and execute it.
- The script automatically generates multiple user accounts under the <b>_EMPLOYEES</b> OU.
- Once complete, confirm the accounts in <b>ADUC</b>.
- Test logging into <b>Client-1</b> with one of the newly created users using the assigned password from the script.

  <img width="975" height="723" alt="image" src="https://github.com/user-attachments/assets/c169370a-fca5-4754-9982-d6b4980d4632" />

  <img width="975" height="686" alt="image" src="https://github.com/user-attachments/assets/583a4c48-9384-48f6-8eda-cd530d5c521c" />

  <img width="952" height="344" alt="image" src="https://github.com/user-attachments/assets/abfd5f29-be09-4d3c-8ae9-8d9cf33c1712" />


  <h2>Save Money – Stop VMs</h2>

- Once the lab is finished, you don’t need to delete the machines.
- Instead, stop <b>DC-1</b> and <b>Client-1</b> from the Azure Portal.
- This halts billing for compute resources while keeping your configuration intact for future labs.
- You’ll use these same VMs again later, so stopping them instead of deleting them is the best way to save money while preserving your work.





