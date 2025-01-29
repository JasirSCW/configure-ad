![image](https://github.com/user-attachments/assets/524ec0f6-de5d-4d72-b991-cfa6ab44a5fe)




# On-Premises Active Directory Deployed in the Cloud (Azure)

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

## **Environments and Technologies Used**
- **Microsoft Azure:** Virtual Machines/Compute
- **Remote Desktop Protocol (RDP)**
- **Active Directory Domain Services (AD DS)**

## **Operating Systems Used**
- **Windows Server 2022:** 2 vCPUs, 8GB RAM
- **Windows 10 Pro (22H2):** 2 vCPUs, 8GB RAM

---

## **High-Level Deployment and Configuration Steps**
1. Install Active Directory on DC-1
2. Create a Domain Admin user for the domain
3. Join Client-1 to the domain

---
## Deployment and Configuration Steps

We'll set up two VMs on Azure: DC-1 as a Windows Server for the domain controller and Client-1 as a Windows Pro client. Both machines will be connected to the same virtual network. After that, we'll log into DC-1 to proceed.


![Active 1](https://github.com/user-attachments/assets/47e5e91c-8833-4a08-a4b3-dfe2aebd89cc)

Server Manager should open automatically; if not, click Start and select the Server Manager icon. Choose "Add Roles and Features," keep the defaults, and select "Active Directory Domain Services" under Server Roles.

![Active 2](https://github.com/user-attachments/assets/069c5232-6586-4789-9298-053490b17f63)

Check "Restart the destination server automatically if required," then click "Install." Once the installation is complete, return to Server Manager and click the flagged icon in the top-right corner. Select "Promote this server to a domain controller."

![Active 3](https://github.com/user-attachments/assets/5e5bc705-73d7-4256-af1b-6557f330ebb2)

In the Deployment Configuration, select "Add a new forest" and enter mydomain.com (or any domain name you prefer).

![Active 9](https://github.com/user-attachments/assets/3f55ee1b-a38d-4fb0-a485-aac7002be536)

Click "Next" and set a Directory Services Restore Mode password (you can choose anything, as it’s rarely used). Continue the configuration, and the system will automatically sign you out as Active Directory finishes installation.

Next, log into the Client-1 machine. Logging in with your usual credentials will fail because no domain is specified. Instead, use: mydomain.com\your-username and enter your password.

Once logged in, switch back to DC-1. The next step is to create a Domain Admin user. This role manages the entire domain and is critical. Click Start -> Windows Administrative Tools -> Active Directory Users and Computers. Right-click mydomain.com -> New -> Organizational Unit (OU).

![Active 4](https://github.com/user-attachments/assets/466aa126-d7bf-45d4-b94d-5a8c91961109)

Name the new OUs _EMPLOYEES and _ADMINS. To create an admin user, right-click _ADMINS -> New -> User. Name the user Jane Doe with the username jane_admin, and set a password of your choice.

Although Jane Doe is in the _ADMINS OU, she isn't a Domain Admin yet. To grant those privileges, click on _ADMINS, right-click Jane Doe, and select Properties -> Member Of. Type Domain Admins in the box, click Check Names, then OK, and apply the settings.

![Active 5](https://github.com/user-attachments/assets/d343d9c6-8565-41b5-bee5-9e370966ac5c)

Next, update Client-1's DNS settings to point to the private IP address of DC-1. This ensures proper communication within the domain.

![Active 6](https://github.com/user-attachments/assets/11fa6ff9-9a76-4053-9354-fda200fa61a8)

Log back into Client-1 as Jane using mydomain.com\jane_admin and your password. Right-click the Start menu -> Settings -> Rename this PC (advanced). Under Computer Name, click Change, select Domain, and enter mydomain.com, then click OK.

![Active 7](https://github.com/user-attachments/assets/d99991c2-17ef-4c3f-9960-17c9febd5d5e)

Restart Client-1 to apply the changes. Return to DC-1 and open Active Directory Users and Computers. Click on mydomain.com -> Computers, and you should see Client-1 listed, confirming it has joined the domain.

![Active 8](https://github.com/user-attachments/assets/e5423803-4cf6-4a2e-ab17-29a1f6387334)
