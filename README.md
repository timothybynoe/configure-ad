<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022, 2vCPUs, 8GM RAM
- Windows 10 Pro (22H2), 2vCPUs, 8GB RAM

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory
- Step 2: Create a Domain Admin user for the domain
- Step 3: Join client-1 to domain

<p>
Before setting up Active Directory, we need to deploy two virtual machines on Azure: one as a Windows Server for the domain controller and the other as a Windows Pro machine to serve as a client. We can name the server VM DC-1 and the client VM Client-1. Additionally, both machines must be configured to reside on the same virtual network.

Once that's done, we'll log into DC-1.
</p>
<br />
<p>
<img width="742" alt="image" src="https://github.com/user-attachments/assets/0c6f0928-4e12-46fd-8874-26e9a6c8ae18">
</p>
<p>
The Server Manager should open automatically. If it doesn’t, click Start, and you’ll find the Server Manager icon. Once open, click Add Roles and Features and proceed with the default settings until you reach the Select Server Roles step. Here, choose Active Directory Domain Services.
</p>
<br />
<p>
<img width="1439" alt="image" src="https://github.com/user-attachments/assets/611e8f17-952d-477e-81c0-c639723d1158">
</p>
<p>
Proceed with the setup, ensuring you check the option Restart the destination server automatically if required before clicking Install. Once the installation is complete, return to Server Manager and click the flagged icon in the top-right corner. Then, select the link that says Promote this server to a domain controller.
</p>
<br />
<p>
<img width="1290" alt="image" src="https://github.com/user-attachments/assets/a0643fec-083e-4b0c-8155-3ec6325ea61e">
</p>
<p>
In the deployment configuration section, select Add a new forest and enter mydomain.com. Note that you can choose any domain name you prefer.

Click Next and set a password for Directory Services Restore Mode. You can choose any password, as it is unlikely to be needed. Continue with the configuration process, and the system will automatically log you out as Active Directory completes its installation.

Next, switch to the Client-1 machine. Attempting to log in with your usual credentials will fail because no domain is specified, as DC-1 has just been configured as the domain controller. To log in, specify the domain in the following format: mydomain.com\yourusername, and then enter the password.

Once that's confirmed, we jump back log back into the DC-1 machine. The next step is to create a Domain admin user within the domain. The person will be able to administer the entire domain of users and is quite an important task. So we can click on Start -> Windows Administrative Tools -> Active Directory Users and Computers. Now right click on mydomain.com -> New -> Organizational Unit (OU).
</p>
<br />
<p>
<img width="473" alt="image" src="https://github.com/user-attachments/assets/705392dc-33b2-45d9-8b08-2aadcbccdba5">
</p>
<p>
Create a new Organizational Unit (OU) named _EMPLOYEES and another named _ADMINS. Next, create an admin user. Right-click on _ADMINS and select New -> User. Name this user Jane Doe with a username of jane_admin, and set a password of your choice.

Although Jane Doe is in the _ADMINS OU, she is not yet a Domain Admin since those privileges have not been assigned. To grant her Domain Admin rights, click on _ADMINS to locate Jane Doe. Right-click on her name, select Properties, and navigate to the Member Of tab. In the input box, type Domain Admins, click Check Names, then confirm with OK. Finally, apply the changes.
</p>
<br />
<p>
<img width="947" alt="image" src="https://github.com/user-attachments/assets/a6ffd34f-838b-4b38-a8ba-163722dade94">
</p>
<br />
Next, we need to update Client-1's DNS servers to point to the private IP address of DC-1.

<p>
<img width="742" alt="image" src="https://github.com/user-attachments/assets/d6f5f0bf-28de-4716-a021-faf5a18fb8af">
</p>
<br />
<p>
Log back into Client-1 as Jane, ensuring you include mydomain.com\ before the username. Right-click the Start menu and select Settings -> Rename this PC (advanced). Under Computer Name, click Change, enter mydomain.com in the Domain field, and click OK.
</p>

<p>
<img width="908" alt="image" src="https://github.com/user-attachments/assets/2c6fc2e5-878a-4781-a1f9-1b23705eabc7">
</p>
<p>
To apply these changes, restart Client-1. Once it restarts, return to DC-1 to verify that Client-1 has joined the domain. In Active Directory Users and Computers, expand mydomain.com, click on Computers, and you should see Client-1 listed there.
</p>
<br />























