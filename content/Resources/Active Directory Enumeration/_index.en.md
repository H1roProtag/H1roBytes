+++
tags = ["Active Directory", "Windows", "Enumeration", "PowerView"]
title = "Active Directory Enumeration"
weight = 10
draft = false
images = [ "/Resources/Active Directory Enumeration/logo.png" ]
description = "Active Directory Enumeration."
+++

![Logo](logo.png)

Date written: February 2024     
Date published: April 2024

# Disclaimer 

Please see the disclaimer [here](https://hirobytes.xyz/disclaimer/). 

## Tools

> [!Note] Hacktricks also has a page about this
>  https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters/powerview
>  
>  And S1ckB0y1337 PayloadAllTheThings-like
>  https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet?tab=readme-ov-file#using-powerview

- ActiveDirectory PowerShell module
	- https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps
	- https://github.com/samratashok/ADModule
	
```powershell
# Import ADModule
Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

- Bloodhound
	- https://github.com/BloodHoundAD/BloodHound
- PowerView
	- https://github.com/ZeroDayLab/PowerSploit/blob/master/Recon/PowerView.ps1
- SharpView - Doesn't support filtering using Pipeline
	- https://github.com/tevora-threat/SharpView/	  

## Domain Enumeration

### PowerView 

#### Domains and DCs

```powershell
# Get current or specified domain (General enumeration)
Get-Domain 
Get-Domain -Domain <DomainName> 

# Get domain SID for the current domain
Get-DomainSID 

# Get domain policy for the current domain
Get-DomainPolicyData 

# Get domain controllers for the current or specified domain
Get-DomainController 
Get-DomainController -Domain <DomainName>
```

#### Users

```powershell 
# Get a list of users in the current domain
Get-DomainUser 

# Search for a specific user in the current domain and display all or specified properties. 
Get-DomainUser -Identity <UserName> 
Get-DomainUser -Identity <UserName> -Properties * # all properties
Get-DomainUser -Properties samaccountname,logonCount # specified properties

# Lists only the "samaccountname" property for domain users. Could replace with any User field.
Get-DomainUser | select -ExpandProperty samaccountname 

# Search for a string in a user's attributes.
Get-DomainUser -LDAPFilter "Description=*built*" |  Select name,Description 
```

#### Logon and Sessions  

```powershell 
# Get actively logged users on a computer (needs local admin rights on  the target)
Get-NetLoggedon -ComputerName <ComputerName>

# Get locally logged users on a computer (needs remote registry on the target - started by-default on server OS)
Get-LoggedonLocal -ComputerName <ComputerName> 

# Get the last logged user on a computer (needs administrative rights and  remote registry on the target)
Get-LastLoggedOn -ComputerName <ComputerName> 

 # Lists the current network session properties
Get-NetSession 

# Lists current users that are logged onto the domain
Get-NetLoggedon 
```

#### Computers

```powershell
# Get a list of computers in the current domain. Can only show dnshostname/specific fields. 
Get-DomainComputer 
Get-DomainComputer | select dnshostname 

# Lists all computers in the current domain with the operating system containing Server 2022
Get-DomainComputer -OperatingSystem "*Server 2022*" 

# Gets all current domain computers and also checks to see if the host is up. 
Get-DomainComputer -Ping # Gets all current domain computers and also checks to see if the host is up. 
```

#### Shared Files and Folder

```powershell
# Find shares on hosts in current domain.
Invoke-ShareFinder -Verbose 

# Find sensitive files on computers in the domain
Invoke-FileFinder -Verbose 

# Get all fileservers of the domain
Get-NetFileServer 
```
#### Groups

```powershell
# Get all the groups in the current or specified domain
Get-DomainGroup 
Get-DomainGroup -Domain <targetdomain>
Get-DomainGroup | select Name # Lists all groups in the current domain, only showing Name. 

# Get all groups containing the words like "admin" in group name. Use quotes instead of * for more refined results.
Get-DomainGroup *admin* 

# Get all the members of the specified group on the current or specified domain
Get-DomainGroupMember -Identity "<GroupName>" -Recurse 
Get-DomainGroupMember -Identity "<GroupName>" -Domain <DomainName> 

# Get the group membership for specific user
Get-DomainGroup -UserName "<UserName>" 

# Lists all local groups on the machine with in the current or specified domain. Requireds admin on non-DC mahcines.
Get-NetLocalGroup -ComputerName <ComputerName> 
Get-NetLocalGroupMember -ComputerName <ComputerName> -GroupName <GroupName> 
```

#### GPO

```powershell
# Get list of GPO in current domain. Can also specify a single computer. 
Get-DomainGPO 
Get-DomainGPO -ComputerIdentity <ComputerName> 

# Get GPO(s) that use Restricted Groups or groups.xml for interesting users
Get-DomainGPOLocalGroup 

# Get users that are in a local group of a machine using GPO
Get-DomainGPOComputerLocalGroupMapping -ComputerIdentity <ComputerName>

# Get machines where the given user is member of a specific group
Get-DomainGPOUserLocalGroupMapping -Identity <UserName> - Verbose 
```

#### OU

```powershell
# List all OUs in current Domain. Can filter and only show name
Get-DomainOU 
Get-DomainOU | select -ExpandProperty name 

# List all computers in the OU.
(Get-DomainOU -Identity <OUName>).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name 

 # Shows the gplink for the StudentMachines OU. Used to enumerate GPO of that OU. 
(Get-DomainOU -Identity <OUName>).gplink

 # Enumerates the GPOs on the StudentMachines OU.
Get-DomainGPO -Identity '{7478F170-6A0C-490C-B355-9E4618BC785D}'

# The above two commands into 1
Get-DomainGPO -Identity (Get-DomainOU -Identity StudentMachines).gplink.substring(11,(Get-DomainOU -Identity StudentMachines).gplink.length-72)
```

`(Get-DomainOU -Identity StudentMachines).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name`

|Command Component|Explanation|
|---|---|
|`Get-DomainOU`|Retrieves information about Organizational Units (OUs) within a domain. |
|`-Identity StudentMachines`|This specifies the OU named "StudentMachines" as the target. `Identity` is a parameter used to specify the object (in this case, OU) to retrieve.|
|`.distinguishedname`|This accesses the `distinguishedname` property of the OU returned by `Get-DomainOU`. This property contains the unique identifier for the OU.|
| `%{}` |For Each Object. So this will cycle through each domain computer.  |
|`Get-DomainComputer`|Retrieves information about computers within a specified domain or OU. |
|`-SearchBase $_`|This parameter specifies the search base for the `Get-DomainComputer` command. `$_` refers to the current object in the pipeline, which is an OU's DN.|
|`select name` |Outputs only the name.  |

#### Access Control Lists

```powershell
# Get the ACLs associated with the specified object
Get-DomainObjectAcl -SamAccountName <AccountName> -ResolveGUIDs 

# Get the ACLs associated with the specified prefix to be used for search
Get-DomainObjectAcl -SearchBase "LDAP://CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local" -ResolveGUIDs - Verbose 

# Search for interesting ACEs
Find-InterestingDomainAcl -ResolveGUIDs 

# check for modify rights/permissions for a user
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "<UserName>"}

# check for modify rights/permissions for a group
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "<GroupName>"}

# Get the ACLs associated with the specified path
Get-PathAcl -Path "\\Path\Of\Share" 

```

#### Trusts

```powershell
# Get a list of all domain trusts for the current or specified domain
Get-DomainTrust 
Get-DomainTrust -Domain <DomainName> 

# Lists only the external trusts in the current **forest**. 
Get-ForestDomain | %{Get-DomainTrust -Domain $_.Name} | ?{$_.TrustAttributes -eq "FILTER_SIDS"}

# Lists external trusts of the current **domain**
Get-DomainTrust | ?{$_.TrustAttributes -eq "FILTER_SIDS"}

# Gets all domains in specified forest and then for each domain it gets trust info??
Get-ForestDomain -Forest <ForestName> | %{Get-DomainTrust -Domain $_.Name}
```

#### Forests

``` powershell
# Get details about the current or specified forest
Get-Forest 
Get-Forest -Forest <ForestName> 

# Get all domains in the current or specified forest
Get-ForestDomain 
Get-ForestDomain -Forest <ForestName>

# Get all global catalogs for the current or specified forest
Get-ForestGlobalCatalog 
Get-ForestGlobalCatalog -Forest <ForestName> 

# Map trusts of the current or specified forest (no Forest trusts in the lab)
Get-ForestTrust 
Get-ForestTrust -Forest <ForestName> 
```

#### User Hunting

```powershell
# Find all machines on the current domain where the current user has local admin access
Find-LocalAdminAccess -Verbose 

# You need to give the list of computers in the domain. Need admin privs. Useful if RCP and SMB ports are closed.
.\Find-WMILocalAdminAccess.ps1 -ComputerFile .\computers.txt

# Find computers where a domain admin (or specified user/group) has sessions
Find-DomainUserLocation -Verbose
Find-DomainUserLocation -UserGroupIdentity "RDPUsers"

# Find computers where a domain admin session is available and current user  has admin access (uses Test-AdminAccess).
Find-DomainUserLocation -CheckAccess

# Find computers (File Servers and Distributed File servers) where a domain  admin session is available.
Find-DomainUserLocation -Stealth

# List sessions on remote machines (https://github.com/Leo4j/Invoke-SessionHunter)
Invoke-SessionHunter -FailSafe
Invoke-SessionHunter -NoPortScan -Targets  C:\AD\Tools\servers.txt
```

### AD Module

#### Domains and DCs

```powershell
# Get current or specified domain (General enumeration)
Get-AADomain 
Get-AADDomain -Identity <DomainName> 

# Get domain SID for the current domain
(Get-AADomain).DomainSID 

# Get domain policy for the current domain
(Get-DomainPolicyData).systemaccess

# Get domain policy for another domain  
(Get-DomainPolicyData -domain  <DomainName>).systemaccess # AAD Module

# Get domain controllers for the current or specified domain
Get-ADDomainController
Get-ADDomainController -DomainName <DomainName> - Discover 

```

#### Users

```powershell 
# Search for a specific user with all properties shown
Get-ADUser -Filter * -Properties *
Get-ADUser -Identity <UserName> -Properties *


# User Properties 
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -  MemberType *Property | select Name
Get-ADUser -Filter * -Properties * | select  name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}}

# Search for a  string in a user's attributes:
Get-ADUser -Filter 'Description -like "*built*"' -  Properties Description | select name,Description
```

#### Computers

```powershell
# Get a list of computers in the current domain
Get-ADComputer -Filter * | select Name
Get-ADComputer -Filter * - Properties *
Get-ADComputer -Filter 'OperatingSystems -like "*Server 2022*"' - Properties OperatingSystem | select Name, OperatingSystem
Get-ADComputer -Filter * -Properties DNSHostName | %{Test-Connection -Count 1 -ComputerName $_.DNSHostName}

```

#### Groups

```powershell
# List groups in the current domain
Get-ADGroup -Filter * | select Name  
Get-ADGroup -Filter * -Properties *

# Get all groups containing the word "admin" in group name. To only filter results with just "admin" use quotes
Get-ADGroup -Filter 'Name -like "*admin*"' | select Name 


# Get all the members of the specified group
Get-ADGroupMember -Identity "<GroupName>" -Recursive

# Get the group membership for a user
Get-ADPrincipalGroupMembership -Identity <UserName>
```

#### OU

```powershell
# General OU
Get-ADOrganizationalUnit -Filter * -Properties *
```

#### Access Control Lists

```powershell
# Enumerate ACLs using ActiveDirectory module but without resolving GUIDs
(Get-Acl'AD:\CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local').Access
```

#### Trusts

```powershell
# Get a list of all domain trusts for the current or specigied domain
Get-ADTrust
Get-ADTrust -Identity <DomainName>

# Maps all the tusts in the current domain. -Server Optional
Get-ADTrust -Filter * -Server <ServerName>

# Lists all trusts in the current forest
Get-ADForest | %{Get-ADTrust -Filter *} 

# Lists only the external trusts in current domain?
(Get-ADForest).Domains | %{Get-ADTrust -Filter '(intraForest -ne $True) -and (ForestTransitive -ne $True)' -Server $_} 

# Lists external trusts in all domains?
Get-ADTrust -Filter '(intraForest -ne $True) -and(ForestTransitive -ne $True)' 
```

#### Forests

``` powershell
# Get details about the current or specified forest
Get-ADForest
Get-ADForest -Identity <ForestName>

# Get all domains in the current forest
(Get-ADForest).Domains

# Get all global catalogs for the current forest
Get-ADForest | select -ExpandProperty GlobalCatalogs

# Map trusts of a forest (no Forest trusts in the lab)
Get-ADTrust -Filter 'msDS-TrustForestTrustInfo -ne "$null"'

```


### BloodHound

> [!Note] CRTP Note
> BloodHound Legacy is present in the C:\AD\Tools directory of your student VM. 
> 
> You can have Read-only access to to the prep-populated BloodHound CE -  [https://crtpbloodhound-altsecdashboard.msappproxy.net/](https://crtpbloodhound-altsecdashboard.msappproxy.net/)
> 
> Use the credentials for crtpreader@altsecdashboard.onmicrosoft.com from the lab portal - https://adlab.enterprisesecurity.io/
>
>  Just need to run the following to access BloodHound during exam. It is already set up
> 
>  ``` powershell
> . C:\AD\Tools\BloodHound-master\Collectors\SharpHound.ps1
>
>  Invoke-BloodHound -CollectionMethod All
>  ```


```powershell
# Supply data to BloodHound (Remember to bypass .NET AMSI):
. C:\AD\Tools\BloodHound-master\Collectors\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All
**or**
SharpHound.exe

# Removes noisy collction methods like RDP, DCOM, PSRemote and  LocalAdmin
Invoke-BloodHound -Steatlh
SharpHound.exe --steatlh

# Avoid detections like MDI (Microsoft Defender for Identity?)
Invoke-BloodHound -ExcludeDCs
```
