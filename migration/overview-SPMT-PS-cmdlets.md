---
title: "Using PowerShell cmdlets to migrate to SharePoint Online"
ms.author: jhendr
author: JoanneHendrickson
manager: pamgreen
ms.date: 6/20/2018
ms.audience: ITPro
ms.topic: get-started-article
ms.prod: sharepoint-server-itpro
localization_priority: Priority
ms.collection:
- IT_SharePoint_Hybrid
- IT_SharePoint_Hybrid_Top
- IT_Sharepoint_Server
- IT_Sharepoint_Server_Top
- Strat_SP_gtc
ms.custom: 
description: "New Powershell cmdlets let you migrate to SharePoint Online."
---

# PowerShell Cmdlets for SPO Migration 

This article is about the new PowerShell cmdlets based on the SharePoint Migration Tool (SPMT) migration engine. They can be used to move files from file shares, SharePoint 2013 on-premises document libraries and list items to Office 365.

The PowerShell cmdlets provide the same functionalities as [SharePoint Migration Tool V2](introducing-the-sharepoint-migration-tool.md) .

 
> [!NOTE]
> Currently these PowerShell cmdlets are not available for users of Office 365 operated by 21Vianet in China or for users of Office 365 Germany. 
  
### Recommended requirements for best performance


|**Description**|**Recommendation**|
|:-----|:-----|
|CPU|64-bit Quad core processor or better|
|RAM|16 GB|
|Local Storage|Hard disk: 150 GB free space|
|Operating system|Windows Server 2016 Standard or Datacenter<br>Windows Server 2012 R2<br>Windows 10 client<br>.Net Framework 4.6.2|
|Microsoft Visual C++ 2015 Redistributable|Required for OneNote migration|



### Minimum requirements (expect slow performance)

|**Description**|**Minimum requirement**|
|:-----|:-----|
|CPU  <br/> |64-bit 1.4 GHz 2-core processor or better  <br/> |
|RAM  <br/> |8 GB  <br/> |
|Local Storage  <br/> |Hard disk: 150 GB free space  <br/> |
|Network card  <br/> |High-speed Internet connection  <br/> |
|Operating system  <br/> |Windows Server 2008 R2<br>Windows 7<br>Windows 8 or 8.1<br/> .NET Framework 4.6.2  <br/> |
|Microsoft Visual C++ 2015 Redistributable  <br/> |Required for OneNote migration.|  <br/> 
 
 
### Before you begin


- Provision your Office 365 with either your existing active directory or one of the other options for adding accounts to Office 365. See See [Office 365 integration with on-premises environments](http://go.microsoft.com/fwlink/?LinkID=616610&amp;clcid=0x409) and [Add users to Office 365 for business](http://go.microsoft.com/fwlink/?LinkID=616611&amp;clcid=0x409) for more information. 
    
<br><br>
  
### Create and initialize a migration session
<a name="Step1CreateInitialize"> </a>

- **[Register-SPMTMigration](https://docs.microsoft.com/en-us/powershell/module/spmt/Register-SPMigration.md)**<br> This cmdlet creates a SPMT migration session and initialization. The initialization includes configuring migration settings at the session level and connecting to SPO. If no specific setting parameters are defined, default settings will be used. 
After a session is registered, the Administrator can add a migration task to the SPMT session and start migration.

  
### Add a migration task
- **[Add-SPMTTask](https://docs.microsoft.com/en-us/powershell/module/spmt/Add-SPMTTask.md)**<br>
Use this cmdlet to add a new migration task to the registered migration session. Currently there are three different types of tasks allowed:  File share task, SharePoint task and a JSON defined task.  Note:  Duplicate tasks are not allowed.
  
 
  
### Remove a task
- **[Remove-SPMTTask](https://docs.microsoft.com/en-us/powershell/module/spmt/Remove-SPMTTask.md)**<br>
Use this cmdlet to remove an existing migration task from the registered migration.


  
### Start your migration
- **[Start-SPMTMigration](https://docs.microsoft.com/en-us/powershell/module/spmt/Start-SPMTTask.md)**<br>
This cmdlet will start the registered SPMT migration.
 
### Return object of current session
- **[Get-SPMTMigration](https://docs.microsoft.com/en-us/powershell/module/spmt/Get-SPMTMigration.md)**<br>
Return object of current session. It includes information regarding to current tasks and current settings. 

### Stop your current migration session
- **[Stop-SPMTMigration](https://docs.microsoft.com/en-us/powershell/module/spmt/Stop-SPMTMigration.md)**<br>
This cmdlet will cancel the current migration session. 


### Show your migration status details
- **[Show-SPMTMigration](https://docs.microsoft.com/en-us/powershell/module/spmt/Show-SPMTMigration.md)**<br>
If you start the migration in *NoShow* mode, running the **Show-SPMTMigration** cmdlet will display the task ID, data source location, target location and migration status in the console. Pressing Ctrl+C will return to *NoShow* mode.  

### Remove the migration session
- **[Unregister-SPMTMigration](https://docs.microsoft.com/en-us/powershell/module/spmt/Unregister-SPMTMigration.md)**<br>
Use this cmdlet to delete the migration session. 

## Sample Scenarios

Example 1: IT admin adds a SharePoint on-prem task and starts migration in the background.<br>

```powershell
$Global:UserID = “abcdefgh123456789E180XXXX”
$Global:SPOUrl = “https://$Global:UserID.sharepoint.com”
$Global:UserName = “admin@$Global:UserID.onmicrosoft.com”
$Global:PassWord = ConvertTo-SecureString -String “Cl@!ms!2” -AsPlainText -Force
$Global:AssemblyDirectory = $pwd.Path
$Global:SPOCredential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $Global:UserName, $Global:YourPassword
$Global:SourceSiteUrl = "http://Yoursite"
$Global:OnPremUserName = "Yourcomputer\administrator"
$Global:OnPremPassword = ConvertTo-SecureString -String "YourPassword" -AsPlainText -Force 
$Global:SPCredential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $Global:OnPremUserName, $Global:OnPremPassword
$Global:SourceListName = "Doc1"
$Global:TargetListName = "TestDoc"


Import-Module $Global:AssemblyDirectory\SPMTPowershell.dll
$StartTime = [DateTime]::UtcNow
Register-SPMTMigration -Credentials $Global:SPOCredential
Add-SPMTTask -SharePointSourceCredential $Global:SPCredential -SharePointSourceSiteUrl $Global:SourceSiteUrl -SourceList $Global:SourceListName -TargetSiteUrl $Global:SPOUrl -TargetList $Global:TargetListName
Start-SPMTMigration -NoShow
```
Example 2: IT admin wants to bring the migration from the background “NoShow mode” to the foreground, run below the cmdlet, so he can see the migration progress in the console.<br>
```powershell
Show-SPMTMigration 
```
