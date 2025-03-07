---
title: Active Directory Security Groups
description: Windows Server Active Directory security groups, scope, and functions
author: dansimp
ms.author: dansimp
ms.topic: article
ms.date: 08/02/2022
---

# Active Directory security groups

>Applies to: Windows Server 2022, Windows Server 2019, Windows Server 2016

This article describes the Default Active Directory security groups.

There are two forms of common security principals in Active Directory: user accounts and computer accounts. These accounts represent a physical entity (a person or a computer). User accounts can also be used as dedicated service accounts for some applications. Security groups are used to collect user accounts, computer accounts, and other groups into manageable units.

In the Windows Server operating system, there are several built-in accounts and security groups that are preconfigured with the appropriate rights and permissions to perform specific tasks. For Active Directory, there are two types of administrative responsibilities:

- **Service administrators**: Responsible for maintaining and delivering Active Directory Domain Services (AD DS), including managing domain controllers and configuring the AD DS.

- **Data administrators**: Responsible for maintaining the data that is stored in AD DS and on domain member servers and workstations.

## About Active Directory groups

Groups are used to collect user accounts, computer accounts, and other groups into manageable units. Working with groups instead of with individual users helps simplify network maintenance and administration.

There are two types of groups in Active Directory:

- **Distribution groups**: Used to create email distribution lists.

- **Security groups**: Used to assign permissions to shared resources.

### Distribution groups

Distribution groups can be used only with email applications (such as Exchange Server) to send email to collections of users. Distribution groups are not security enabled, which means that they cannot be listed in discretionary access control lists (DACLs).

### Security groups

Security groups can provide an efficient way to assign access to resources on your network. By using security groups, you can:

- Assign user rights to security groups in Active Directory.

    User rights are assigned to a security group to determine what members of that group can do within the scope of a domain or forest. User rights are automatically assigned to some security groups when Active Directory is installed to help administrators define a person’s administrative role in the domain.

    For example, a user who is added to the Backup Operators group in Active Directory has the ability to back up and restore files and directories that are located on each domain controller in the domain. This is possible because, by default, the user rights **Backup files and directories** and **Restore files and directories** are automatically assigned to the Backup Operators group. Therefore, members of this group inherit the user rights that are assigned to that group.

    You can use Group Policy to assign user rights to security groups to delegate specific tasks. For more information about using Group Policy, see [User Rights Assignment](/windows/device-security/security-policy-settings/user-rights-assignment).

- Assign permissions to security groups for resources.

    Permissions are different than user rights. Permissions are assigned to the security group for the shared resource. Permissions determine who can access the resource and the level of access, such as Full Control. Some permissions that are set on domain objects are automatically assigned to allow various levels of access to default security groups, such as the Account Operators group or the Domain Admins group.

    Security groups are listed in DACLs that define permissions on resources and objects. When assigning permissions for resources (file shares, printers, and so on), administrators should assign those permissions to a security group rather than to individual users. The permissions are assigned once to the group, instead of several times to each individual user. Each account that is added to a group receives the rights that are assigned to that group in Active Directory along with the user receiving permissions that are defined for that group.

Like distribution groups, security groups can be used as an email entity. Sending an email message to the group sends the message to all the members of the group.

### Group scope

Groups are characterized by a scope that identifies the extent to which the group is applied in the domain tree or forest. The scope of the group defines where the group can be granted permissions. The following three group scopes are defined by Active Directory:

- Universal

- Global

- Domain Local

> [!NOTE]
> In addition to these three scopes, the default groups in the **Builtin** container have a group scope of Builtin Local. This group scope and group type cannot be changed.

The following table lists the three group scopes and more information about each scope for a security group.

|Scope|Possible Members|Scope Conversion|Can Grant Permissions|Possible Member of|
|--- |--- |--- |--- |--- |
|Universal|Accounts from any domain in the same forest<p>Global groups from any domain in the same forest<p>Other Universal groups from any domain in the same forest|Can be converted to<p>Domain Local scope if the group is not a member of any other Universal groups<p>Can be converted to Global scope if the group does not contain any other Universal groups|On any domain in the same forest or trusting forests|Other Universal groups in the same forest<p>Domain<p>Local groups in the same forest or trusting forests<p>Local groups on computers in the same forest or trusting forests|
|Global|Accounts from the same domain<p>Other Global groups from the same domain|Can be converted to Universal scope if the group is not a member of any other global group|On any domain in the same forest, or trusting domains or forests|Universal groups from any domain in the same forest<p>Other Global groups from the same domain<p>Domain Local groups from any domain in the same forest, or from any trusting domain|
|Domain Local|Accounts from any domain or any trusted domain<p>Global groups from any domain or any trusted domain<p>Universal groups from any domain in the same forest<p>Other Domain Local groups from the same domain<p>Accounts, Global groups, and Universal groups from other forests and from external domains|Can be converted to Universal scope if the group does not contain any other Domain Local groups|Within the same domain|Other Domain Local groups from the same domain<p>Local groups on computers in the same domain, excluding built-in groups that have well-known SIDs|

### Special identity groups

Special identities are referred to as groups. Special identity groups do not have specific memberships that can be modified, but they can represent different users at different times, depending on the circumstances. Some of these groups include Creator Owner, Batch, and Authenticated User.

For information about the special identity groups, see [Understand Special Identities](understand-special-identities-groups.md).

## Default security groups

Default groups, such as the Domain Admins group, are security groups that are created automatically when you create an Active Directory domain. You can use these predefined groups to help control access to shared resources and to delegate specific domain-wide administrative roles.

Many default groups are automatically assigned a set of user rights that authorize members of the group to perform specific actions in a domain, such as logging on to a local system or backing up files and folders. For example, a member of the Backup Operators group has the right to perform backup operations for all domain controllers in the domain.

When you add a user to a group, the user receives all the user rights that are assigned to the group including all the permissions that are assigned to the group for any shared resources.

Default groups are located in the **Builtin** container and in the **Users** container in Active Directory Users and Computers. The **Builtin** container includes groups that are defined with the Domain Local scope. The **Users** container includes groups that are defined with Global scope and groups that are defined with Domain Local scope. You can move groups that are located in these containers to other groups or organizational units (OU) within the domain, but you cannot move them to other domains.

Some of the administrative groups that are listed in this article and all members of these groups are protected by a background process that periodically checks for and applies a specific security descriptor. This descriptor is a data structure that contains security information associated with a protected object. This process ensures that any successful unauthorized attempt to modify the security descriptor on one of the administrative accounts or groups will be overwritten with the protected settings.

The security descriptor is present on the **AdminSDHolder** object. This means that if you want to modify the permissions on one of the service administrator groups or on any of its member accounts, you must modify the security descriptor on the **AdminSDHolder** object so that it will be applied consistently. Be careful when you make these modifications because you are also changing the default settings that will be applied to all of your protected administrative accounts.

### Default Active Directory security groups

The following list provides descriptions of the default groups that are located in the **Builtin** and **Users** containers in the Windows Server operating system:

- [Access Control Assistance Operators](#access-control-assistance-operators)
- [Account Operators](#account-operators)
- [Administrators](#administrators)
- [Allowed RODC Password Replication group](#allowed-rodc-password-replication-group)
- [Backup Operators](#backup-operators)
- [Certificate Service DCOM Access](#certificate-service-dcom-access)
- [Cert Publishers](#cert-publishers)
- [Cloneable Domain Controllers](#cloneable-domain-controllers)
- [Cryptographic Operators](#cryptographic-operators)
- [Denied RODC Password Replication group](#denied-rodc-password-replication-group)
- [Device Owners](#device-owners)
- [DHCP Administrators](#dhcp-administrators)
- [DHCP Users](#dhcp-users)
- [Distributed COM Users](#distributed-com-users)
- [DnsUpdateProxy](#dnsupdateproxy)
- [DnsAdmins](#dnsadmins)
- [Domain Admins](#domain-admins)
- [Domain Computers](#domain-computers)
- [Domain Controllers](#domain-controllers)
- [Domain Guests](#domain-guests)
- [Domain Users](#domain-users)
- [Enterprise Admins](#enterprise-admins)
- [Enterprise Key Admins](#enterprise-key-admins)
- [Enterprise Read-only Domain Controllers](#enterprise-read-only-domain-controllers)
- [Event Log Readers](#event-log-readers)
- [Group Policy Creator Owners](#group-policy-creator-owners)
- [Guests](#guests)
- [Hyper-V Administrators](#hyper-v-administrators)
- [IIS_IUSRS](#iis_iusrs)
- [Incoming Forest Trust Builders](#incoming-forest-trust-builders)
- [Key Admins](#key-admins)
- [Network Configuration Operators](#network-configuration-operators)
- [Performance Log Users](#performance-log-users)
- [Performance Monitor Users](#performance-monitor-users)
- [Pre–Windows 2000 Compatible Access](#prewindows-2000-compatible-access)
- [Print Operators](#print-operators)
- [Protected Users](#protected-users)
- [RAS and IAS Servers](#ras-and-ias-servers)
- [RDS Endpoint Servers](#rds-endpoint-servers)
- [RDS Management Servers](#rds-management-servers)
- [RDS Remote Access Servers](#rds-remote-access-servers)
- [Read-only Domain Controllers](#read-only-domain-controllers)
- [Remote Desktop Users](#remote-desktop-users)
- [Remote Management Users](#remote-management-users)
- [Replicator](#replicator)
- [Schema Admins](#schema-admins)
- [Server Operators](#server-operators)
- [Storage Replica Administrators](#storage-replica-administrators)
- [System Managed Accounts group](#system-managed-accounts-group)
- [Terminal Server License Servers](#terminal-server-license-servers)
- [Users](#users)
- [Windows Authorization Access group](#windows-authorization-access-group)
- [WinRMRemoteWMIUsers_](#winrmremotewmiusers_)

### Access Control Assistance Operators

Members of this group can remotely query authorization attributes and permissions for resources on the computer.

The Access Control Assistance Operators group applies to the Windows Server operating system listed in the Default Active Directory security groups table.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-579|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Account Operators

The Account Operators group grants limited account creation privileges to a user. Members of this group can create and modify most types of accounts, including those of users, local groups, global groups, and members can log in locally to domain controllers.

Members of the Account Operators group cannot manage the Administrator user account, the user accounts of administrators, or the [Administrators](#administrators), [Server Operators](#server-operators), [Account Operators](#account-operators), [Backup Operators](#backup-operators), or [Print Operators](#print-operators) groups. Members of this group cannot modify user rights.

The Account Operators group applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list.

> [!NOTE]
> By default, this built-in group has no members, and it can create and manage users and groups in the domain, including its own membership and that of the Server Operators group. This group is considered a service administrator group because it can modify Server Operators, which in turn can modify domain controller settings. As a best practice, leave the membership of this group empty, and do not use it for any delegated administration. This group cannot be renamed, deleted, or moved.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-548|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally:](/windows/device-security/security-policy-settings/allow-log-on-locally) SeInteractiveLogonRight|

### Administrators

Members of the Administrators group have complete and unrestricted access to the computer, or if the computer is promoted to a domain controller, and members have unrestricted access to the domain.

The Administrators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list.

> [!NOTE]
> The Administrators group has built-in capabilities that give its members full control over the system. This group cannot be renamed, deleted, or moved. This built-in group controls access to all the domain controllers in its domain, and it can change the membership of all administrative groups. Membership can be modified by members of the following groups: the default service Administrators, Domain Admins in the domain, or Enterprise Admins. This group has the special privilege to take ownership of any object in the directory or any resource on a domain controller. This account is considered a service administrator group because its members have full access to the domain controllers in the domain.

This security group includes the following changes since Windows Server 2008:

- Default user rights changes: **Allow log on through Terminal Services** existed in Windows Server 2008, and it was replaced by [Allow log on through Remote Desktop Services](/windows/device-security/security-policy-settings/allow-log-on-through-remote-desktop-services).

- [Remove computer from docking station](/windows/device-security/security-policy-settings/remove-computer-from-docking-station) was removed in Windows Server 2012 R2.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-544|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|Administrator, Domain Admins, Enterprise Admins|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Adjust memory quotas for a process](/windows/device-security/security-policy-settings/adjust-memory-quotas-for-a-process): SeIncreaseQuotaPrivilege<p>[Access this computer from the network](/windows/device-security/security-policy-settings/access-this-computer-from-the-network): SeNetworkLogonRight<p>[Allow log on locally](/windows/device-security/security-policy-settings/allow-log-on-locally): SeInteractiveLogonRight<p>[Allow log on through Remote Desktop Services](/windows/device-security/security-policy-settings/allow-log-on-through-remote-desktop-services): SeRemoteInteractiveLogonRight<p>[Back up files and directories](/windows/device-security/security-policy-settings/back-up-files-and-directories): SeBackupPrivilege<p>[Bypass traverse checking](/windows/device-security/security-policy-settings/bypass-traverse-checking): SeChangeNotifyPrivilege<p>[Change the system time](/windows/device-security/security-policy-settings/change-the-system-time): SeSystemTimePrivilege<p>[Change the time zone](/windows/device-security/security-policy-settings/change-the-time-zone): SeTimeZonePrivilege<p>[Create a pagefile](/windows/device-security/security-policy-settings/create-a-pagefile): SeCreatePagefilePrivilege<p>[Create global objects](/windows/device-security/security-policy-settings/create-global-objects): SeCreateGlobalPrivilege<p>[Create symbolic links](/windows/device-security/security-policy-settings/create-symbolic-links): SeCreateSymbolicLinkPrivilege<p>[Debug programs](/windows/device-security/security-policy-settings/debug-programs): SeDebugPrivilege<p>[Enable computer and user accounts to be trusted for delegation](/windows/device-security/security-policy-settings/enable-computer-and-user-accounts-to-be-trusted-for-delegation): SeEnableDelegationPrivilege<p>[Force shutdown from a remote system](/windows/device-security/security-policy-settings/force-shutdown-from-a-remote-system): SeRemoteShutdownPrivilege<p>[Impersonate a client after authentication](/windows/device-security/security-policy-settings/impersonate-a-client-after-authentication): SeImpersonatePrivilege<p>[Increase scheduling priority](/windows/device-security/security-policy-settings/increase-scheduling-priority): SeIncreaseBasePriorityPrivilege<p>[Load and unload device drivers](/windows/device-security/security-policy-settings/load-and-unload-device-drivers): SeLoadDriverPrivilege<p>[Log on as a batch job](/windows/device-security/security-policy-settings/log-on-as-a-batch-job): SeBatchLogonRight<p>[Manage auditing and security log](/windows/device-security/security-policy-settings/manage-auditing-and-security-log): SeSecurityPrivilege<p>[Modify firmware environment values](/windows/device-security/security-policy-settings/modify-firmware-environment-values): SeSystemEnvironmentPrivilege<p>[Perform volume maintenance tasks](/windows/device-security/security-policy-settings/perform-volume-maintenance-tasks): SeManageVolumePrivilege<p>[Profile system performance](/windows/device-security/security-policy-settings/profile-system-performance): SeSystemProfilePrivilege<p>[Profile single process](/windows/device-security/security-policy-settings/profile-single-process): SeProfileSingleProcessPrivilege<p>[Remove computer from docking station](/windows/device-security/security-policy-settings/remove-computer-from-docking-station): SeUndockPrivilege<p>[Restore files and directories](/windows/device-security/security-policy-settings/restore-files-and-directories): SeRestorePrivilege<p>[Shut down the system](/windows/device-security/security-policy-settings/shut-down-the-system): SeShutdownPrivilege<p>[Take ownership of files or other objects](/windows/device-security/security-policy-settings/take-ownership-of-files-or-other-objects): SeTakeOwnershipPrivilege|

### Allowed RODC Password Replication group

The purpose of this security group is to manage a RODC password replication policy. This group has no members by default, and it results in the condition that new Read-only domain controllers do not cache user credentials. The [Denied RODC Password Replication](#denied-rodc-password-replication-group) group contains various high-privilege accounts and security groups. The Denied RODC Password Replication group supersedes the Allowed RODC Password Replication group.

The Allowed RODC Password Replication applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-571|
|Type|Domain local|
|Default container|CN=Users DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Backup Operators

Members of the Backup Operators group can back up and restore all files on a computer, regardless of the permissions that protect those files. Backup Operators also can log on to and shut down the computer. This group cannot be renamed, deleted, or moved. By default, this built-in group has no members, and it can perform backup and restore operations on domain controllers. Its membership can be modified by the following groups: default service Administrators, Domain Admins in the domain, or Enterprise Admins. It cannot modify the membership of any administrative groups. While members of this group cannot change server settings or modify the configuration of the directory, they do have the permissions needed to replace files (including operating system files) on domain controllers. Because of this, members of this group are considered service administrators.

The Backup Operators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-551|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](/windows/device-security/security-policy-settings/allow-log-on-locally): SeInteractiveLogonRight<p>[Back up files and directories](/windows/device-security/security-policy-settings/back-up-files-and-directories): SeBackupPrivilege<p>[Log on as a batch job](/windows/device-security/security-policy-settings/log-on-as-a-batch-job): SeBatchLogonRight<p>[Restore files and directories](/windows/device-security/security-policy-settings/restore-files-and-directories): SeRestorePrivilege<p>[Shut down the system](/windows/device-security/security-policy-settings/shut-down-the-system): SeShutdownPrivilege|

### Certificate Service DCOM Access

Members of this group are allowed to connect to certification authorities in the enterprise.

The Certificate Service DCOM Access applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-\<domain>-574|
|Type|Domain Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Cert Publishers

Members of the Cert Publishers group are authorized to publish certificates for User objects in Active Directory.

The Cert Publishers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-517|
|Type|Domain Local|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### Cloneable Domain Controllers

Members of the Cloneable Domain Controllers group that are domain controllers may be cloned. In Windows Server 2012 R2 and Windows Server 2012, you can deploy domain controllers by copying an existing virtual domain controller. In a virtual environment, you no longer have to repeatedly deploy a server image that is prepared by using sysprep.exe, promoting the server to a domain controller, and then complete additional configuration requirements for deploying each domain controller (including adding the virtual domain controller to this security group).

For more information, see [Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-522|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Cryptographic Operators

Members of this group are authorized to perform cryptographic operations. This security group was added in Windows Vista Service Pack 1 (SP1) to configure Windows Firewall for IPsec in Common Criteria mode.

The Cryptographic Operators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

This security group was introduced in Windows Vista Service Pack 1, and it has not changed in subsequent versions.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-569|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Denied RODC Password Replication group

Members of the Denied RODC Password Replication group cannot have their passwords replicated to any Read-only domain controller.

The purpose of this security group is to manage a RODC password replication policy. This group contains various high-privilege accounts and security groups. The Denied RODC Password Replication group supersedes the [Allowed RODC Password Replication group](#allowed-rodc-password-replication-group).

This security group includes the following changes since Windows Server 2008:

- Windows Server 2012 changed the default members to include [Cert Publishers](#cert-publishers).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-572|
|Type|Domain local|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|[Cert Publishers](#cert-publishers)<p>[Domain Admins](#domain-admins)<p>[Domain Controllers](#domain-controllers)<p>[Enterprise Admins](#enterprise-admins)<p>[Group Policy Creator Owners](#group-policy-creator-owners)<p>[Read-only Domain Controllers](#read-only-domain-controllers)<p>[Schema Admins](#schema-admins)|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?||
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Device Owners

Microsoft does not recommend changing the default configuration where this security group has zero members. Changing the default configuration could hinder future scenarios that rely on this group. This group is not currently used in Windows.

The Device Owners applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-583|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Can be moved out but it is not recommended|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](/windows/device-security/security-policy-settings/allow-log-on-locally): SeInteractiveLogonRight<p>[Access this computer from the network](/windows/device-security/security-policy-settings/access-this-computer-from-the-network): SeNetworkLogonRight<p>[Bypass traverse checking](/windows/device-security/security-policy-settings/bypass-traverse-checking): SeChangeNotifyPrivilege<p>[Change the time zone](/windows/device-security/security-policy-settings/change-the-time-zone): SeTimeZonePrivilege|

### DHCP Administrators

Members of the DHCP Administrators group have the ability to create, delete, and manage different areas of the servers scope including the rights to backup and restore the DHCP database. Even though this group has administrative rights, it is not a part of the Administrators group as this role is limited to DHCP services.

The DHCP Administrators group applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|[Users](#users)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Can be moved out but it is not recommended|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### DHCP Users

Members of the DHCP Users group have the ability to see which scopes are active, inactive, IP addresses assigned, and connectivity issues if the DHCP server is not configured correctly. This group is limited to read-only access to the DHCP server.

The DHCP Users group applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|[Users](#users)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Can be moved out but it is not recommended|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### Distributed COM Users

Members of the Distributed COM Users group are allowed to launch, activate, and use Distributed COM objects on the computer. Microsoft Component Object Model (COM) is a platform-independent, distributed, object-oriented system for creating binary software components that can interact. Distributed Component Object Model (DCOM) allows applications to be distributed across locations that make the most sense to you and to the application. This group appears as a SID (security identifier) until the domain controller is made. The primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Distributed COM Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-562|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### DnsUpdateProxy

Members of the DnsUpdateProxy group are DNS clients. They are permitted to perform dynamic updates on behalf of other clients (such as DHCP servers). A DNS server can develop stale resource records when a DHCP server is configured to dynamically register host (A) and pointer (PTR) resource records on behalf of DHCP clients by using dynamic update. Adding clients to this security group mitigates this scenario.

However, to protect against unsecured records or to permit members of the DnsUpdateProxy group to register records in zones that allow only secured dynamic updates, you must create a dedicated user account and configure DHCP servers to perform DNS dynamic updates by using the credentials of this account (user name, password, and domain). Multiple DHCP servers can use the credentials of one dedicated user account. This group exists only if the DNS server role is or was once installed on a domain controller in the domain.

For information, see [DNS Record Ownership and the DnsUpdateProxy Group](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd334715(v=ws.10)).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-\<variable RI>|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### DnsAdmins

Members of DnsAdmins group have access to network DNS information. The default permissions are as follows: Allow: Read, Write, Create All Child objects, Delete Child objects, Special Permissions. This group exists only if the DNS server role is or was once installed on a domain controller in the domain.

For more information about security and DNS, see [DNSSEC in Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn593694(v=ws.11)).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-\<variable RI>|
|Type|Builtin Local|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Domain Admins

Members of the Domain Admins security group are authorized to administer the domain. By default, the Domain Admins group is a member of the Administrators group on all computers that have joined a domain, including the domain controllers. The Domain Admins group is the default owner of any object that is created in Active Directory for the domain by any member of the group. If members of the group create other objects, such as files, the default owner is the Administrators group.

The Domain Admins group controls access to all domain controllers in a domain, and it can modify the membership of all administrative accounts in the domain. Membership can be modified by members of the service administrator groups in its domain (Administrators and Domain Admins), and by members of the Enterprise Admins group. This is considered a service administrator account because its members have full access to the domain controllers in a domain.

The Domain Admins applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-512|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Administrator|
|Default member of|[Administrators](#administrators)<p>[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Administrators](#administrators)<p>See [Denied RODC Password Replication group](#denied-rodc-password-replication-group)|

### Domain Computers

This group can include all computers and servers that have joined the domain, excluding domain controllers. By default, any computer account that is created automatically becomes a member of this group.

The Domain Computers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-515|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|All computers joined to the domain, excluding domain controllers|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes (but not required)|
|Safe to delegate management of this group to non-Service admins?|Yes |
|Default User Rights|None|

### Domain Controllers

The Domain Controllers group can include all domain controllers in the domain. New domain controllers are automatically added to this group.

The Domain Controllers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-516|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Computer accounts for all domain controllers of the domain|
|Default member of|[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|No|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### Domain Guests

The Domain Guests group includes the domain’s built-in Guest account. When members of this group sign in as local guests on a domain-joined computer, a domain profile is created on the local computer.

The Domain Guests applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-514|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Guest|
|Default member of|[Guests](#guests)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Can be moved out but it is not recommended|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Guests](#guests)|

### Domain Users

The Domain Users group includes all user accounts in a domain. When you create a user account in a domain, it is automatically added to this group.

By default, any user account that is created in the domain automatically becomes a member of this group. This group can be used to represent all users in the domain. For example, if you want all domain users to have access to a printer, you can assign permissions for the printer to this group or add the Domain Users group to a local group on the print server that has permissions for the printer.

The Domain Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-513|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Administrator
krbtgt|
|Default member of|[Users](#users)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Users](#users)|

### Enterprise Admins

The Enterprise Admins group exists only in the root domain of an Active Directory forest of domains. It is a Universal group if the domain is in native mode; it is a Global group if the domain is in mixed mode. Members of this group are authorized to make forest-wide changes in Active Directory, such as adding child domains.

By default, the only member of the group is the Administrator account for the forest root domain. This group is automatically added to the Administrators group in every domain in the forest, and it provides complete access for configuring all domain controllers. Members in this group can modify the membership of all administrative groups. Membership can be modified only by the default service administrator groups in the root domain. This is considered a service administrator account.

The Enterprise Admins applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<root domain>-519|
|Type|Universal (if Domain is in Native-Mode) else Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Administrator|
|Default member of|[Administrators](#administrators)<p>[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Administrators](#administrators)<p>See [Denied RODC Password Replication group](#denied-rodc-password-replication-group)|

### Enterprise Key Admins

Members of this group can perform administrative actions on key objects within the forest.

| Attribute | Value |
|-----------|-------|
| Well-Known SID/RID | S-1-5-21-\<domain>-527 |
| Type | Global |
| Default container | CN=Users, DC=\<domain>, DC= |
| Default members | None |
| Default member of | None |
| Protected by ADMINSDHOLDER? | Yes |
| Safe to move out of default container? | Yes |
| Safe to delegate management of this group to non-Service admins? | No |
| Default User Rights | None |

### Enterprise Read-Only Domain Controllers

Members of this group are Read-Only Domain Controllers in the enterprise. Except for account passwords, a Read-only domain controller holds all the Active Directory objects and attributes that a writable domain controller holds. However, changes cannot be made to the database that is stored on the Read-only domain controller. Changes must be made on a writable domain controller and then replicated to the Read-only domain controller.

Read-only domain controllers address some of the issues that are commonly found in branch offices. These locations might not have a domain controller. Or, they might have a writable domain controller, but not the physical security, network bandwidth, or local expertise to support it.

For more information, see [What is a RODC?](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771030(v=ws.10)).

The Enterprise Read-Only Domain Controllers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<root domain>-498|
|Type|Universal|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?||
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Event Log Readers

Members of this group can read event logs from local computers. The group is created when the server is promoted to a domain controller.

The Event Log Readers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-573|
|Type|Domain Local|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Group Policy Creator Owners

This group is authorized to create, edit, or delete Group Policy Objects in the domain. By default, the only member of the group is Administrator.

For information about other features you can use with this security group, see [Group Policy Overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831791(v=ws.11)).

The Group Policy Creator Owners applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-520|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Administrator|
|Default member of|[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|No|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Denied RODC Password Replication group](#denied-rodc-password-replication-group)|

### Guests

Members of the Guests group have the same access as members of the Users group by default, except that the Guest account has further restrictions. By default, the only member is the Guest account. The Guests group allows occasional or one-time users to sign in with limited privileges to a computer’s built-in Guest account.

When a member of the Guests group signs out, the entire profile is deleted. This includes everything that is stored in the **%userprofile%** directory, including the user's registry hive information, custom desktop icons, and other user-specific settings. This implies that a guest must use a temporary profile to sign in to the system. This security group interacts with the Group Policy setting. **Do not logon users with temporary profiles** when it is enabled. This setting is located under the following path: **Computer Configuration > Administrative Templates > System > User Profiles**

> [!NOTE]
> A Guest account is a default member of the Guests security group. People who do not have an actual account in the domain can use the Guest account. A user whose account is disabled (but not deleted) can also use the Guest account.The Guest account does not require a password. You can set rights and permissions for the Guest account as in any user account. By default, the Guest account is a member of the built-in Guests group and the Domain Guests global group, which allows a user to sign in to a domain. The Guest account is disabled by default, and we recommend that it stay disabled.

The Guests applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-546|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|[Domain Guests](#domain-guests)|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### Hyper-V Administrators

Members of the Hyper-V Administrators group have complete and unrestricted access to all the features in Hyper-V. Adding members to this group helps reduce the number of members required in the Administrators group, and further separates access.

> [!NOTE]
> Prior to Windows Server 2012, access to features in Hyper-V was controlled in part by membership in the Administrators group.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-578|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### IIS\_IUSRS

IIS\_IUSRS is a built-in group that is used by Internet Information Services beginning with IIS 7.0. A built-in account and group are guaranteed by the operating system to always have a unique SID. IIS 7.0 replaces the IUSR\_MachineName account and the IIS\_WPG group with the IIS\_IUSRS group to ensure that the actual names that are used by the new account and group will never be localized. For example, regardless of the language of the Windows operating system that you install, the IIS account name will always be IUSR, and the group name will be IIS\_IUSRS.

For more information, see [Understanding Built-In User and Group Accounts in IIS 7](/iis/get-started/planning-for-security/understanding-built-in-user-and-group-accounts-in-iis).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-568|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=\<domain>, DC=|
|Default members|IUSR|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?||
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Incoming Forest Trust Builders

Members of the Incoming Forest Trust Builders group can create incoming, one-way trusts to this forest. Active Directory provides security across multiple domains or forests through domain and forest trust relationships. Before authentication can occur across trusts, Windows must determine whether the domain being requested by a user, computer, or service has a trust relationship with the logon domain of the requesting account.

To make this determination, the Windows security system computes a trust path between the domain controller for the server that receives the request and a domain controller in the domain of the requesting account. A secured channel extends to other Active Directory domains through interdomain trust relationships. This secured channel is used to obtain and verify security information, including security identifiers (SIDs) for users and groups.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

For more information, see [How Domain and Forest Trusts Work: Domain and Forest Trusts](/previous-versions/windows/it-pro/windows-server-2003/cc773178(v=ws.10)).

The Incoming Forest Trust Builders applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-557|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### Key Admins

Members of this group can perform administrative actions on key objects within the domain.

The Key Admins applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

| Attribute | Value |
|-----------|-------|
| Well-Known SID/RID | S-1-5-21-\<domain>-526 |
| Type | Global |
| Default container | CN=Users, DC=\<domain>, DC= |
| Default members | None |
| Default member of | None |
| Protected by ADMINSDHOLDER? | Yes |
| Safe to move out of default container? | Yes |
| Safe to delegate management of this group to non-Service admins? | No |
| Default User Rights | None |

### Network Configuration Operators

Members of the Network Configuration Operators group can have the following administrative privileges to manage configuration of networking features:

- Modify the Transmission Control Protocol/Internet Protocol (TCP/IP) properties for a local area network (LAN) connection, which includes the IP address, the subnet mask, the default gateway, and the name servers.

- Rename the LAN connections or remote access connections that are available to all the users.

- Enable or disable a LAN connection.

- Modify the properties of all of remote access connections of users.

- Delete all the remote access connections of users.

- Rename all the remote access connections of users.

- Issue **ipconfig**, **ipconfig /release**, or **ipconfig /renew** commands.

- Enter the PIN unblock key (PUK) for mobile broadband devices that support a SIM card.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Network Configuration Operators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-556|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### Performance Log Users

Members of the Performance Log Users group can manage performance counters, logs, and alerts locally on the server and from remote clients without being a member of the Administrators group. Specifically, members of this security group:

- Can use all the features that are available to the Performance Monitor Users group.

- Can create and modify Data Collector Sets after the group is assigned the [Log on as a batch job](/windows/device-security/security-policy-settings/log-on-as-a-batch-job) user right.

  > [!WARNING]
  > If you are a member of the Performance Log Users group, you must configure Data Collector Sets that you create to run under your credentials.

  > [!NOTE]
  > In Windows Server 2016 or later, Data Collector Sets cannot be created by a member of the Performance Log Users group. If a member of the Performance Log Users group tries to create Data Collector Sets, they cannot complete creation because access will be denied.

- Cannot use the Windows Kernel Trace event provider in Data Collector Sets.

For members of the Performance Log Users group to initiate data logging or modify Data Collector Sets, the group must first be assigned the [Log on as a batch job](/windows/device-security/security-policy-settings/log-on-as-a-batch-job) user right. To assign this user right, use the Local Security Policy snap-in in Microsoft Management Console.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Performance Log Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

> [!NOTE]
> This account cannot be renamed, deleted, or moved.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-559|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|[Log on as a batch job](/windows/device-security/security-policy-settings/log-on-as-a-batch-job): SeBatchLogonRight|

### Performance Monitor Users

Members of this group can monitor performance counters on domain controllers in the domain, locally and from remote clients without being a member of the Administrators or Performance Log Users groups. The Windows Performance Monitor is a Microsoft Management Console (MMC) snap-in that provides tools for analyzing system performance. From a single console, you can monitor application and hardware performance, customize what data you want to collect in logs, define thresholds for alerts and automatic actions, generate reports, and view past performance data in various ways.

Specifically, members of this security group:

- Can use all the features that are available to the Users group.

- Can view real-time performance data in Performance Monitor.

- Can change the Performance Monitor display properties while viewing data.

- Cannot create or modify Data Collector Sets.

> [!WARNING]
> You cannot configure a Data Collector Set to run as a member of the Performance Monitor Users group.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO). This group cannot be renamed, deleted, or moved.

The Performance Monitor Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-558|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### Pre–Windows 2000 Compatible Access

Members of the Pre–Windows 2000 Compatible Access group have Read access for all users and groups in the domain. This group is provided for backward compatibility for computers running Windows NT 4.0 and earlier. By default, the special identity group, Everyone, is a member of this group. Add users to this group only if they are running Windows NT 4.0 or earlier.

> [!WARNING]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Pre–Windows 2000 Compatible Access applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-554|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|If you choose the Pre–Windows 2000 Compatible Permissions mode, Everyone and Anonymous are members. If you choose the Windows 2000-only permissions mode, Authenticated Users are members.|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Access this computer from the network](/windows/device-security/security-policy-settings/access-this-computer-from-the-network): SeNetworkLogonRight<p>[Bypass traverse checking](/windows/device-security/security-policy-settings/bypass-traverse-checking): SeChangeNotifyPrivilege|

### Print Operators

Members of this group can manage, create, share, and delete printers that are connected to domain controllers in the domain. They can also manage Active Directory printer objects in the domain. Members of this group can locally sign in to and shut down domain controllers in the domain.

This group has no default members. Because members of this group can load and unload device drivers on all domain controllers in the domain, add users with caution. This group cannot be renamed, deleted, or moved.

The Print Operators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

For more information, see [Assign Delegated Print Administrator and Printer Permission Settings in Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj190062(v=ws.11)).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-550|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](/windows/device-security/security-policy-settings/allow-log-on-locally): SeInteractiveLogonRight<p>[Load and unload device drivers](/windows/device-security/security-policy-settings/load-and-unload-device-drivers): SeLoadDriverPrivilege<p>[Shut down the system](/windows/device-security/security-policy-settings/shut-down-the-system): SeShutdownPrivilege|

### Protected Users

Members of the Protected Users group are afforded additional protection against the compromise of credentials during authentication processes.

This security group is designed as part of a strategy to effectively protect and manage credentials within the enterprise. Members of this group automatically have non-configurable protection applied to their accounts. Membership in the Protected Users group is meant to be restrictive and proactively secure by default. The only method to modify the protection for an account is to remove the account from the security group.

This domain-related, global group triggers non-configurable protection on devices and host computers, starting with the Windows Server 2012 R2 and Windows 8.1 operating systems. It also triggers non-configurable protection on domain controllers in domains with a primary domain controller running Windows Server 2012 R2 or Windows Server 2016. This greatly reduces the memory footprint of credentials when users sign in to computers on the network from a non-compromised computer.

Depending on the account’s domain functional level, members of the Protected Users group are further protected due to behavior changes in the authentication methods that are supported in Windows.

- Members of the Protected Users group cannot authenticate by using the following Security Support Providers (SSPs): NTLM, Digest Authentication, or CredSSP. Passwords are not cached on a device running Windows 8.1 or Windows 10, so the device fails to authenticate to a domain when the account is a member of the Protected User group.

- The Kerberos protocol will not use the weaker DES or RC4 encryption types in the preauthentication process. This means that the domain must be configured to support at least the AES cipher suite.

- The user’s account cannot be delegated with Kerberos constrained or unconstrained delegation. This means that former connections to other systems may fail if the user is a member of the Protected Users group.

- The default Kerberos ticket-granting tickets (TGTs) lifetime setting of four hours is configurable by using Authentication Policies and Silos, which can be accessed through the Active Directory Administrative Center. This means that when four hours have passed, the user must authenticate again.

The Protected Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

This group was introduced in Windows Server 2012 R2. For more information about how this group works, see [Protected Users Security Group](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466518(v=ws.11)).

The following table specifies the properties of the Protected Users group.

|Attribute|Value|
|--- |--- |
|Well-known SID/RID|S-1-5-21-\<domain>-525|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-service admins?|No|
|Default user rights|None|

### RAS and IAS Servers

Computers that are members of the RAS and IAS Servers group, when properly configured, are allowed to use remote access services. By default, this group has no members. Computers that are running the Routing and Remote Access service are added to the group automatically, such as IAS servers and Network Policy Servers. Members of this group have access to certain properties of User objects, such as Read Account Restrictions, Read Logon Information, and Read Remote Access Information.

The RAS and IAS Servers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-553|
|Type|Builtin Local|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### RDS Endpoint Servers

Servers that are members in the RDS Endpoint Servers group can run virtual machines and host sessions where user RemoteApp programs and personal virtual desktops run. This group needs to be populated on servers running RD Connection Broker. Session Host servers and RD Virtualization Host servers used in the deployment need to be in this group.

For information about Remote Desktop Services, see [Host desktops and apps in Remote Desktop Services](/windows-server/remote/remote-desktop-services/welcome-to-rds).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-576|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### RDS Management Servers

Servers that are members in the RDS Management Servers group can be used to perform routine administrative actions on servers running Remote Desktop Services. This group needs to be populated on all servers in a Remote Desktop Services deployment. The servers running the RDS Central Management service must be included in this group.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-577|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### RDS Remote Access Servers

Servers in the RDS Remote Access Servers group provide users with access to RemoteApp programs and personal virtual desktops. In Internet facing deployments, these servers are typically deployed in an edge network. This group needs to be populated on servers running RD Connection Broker. RD Gateway servers and RD Web Access servers that are used in the deployment need to be in this group.

For more information, see [Host desktops and apps in Remote Desktop Services](/windows-server/remote/remote-desktop-services/welcome-to-rds).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-575|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Read-only Domain Controllers

This group is composed of the Read-only domain controllers in the domain. A Read-only domain controller makes it possible for organizations to easily deploy a domain controller in scenarios where physical security cannot be guaranteed, such as branch office locations, or where local storage of all domain passwords is considered a primary threat, such as in an extranet or in an application-facing role.

Because administration of a Read-only domain controller can be delegated to a domain user or security group, a Read-only domain controller is well suited for a site that should not have a user who is a member of the Domain Admins group. A Read-only domain controller encompasses the following functionality:

- Read-only AD DS database

- Unidirectional replication

- Credential caching

- Administrator role separation

- Read-only Domain Name System (DNS)

For information about deploying a Read-only domain controller, see [Understanding Planning and Deployment for Read-Only Domain Controllers](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754719(v=ws.10)).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-521|
|Type|Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|See [Denied RODC Password Replication group](#denied-rodc-password-replication-group)|

### Remote Desktop Users

The Remote Desktop Users group on an RD Session Host server is used to grant users and groups permissions to remotely connect to an RD Session Host server. This group cannot be renamed, deleted, or moved. It appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Remote Desktop Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-555|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### Remote Management Users

Members of the Remote Management Users group can access WMI resources over management protocols (such as WS-Management via the Windows Remote Management service). This applies only to WMI namespaces that grant access to the user.

The Remote Management Users group is used to allow users to manage servers through the Server Manager console, whereas the [WinRMRemoteWMIUsers\\_](#winrmremotewmiusers_) group allows remotely running Windows PowerShell commands.

For more information, see [What's New in MI?](/previous-versions/windows/desktop/wmi_v2/what-s-new-in-mi) and [About WMI](/windows/win32/wmisdk/about-wmi).

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-580|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Replicator

Computers that are members of the Replicator group support file replication in a domain. Windows Server operating systems use the File Replication service (FRS) to replicate system policies and logon scripts stored in the System Volume (SYSVOL). Each domain controller keeps a copy of SYSVOL for network clients to access. FRS can also replicate data for the Distributed File System (DFS), synchronizing the content of each member in a replica set as defined by DFS. FRS can copy and maintain shared files and folders on multiple servers simultaneously. When changes occur, content is synchronized immediately within sites and by a schedule between sites.

> [!WARNING]
> In Windows Server 2008 R2, FRS cannot be used for replicating DFS folders or custom (non-SYSVOL) data. A Windows Server 2008 R2 domain controller can still use FRS to replicate the contents of a SYSVOL shared resource in a domain that uses FRS for replicating the SYSVOL shared resource between domain controllers.However, Windows Server 2008 R2 servers cannot use FRS to replicate the contents of any replica set apart from the SYSVOL shared resource. The DFS Replication service is a replacement for FRS, and it can be used to replicate the contents of a SYSVOL shared resource, DFS folders, and other custom (non-SYSVOL) data. You should migrate all non-SYSVOL FRS replica sets to DFS Replication.

For more information, see:

- [File Replication Service (FRS) Is Deprecated in Windows Server 2008 R2 (Windows)](/windows/win32/win7appqual/file-replication-service--frs--is-deprecated-in-windows-server-2008-r2)
- [DFS Namespaces and DFS Replication Overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11))

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-552|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### Schema Admins

Members of the Schema Admins group can modify the Active Directory schema. This group exists only in the root domain of an Active Directory forest of domains. It is a Universal group if the domain is in native mode; it is a Global group if the domain is in mixed mode.

The group is authorized to make schema changes in Active Directory. By default, the only member of the group is the Administrator account for the forest root domain. This group has full administrative access to the schema.

The membership of this group can be modified by any of the service administrator groups in the root domain. This is considered a service administrator account because its members can modify the schema, which governs the structure and content of the entire directory.

For more information, see [What Is the Active Directory Schema?](/previous-versions/windows/it-pro/windows-server-2003/cc784826(v=ws.10)).

The Schema Admins applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<root domain>-518|
|Type|Universal (if Domain is in Native-Mode) else Global|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|Administrator|
|Default member of|[Denied RODC Password Replication group](#denied-rodc-password-replication-group)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Denied RODC Password Replication group](#denied-rodc-password-replication-group)|

### Server Operators

Members in the Server Operators group can administer domain controllers. This group exists only on domain controllers. By default, the group has no members. Members of the Server Operators group can perform the following: sign in to a server interactively, create and delete network shared resources, start and stop services, back up and restore files, format the hard disk drive of the computer, and shut down the computer. This group cannot be renamed, deleted, or moved.

By default, this built-in group has no members, and it has access to server configuration options on domain controllers. Its membership is controlled by the service administrator groups Administrators and Domain Admins in the domain, and the Enterprise Admins group in the forest root domain. Members in this group cannot change any administrative group memberships. This is considered a service administrator account because its members have physical access to domain controllers. They can perform maintenance tasks such as backup and restore, and they have the ability to change binaries that are installed on the domain controllers. See default user rights in the following table.

The Server Operators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-549|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](/windows/device-security/security-policy-settings/allow-log-on-locally): SeInteractiveLogonRight<p>[Back up files and directories](/windows/device-security/security-policy-settings/back-up-files-and-directories): SeBackupPrivilege<p>[Change the system time](/windows/device-security/security-policy-settings/change-the-system-time): SeSystemTimePrivilege<p>[Change the time zone](/windows/device-security/security-policy-settings/change-the-time-zone): SeTimeZonePrivilege<p>[Force shutdown from a remote system](/windows/device-security/security-policy-settings/force-shutdown-from-a-remote-system): SeRemoteShutdownPrivilege<p>[Restore files and directories](/windows/device-security/security-policy-settings/restore-files-and-directories): Restore files and directories SeRestorePrivilege<p>[Shut down the system](/windows/device-security/security-policy-settings/shut-down-the-system): SeShutdownPrivilege|

### Storage Replica Administrators

Members of this group have complete and unrestricted access to all features of Storage Replica. The Storage Replica Administrators applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

| Attribute | Value |
|-----------|-------|
| Well-Known SID/RID | S-1-5-32-582 |
| Type | Builtin Local |
| Default container | CN=BuiltIn, DC=\<domain>, DC= |
| Default members | None |
| Default member of | None |
| Protected by ADMINSDHOLDER? | No |
| Safe to move out of default container? | Yes |
| Safe to delegate management of this group to non-Service admins? | No |
| Default User Rights | None |

### System Managed Accounts group

Members of this group are managed by the system.

The System Managed Accounts applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

| Attribute | Value |
|-----------|-------|
| Well-Known SID/RID | S-1-5-32-581 |
| Type | Builtin Local |
| Default container | CN=BuiltIn, DC=\<domain>, DC= |
| Default members | Users |
| Default member of | None |
| Protected by ADMINSDHOLDER? | No |
| Safe to move out of default container? | Yes |
| Safe to delegate management of this group to non-Service admins? | No |
| Default User Rights | None |

### Terminal Server License Servers

Members of the Terminal Server License Servers group can update user accounts in Active Directory with information about license issuance. This is used to track and report TS Per User CAL usage. A TS Per User CAL gives one user the right to access a Terminal Server from an unlimited number of client computers or devices. This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

For more information about this security group, see [Terminal Services License Server Security Group Configuration](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc775331(v=ws.10)).

The Terminal Server License Servers applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-561|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Safe to move out of default container?|Cannot be moved|
|Protected by ADMINSDHOLDER?|No|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### Users

Members of the Users group are prevented from making accidental or intentional system-wide changes, and they can run most applications. After the initial installation of the operating system, the only member is the Authenticated Users group. When a computer joins a domain, the Domain Users group is added to the Users group on the computer.

Users can perform tasks such as running applications, using local and network printers, shutting down the computer, and locking the computer. Users can install applications that only they are allowed to use if the installation program of the application supports per-user installation. This group cannot be renamed, deleted, or moved.

The Users applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

This security group includes the following changes since Windows Server 2008:

- In Windows Server 2008 R2, INTERACTIVE was added to the default members list.

- In Windows Server 2012, the default **Member Of** list changed from Domain Users to none.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-545|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|Authenticated Users<p>[Domain Users](#domain-users)<p>INTERACTIVE|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### Windows Authorization Access Group

Members of this group have access to the computed token GroupsGlobalAndUniversal attribute on User objects. Some applications have features that read the token-groups-global-and-universal (TGGAU) attribute on user account objects or on computer account objects in Active Directory Domain Services. Some Win32 functions make it easier to read the TGGAU attribute. Applications that read this attribute or that call an API (referred to as a function) that reads this attribute do not succeed if the calling security context does not have access to the attribute. This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Windows Authorization Access applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-32-560|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=\<domain>, DC=|
|Default members|Enterprise Domain Controllers|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default user rights|None|

### WinRMRemoteWMIUsers\_

In Windows 8 and in Windows Server 2012, a **Share** tab was added to the Advanced Security Settings user interface. This tab displays the security properties of a remote file share. To view this information, you must have the following permissions and memberships, as appropriate for the version of Windows Server that the file server is running.

The WinRMRemoteWMIUsers\_ applies to the Windows Server operating system in the [Default Active Directory security groups](#default-active-directory-security-groups) list above.

- If the file share is hosted on a server that is running a supported version of the operating system:

  - You must be a member of the WinRMRemoteWMIUsers\_\_ group or the BUILTIN\\Administrators group.

  - You must have Read permissions to the file share.

- If the file share is hosted on a server that is running a version of Windows Server that is earlier than Windows Server 2012:

  - You must be a member of the BUILTIN\\Administrators group.

  - You must have Read permissions to the file share.

In Windows Server 2012, the Access Denied Assistance functionality adds the Authenticated Users group to the local WinRMRemoteWMIUsers\_\_ group. Therefore, when the Access Denied Assistance functionality is enabled, all authenticated users who have Read permissions to the file share can view the file share permissions.

> [!NOTE]
> The WinRMRemoteWMIUsers\_\_ group allows running Windows PowerShell commands remotely whereas the [Remote Management Users](#remote-management-users) group is generally used to allow users to manage servers by using the Server Manager console.

|Attribute|Value|
|--- |--- |
|Well-Known SID/RID|S-1-5-21-\<domain>-\<variable RI>|
|Type|Domain local|
|Default container|CN=Users, DC=\<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

## See also

- [Security Principals](understand-security-principals.md)

- [Special Identities](understand-special-identities-groups.md)

- [Access Control Overview](/windows/security/identity-protection/access-control/access-control)
