# Active-Directory-Lab
![Blue   Yellow Professional Future Technology Presentation](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/be7f93bb-d185-4e87-94fc-859a4342c73a)


## Objective
In this lab, I used two virtual machines on VirtualBox to simulate an Active Directory environment using Windows Server 2019 and Windows 10. The goal was to configure a fully functional AD environment by setting up a domain controller and a client machine. After configuring IP Addressing, DNS, Active Directory Domain Services (AD DS), Network Address Translation (NAT) with Remote Access Service (RAS), and Dynamic Host Configuration Protocol (DHCP), I used a PowerShell script to create 1,000 users in Active Directory. Finally, I created a client machine running Windows 10, connected it to the virtual network, and added it to the Active Directory domain.
## Skills Learned
- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

## Tools Used
- VirtualBox for creating and managing virtual machines.
- Windows Server 2019 ISO for setting up the domain controller.
- Windows 10 ISO for setting up the client machine.
- PowerShell for automating user creation and management.

## Steps

## step 1: Setting up VirtualBox:
- Download VirtualBox from here: [Link](https://www.virtualbox.org/wiki/Downloads)
- Documention of how to use: [Link](https://www.virtualbox.org/manual/ch01.html)
- [Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)
- [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10)

## Step 2: Create the Domain Controller VM
1. Open VirtualBox and click New.
2. Name the VM DomainController, set the type to Microsoft Windows, and version to Windows 2019 (64-bit).
3. Allocate at least 2GB of RAM (2048MB) and create a new virtual hard disk with the default settings.
![Screenshot 2024-06-30 210408](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/297eb4a7-2839-406e-898c-91a33a69de9b)

4. Configure the VM:
- Go to Settings > System > Processor and increase the number of processors to 2 or more.
- Go to Settings > Network:
            - Adapter 1: Set to NAT.
            - Adapter 2: Enable and set to Internal Network.
![Screenshot 2024-06-30 210757](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/96d91fb9-efdf-470f-9fae-a965f5f2f1ff)

5. Start the VM and select the Windows Server 2019 ISO as the startup disk.
6. Follow the installation steps to install Windows Server 2019. 
## Step 3: Configure the Domain Controller

### Setting Static IP for Internal Network
![Screenshot 2024-07-05 152512](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/c546d130-e448-42a5-a43c-54db7bd6e001)
### Configure the network settings to set a static IP for the internal network adapter (e.g., 172.16.0.1).
![Screenshot 2024-06-30 214402](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/f9bfae5f-197f-4e7c-968e-75f35f028949)

### Installing AD DS
1. Open Server Manager, add roles and features >> Select Active Directory Domain Services and install
![Screenshot 2024-06-30 214632](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/20ab8d52-0c36-4151-88fd-00127c448f6e)

2. After installation, promote the server to a domain controller and create a new forest (e.g., mydomain.com).
![Screenshot 2024-07-01 134430](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/5fa6c885-b1a3-4ab4-b320-abea325bc503)

3. Create an Organizational Unit named _ADMINS to add yourself as a domain administrator.

## Step 3: Set Up RAS/NAT for Internet Access
### Installing Remote Access
- Go to Add roles and features, select Remote Access, and then Routing.
- Install the roles and features.
- Open Routing and Remote Access from Server Manager.
- Configure NAT to allow internal clients to access the internet using one public IP address.
![Screenshot 2024-06-30 222141](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/e128db4f-a31d-4161-b698-58c363e05ebc)

## Step 4: Set Up DHCP
### Installing DHCP
- Go to Add roles and features, select DHCP Server, and install.
- Open Tools > DHCP to set up the scope.
- Create a DHCP scope to give IP addresses in the range 172.16.0.100 to 172.16.0.200 with the correct subnet mask.
![Screenshot 2024-06-30 222818](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/10441646-4cfb-4582-ba38-d5cf5a93bf97)

- Set the default gateway to the domain controller's IP (172.16.0.1).

## Step 5: Create the Client VM
1. Open VirtualBox and click New.
2. Name the VM Client1, set the type to Microsoft Windows, and version to Windows 10 (64-bit).
3. Allocate at least 2GB of RAM (2048MB) and create a new virtual hard disk with the default settings.
4. Configure the VM:
- Go to Settings > System > Processor and increase the number of processors to 2 or more.
- Go to Settings > Network: Adapter 1: Enable and set to Internal Network.
![Screenshot 2024-06-30 231019](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/55b3330b-96ec-4a53-befa-29a050fd47c0)

5. Start the VM and select the Windows 10 ISO as the startup disk.
6. Follow the installation steps to install Windows 10.

## Step 6: Join the Client to the Domain
1. Ensure the client VM is set to use DHCP and receives an IP address from the DHCP server on the domain controller.
  ![Screenshot 2024-07-01 144627](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/944f512f-66b1-4523-930f-bf4d935ea682)

2. Open system properties, change settings to join the domain (mydomain.com).
![Screenshot 2024-07-01 204742](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/d121baac-c7e7-42b4-9d32-cf106409851c)

3. Restart the client VM.
4. Log in to the client VM using domain credentials.
## Step 7: Create Users with PowerShell Script
1. On DomainController VM
2. We will use a PowerShell script to add 1,000 users to Active Directory. To streamline this process, a random name generator script was used to populate a .txt file with the 1,000 names. This file will be referenced in the PowerShell script for user creation.
- [PowerShell script and name.txt file](https://github.com/erickjsanz/PowerShell-Files))
- Important: Ensure to set the execution policy to unrestricted before running the PowerShell script:
![Screenshot 2024-06-30 225752](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/2ff1eaa9-3b57-4a27-a5e4-c046fa596c0e)

- You may edit the names.txt and add your own name or any name. In my case, I added Elliot Alderson for fun.

![Screenshot 2024-06-30 224408](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/c16a7c1b-b08c-426c-ba26-c8d3a6a6a76b)

### PowerShell Script for Bulk User Creation

![Screenshot 2024-06-30 225558](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/6c28e689-3753-4bea-bb65-6971e6d293bd)

- Once the script has run and the 1,000 users have been added to Active Directory, you can further experiment by adding users to different Organizational Units (OUs) and assigning various permissions.

![Screenshot 2024-06-30 230204](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/1b2f59b1-da86-4cab-b0a8-424d78a0dcd0)

- The added users on Active Directory

![Screenshot 2024-06-30 230426](https://github.com/erickjsanz/Active-Directory-Lab/assets/7691426/ea251ba2-6eb2-4a63-bb91-5169f6be33f6)

## Step 8: Testing Users
1. On Client1, log in with one of the newly created domain user accounts.
2. Verify successful login and access to domain resources.

## Conclusion

This project sets up a basic Active Directory environment using VirtualBox, emulating a corporate network. It helps in understanding Active Directory (AD), Dynamic Host Configuration Protocol (DHCP), Domain Name System (DNS), and network configuration in a virtualized environment. Additionally, it demonstrates the use of PowerShell to automate the creation and management of large numbers of users in Active Directory, providing valuable experience in scripting and automation within a Windows Server environment.






