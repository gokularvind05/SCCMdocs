---
title: "Certificate profile prerequisites"
titleSuffix: "Configuration Manager"
description: "Learn about certificate profiles in System Center Configuration Manager and their external dependencies and dependencies in the product."
ms.custom: na
ms.date: 03/29/2017
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
  - configmgr-other
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 0317fd02-3721-4634-b18b-7c976a4e92bf
caps.latest.revision: 9
author: arob98
ms.author: angrobe
manager: angrobe

---
# Prerequisites for certificate profiles in System Center Configuration Manager

*Applies to: System Center Configuration Manager (Current Branch)*


Certificate profiles in System Center Configuration Manager have external dependencies and dependencies in the product.  

## Dependencies External to Configuration Manager  

|Dependency|More information|  
|----------------|----------------------|  
|An enterprise issuing certification authority (CA) that is running Active Directory Certificate Services (AD CS).<br /><br /> To revoke certificates the computer account of the site server at the top of the hierarchy requires *Issue and Manage Certificates* rights for each certificate template used by a certificate profile in Configuration Manager. Alternatively, grant Certificate Manager permissions to grant permissions on all certificate templates used by that CA<br /><br /> Manager approval for certificate requests is supported. However, the certificate templates that are used to issue certificates must be configured for **Supply in the request** for the certificate subject so that System Center Configuration Manager can automatically supply this value.|For more information about Active Directory Certificate Services, see your Windows Server documentation.<br /><br /> For Windows Server 2012: [Active Directory Certificate Services Overview](http://go.microsoft.com/fwlink/p/?LinkId=286744)<br /><br /> For Windows Server 2008: [Active Directory Certificate Services in Windows Server 2008](http://go.microsoft.com/fwlink/p/?LinkId=115018)|  
|The Network Device Enrollment Service role service for Active Directory Certificate Services, running on Windows Server 2012 R2.<br /><br /> In addition:<br /><br /> Port numbers other than TCP 443 (for HTTPS) or TCP 80 (for HTTP) are not supported for the communication between the client and the Network Device Enrollment Service.<br /><br /> The server that is running the Network Device Enrollment Service must be on a different server from the issuing CA.|System Center Configuration Manager communicates with the Network Device Enrollment Service in Windows Server 2012 R2 to generate and verify Simple Certificate Enrollment Protocol (SCEP) requests.<br /><br /> If you will issue certificates to users or devices that connect from the Internet, such as mobile devices that are managed by Microsoft Intune, those devices must be able to access the server that runs the Network Device Enrollment Service from the Internet. For example, install the server in a perimeter network (also known as a DMZ, demilitarized zone, and screened subnet).<br /><br /> If you have a firewall between the server that is running the Network Device Enrollment Service and the issuing CA, you must configure the firewall to allow the communication traffic (DCOM) between the two servers. This firewall requirement also applies to the server running the System Center Configuration Manager site server and the issuing CA, so that System Center Configuration Manager can revoke certificates.<br /><br /> If the Network Device Enrollment Service is configured to require SSL, a security best practice is to make sure that connecting devices can access the certificate revocation list (CRL) to validate the server certificate.<br /><br /> For more information about the Network Device Enrollment Service in Windows Server 2012 R2, see [Using a Policy Module with the Network Device Enrollment Service](http://go.microsoft.com/fwlink/p/?LinkId=328657).|  
|If the issuing CA runs Windows Server 2008 R2, this server requires a hotfix for SCEP renewal requests.|If the hotfix is not already installed on the issuing CA computer, install the hotfix. For more information, see article [2483564: Renewal request for an SCEP certificate fails in Windows Server 2008 R2 if the certificate is managed by using NDES](http://go.microsoft.com/fwlink/?LinkId=311945) in the Microsoft Knowledge Base.|  
|A PKI client authentication certificate and exported root CA certificate.|This certificate authenticates the server that is running the Network Device Enrollment Service to System Center Configuration Manager.<br /><br /> For more information, see [PKI certificate requirements for System Center Configuration Manager](../../core/plan-design/network/pki-certificate-requirements.md).|  
|Supported device operating systems.|You can deploy certificate profiles to devices that run iOS, Windows 8.1, Windows RT 8.1, Windows 10, and Android operating systems.|  

## Configuration Manager Dependencies  

|Dependency|More information|  
|----------------|----------------------|  
|Certificate registration point site system role|Before you can use certificate profiles, you must install the certificate registration point site system role. This role communicates with the System Center Configuration Manager database, the System Center Configuration Manager site server, and the System Center Configuration Manager Policy Module.<br /><br /> For more information about system requirements for this site system role and where to install the role in the hierarchy, see the **Site System Requirements** section in the [Supported configurations for System Center Configuration Manager](../../core/plan-design/configs/supported-configurations.md) topic.<br /><br /> Note that the certificate registration point must not be installed on the same server that runs the Network Device Enrollment Service.|  
|System Center Configuration Manager Policy Module that is installed on the server that is running the Network Device Enrollment Service role service for Active Directory Certificate Services|To deploy certificate profiles, you must install the System Center Configuration Manager Policy Module. You can find this policy module on the System Center Configuration Manager installation media.|  
|Discovery data|Values for the certificate subject and the subject alternative name are supplied by System Center Configuration Manager and retrieved from information that is collected from discovery:<br /><br /> For user certificates: Active Directory User Discovery<br /><br /> For computer certificates: Active Directory System Discovery and Network Discovery|  
|Specific security permissions to manage certificate profiles|You must have the following security permissions to manage company resource access settings, such as certificate profiles, Wi-Fi profiles and VPN profiles:<br /><br /> To view and manage alerts and reports for certificate profiles: **Create**, **Delete**, **Modify**, **Modify Report**, **Read**, and **Run Report** for the **Alerts** object.<br /><br /> To create and manage certificate profiles: **Author Policy**, **Modify Report**, **Read** and **Run Report** for the **Certificate Profile** object.<br /><br /> To manage Wi-Fi, certificate and VPN profile deployments: **Deploy Configuration Policies**, **Modify Client Status Alert**, **Read**, and **Read Resource** for the **Collection** object.<br /><br /> To manage all configuration policies: **Create**, **Delete**, **Modify**, **Read** and **Set Security Scope** for the **Configuration Policy** object.<br /><br /> To run queries related to certificate profiles: **Read** permission for the **Query** object.<br /><br /> To view certificate profiles information in the System Center Configuration Manager console: **Read** permission for the **Site** object.<br /><br /> To view status messages for certificate profiles: **Read** permission for the **Status Messages** object.<br /><br /> To create and modify the Trusted CA certificate profile: **Author Policy**, **Modify Report**, **Read** and **Run Report** for the **Trusted CA Certificate Profile** object.<br /><br /> To create and manage VPN profiles: **Author Policy**, **Modify Report**, **Read** and **Run Report** for the **VPN Profile** object.<br /><br /> To create and manage Wi-Fi profiles: **Author Policy**, **Modify Report**, **Read** and **Run Report** for the **Wi-Fi Profile** object.<br /><br /> The **Company Resource Access Manager** security role includes these permissions that are required to manage certificate profiles in System Center Configuration Manager. For more information, see the **Configure role-based administration** section in the [Configure security in System Center Configuration Manager](../../core/plan-design/security/configure-security.md) topic.|  
