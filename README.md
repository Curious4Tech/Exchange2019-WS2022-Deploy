# Step-by-Step Guide to Install Exchange Server 2019 on Windows Server 2022

A comprehensive step-by-step guide to install Exchange Server 2019 on Windows Server 2022.


## Prerequisites

1. **Hardware Requirements**
   - Processor: 64-bit processor that supports x64 architecture
   - Memory: 8+ GB RAM (16+ GB recommended)
   - Disk space: At least 30GB for installation + appropriate storage for mailbox databases
   - File system: NTFS for all Exchange data

2. **Software Requirements**
   - Windows Server 2022 with Desktop Experience (Standard or Datacenter edition)
   - Exchange Server 2019 CU12 or later (required for Windows Server 2022 compatibility)
   - .NET Framework 4.8 or later
   - Active Directory domain environment

## Step 1: Initial Windows Server 2022 Setup

1. Install Windows Server 2022 with Desktop Experience
2. Apply all available Windows updates
3. Configure computer name (important: don't change the name after Exchange is installed)
4. Join the server to your Active Directory domain
5. Configure static IP address and DNS settings (point to domain controllers)

![image](https://github.com/user-attachments/assets/53bb467b-4916-433a-848d-88814c959ac9)

6. Ensure Remote Registry service is set to Automatic

![image](https://github.com/user-attachments/assets/6eee5382-1054-4631-aa79-1fc89d8e6735)

7. Restart the server

## Step 2: Prepare Active Directory

1. Ensure you have the following permissions:
   - Schema Admins group membership
   - Enterprise Admins group membership
   - Domain Admins group membership

2. Mount the Exchange Server 2019 ISO file:
   ```
   Mount-DiskImage -ImagePath C:\path\to\ExchangeServer2019.iso
   ```

![Ex1](https://github.com/user-attachments/assets/d64a66fc-a897-41b1-9903-d2f05a941894)

3. Prepare Active Directory schema:
   ```
   E:\Setup.exe /PrepareSchema /IAcceptExchangeServerLicenseTerms_DiagnosticDataON
   ```

![Ex2](https://github.com/user-attachments/assets/712a8e1b-afe7-42b5-ac20-f1d731dd0a59)

4. Prepare Active Directory:
   ```
   E:\Setup.exe /PrepareAD /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /OrganizationName:"YourOrgName"
   ```

![Ex3](https://github.com/user-attachments/assets/906031aa-2d5c-4c41-874b-98cfebd58e58)

5. Prepare all domains that will contain Exchange servers or mailboxes:
   ```
   E:\Setup.exe /PrepareDomain /IAcceptExchangeServerLicenseTerms_DiagnosticDataON
   ```

![Ex4](https://github.com/user-attachments/assets/a81b1ed1-613e-4eb2-afd2-869f698ba707)

   Or prepare all domains in the forest:
   ```
   E:\Setup.exe /PrepareAllDomains /IAcceptExchangeServerLicenseTerms_DiagnosticDataON
   ```

![Ex5](https://github.com/user-attachments/assets/f8823e20-89f0-4f79-8736-62a50505315a)

6. Wait for Active Directory replication to complete

## Step 3: Install Exchange Server Prerequisites

1. Open PowerShell as Administrator and run:
   ```
   Install-WindowsFeature Server-Media-Foundation, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-HTTP-Activation45, NET-WCF-Pipe-Activation45, NET-WCF-TCP-Activation45, NET-WCF-TCP-PortSharing45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
   ```

![Ex6](https://github.com/user-attachments/assets/45a31a36-6d89-42df-92f9-f952bded206e)

2. Install .NET Framework 4.8 (if not already installed)

3. Install Visual C++ Redistributable Package for Visual Studio 2012: [Dowlod from here](https://www.microsoft.com/en-us/download/details.aspx?id=30679)


4. Install Visual C++ Redistributable Packages for Visual Studio 2013

5. Install Unified Communications Managed API 4.0 Runtime (download from Microsoft or find in the \UCMARedist folder on the Exchange media)

6. Install the IIS URL Rewrite Module

![image](https://github.com/user-attachments/assets/67606cb3-6c64-4d80-858c-cf35a751631e)
 
7. Restart the server

## Step 4: Install Exchange Server 2019

### Method 1: Using the GUI Setup Wizard

1. Mount the Exchange Server 2019 ISO (if not already mounted)
2. Right-click on Setup.exe and select "Run as administrator"

![Ex7](https://github.com/user-attachments/assets/3fdfc154-b68d-4e37-a65f-571d8751f910)

3. On the "Check for Updates" page, choose whether to check for updates

![Ex8](https://github.com/user-attachments/assets/48dd76c7-6176-49cb-b5f2-81c2adac5b22)

4. On the "Introduction" page, click Next
5. Accept the license agreement and click Next

![Ex9](https://github.com/user-attachments/assets/0034ab35-d3ca-4a6d-9854-297b27de711c)

6. Choose whether to use recommended settings and click Next

![Ex10](https://github.com/user-attachments/assets/a84a90ff-ff1c-4e62-a70f-0d1d60800b6a)

7. Select "Mailbox role" (Management tools are selected automatically)

![Ex11](https://github.com/user-attachments/assets/33314d58-3ce7-4d8e-81de-995cd3a45471)

8. Check "Automatically install Windows Server roles and features that are required to install Exchange Server"
9. Specify installation path and click Next

![Ex12](https://github.com/user-attachments/assets/bf7f8fbb-04f7-4e42-a4e2-3020f072a1bf)

10. Configure malware protection settings and click Next

![Ex13](https://github.com/user-attachments/assets/d9cd1337-804e-4e9c-b0f3-c8158813b9a5)

11. Wait for readiness checks to complete and resolve any errors
12. Click Install to begin installation

![Ex14](https://github.com/user-attachments/assets/051774d7-2371-488d-96f1-acac4130a0c5)
   
13. After installation completes, click Finish

![Ex16](https://github.com/user-attachments/assets/de59eb7d-c0be-4d53-a4a0-2f39e2a50df5)

14. Restart the server

### Method 2: Using Unattended Setup (Command Line)

1. Mount the Exchange Server 2019 ISO (if not already mounted)
2. Open Command Prompt as administrator
3. Run the following command:
   ```
   E:\Setup.exe /Mode:Install /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /Roles:Mailbox /InstallWindowsComponents
   ```
4. Wait for installation to complete
5. Restart the server

## Step 5: Post-Installation Tasks

1. Verify installation by opening Exchange Management Shell and running:
   ```
   C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
   Get-ExchangeServer
   Get-ExchangeServer | Format-List Name, Edition, AdminDisplayVersion
   ```

![image](https://github.com/user-attachments/assets/eebfd6a6-a90e-42c3-a95b-7ac0d4c96038)

2. Access Exchange Admin Center by opening a browser and navigating to:
   ```
   https://servername/ecp
   ```
![Ex18](https://github.com/user-attachments/assets/5a5e281e-f03d-45d0-86c7-f454b622675c)


7. After authenticating with AD Admin credentials, you can now access the Admin Center Inferface.

![Ex17](https://github.com/user-attachments/assets/0a72068a-21f0-4f96-803c-e4aa4f955b0f)

8. Apply the latest security updates and cumulative updates

9. Configure backup and recovery solutions

10. Set up monitoring and maintenance plans

By following these steps, you'll have a fully functional Exchange Server 2019 installation on Windows Server 2022. Remember to regularly check for and apply the latest updates to maintain security and performance.
