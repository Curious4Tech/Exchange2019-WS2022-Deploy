# Step-by-Step Guide to Install Exchange Server 2019 on Windows Server 2022

I'll provide you with a comprehensive guide to install Exchange Server 2019 on Windows Server 2022. Let me search for the most up-to-date information first.

Now I'll search for the specific installation steps for Exchange Server 2019 on Windows Server 2022:

Let me fetch the specific Windows Server 2022 prerequisites information:

Based on the information I've gathered, here's a comprehensive step-by-step guide to install Exchange Server 2019 on Windows Server 2022:

## Prerequisites

1. **Hardware Requirements**
   - Processor: 64-bit processor that supports x64 architecture
   - Memory: Minimum 128GB for production environments
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
6. Ensure Remote Registry service is set to Automatic
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

3. Prepare Active Directory schema:
   ```
   E:\Setup.exe /PrepareSchema /IAcceptExchangeServerLicenseTerms_DiagnosticDataON
   ```

4. Prepare Active Directory:
   ```
   E:\Setup.exe /PrepareAD /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /OrganizationName:"YourOrgName"
   ```

5. Prepare all domains that will contain Exchange servers or mailboxes:
   ```
   E:\Setup.exe /PrepareDomain /IAcceptExchangeServerLicenseTerms_DiagnosticDataON
   ```
   
   Or prepare all domains in the forest:
   ```
   E:\Setup.exe /PrepareAllDomains /IAcceptExchangeServerLicenseTerms_DiagnosticDataON
   ```

6. Wait for Active Directory replication to complete

## Step 3: Install Exchange Server Prerequisites

1. Open PowerShell as Administrator and run:
   ```
   Install-WindowsFeature Server-Media-Foundation, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-HTTP-Activation45, NET-WCF-Pipe-Activation45, NET-WCF-TCP-Activation45, NET-WCF-TCP-PortSharing45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
   ```

2. Install .NET Framework 4.8 (if not already installed)

3. Install Visual C++ Redistributable Package for Visual Studio 2012

4. Install Visual C++ Redistributable Packages for Visual Studio 2013

5. Install Unified Communications Managed API 4.0 Runtime (download from Microsoft or find in the \UCMARedist folder on the Exchange media)

6. Install the IIS URL Rewrite Module

7. Restart the server

## Step 4: Install Exchange Server 2019

### Method 1: Using the GUI Setup Wizard

1. Mount the Exchange Server 2019 ISO (if not already mounted)
2. Right-click on Setup.exe and select "Run as administrator"
3. On the "Check for Updates" page, choose whether to check for updates
4. On the "Introduction" page, click Next
5. Accept the license agreement and click Next
6. Choose whether to use recommended settings and click Next
7. Select "Mailbox role" (Management tools are selected automatically)
8. Check "Automatically install Windows Server roles and features that are required to install Exchange Server"
9. Specify installation path and click Next
10. Enter a name for your Exchange organization and click Next
11. Configure malware protection settings and click Next
12. Wait for readiness checks to complete and resolve any errors
13. Click Install to begin installation
14. After installation completes, click Finish
15. Restart the server

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
   Get-ExchangeServer | FL Name,ServerRole,Edition,AdminDisplayVersion
   ```

2. Access Exchange Admin Center by opening a browser and navigating to:
   ```
   https://servername/ecp
   ```

3. Configure Exchange certificates:
   - Create a certificate request
   - Obtain an SSL certificate from a trusted CA
   - Import and enable the certificate for IIS and SMTP services

4. Configure virtual directories:
   - Set internal and external URLs for OWA, ECP, ActiveSync, etc.
   - Configure authentication settings

5. Configure mail flow:
   - Set up send/receive connectors
   - Configure mail routing

6. Configure mailbox databases:
   - Create additional mailbox databases as needed
   - Configure database settings (retention, quotas, etc.)

7. Create test mailboxes and verify functionality

8. Apply the latest security updates and cumulative updates

9. Configure backup and recovery solutions

10. Set up monitoring and maintenance plans

By following these steps, you'll have a fully functional Exchange Server 2019 installation on Windows Server 2022. Remember to regularly check for and apply the latest updates to maintain security and performance.
