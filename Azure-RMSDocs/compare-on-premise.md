---
# required metadata

title: Compare Azure Information Protection & AD RMS - AIP
description: If you know or have previously deployed Active Directory Rights Management Services (AD RMS), you might be wondering how Azure Information Protection compares in terms of functionality and requirements. 
author: mlottner
ms.author: mlottner
manager: rkarlin
ms.date: 04/28/2020
ms.topic: conceptual
ms.collection: M365-security-compliance
ms.service: information-protection
ms.assetid: 8123bd62-1814-4d79-b306-e20c1a00e264

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: admin

---

# Comparing Azure Information Protection and AD RMS

>*Applies to: Active Directory Rights Management Services, [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection), [Office 365](https://download.microsoft.com/download/E/C/F/ECF42E71-4EC0-48FF-AA00-577AC14D5B5C/Azure_Information_Protection_licensing_datasheet_EN-US.pdf)*

If you know or have previously deployed Active Directory Rights Management Services (AD RMS), you might be wondering how Azure Information Protection compares in terms of functionality and requirements as an information protection solution.

Some of the main differences for Azure Information Protection:

- **No server infrastructure required**: Azure Information Protection doesn't require the additional servers and PKI certificates that AD RMS needs, because Microsoft Azure takes care of those for you. That makes this cloud solution quicker to deploy and easier to maintain.

- **Cloud-based authentication**: Azure Information Protection uses Azure AD for authentication - for both internal users and users from other organizations. That means your users can be authenticated even when they are not connected to your internal network and it is easier to share protected content with users from other organizations. Many organizations already have user accounts in Azure AD because they are running Azure services or have Microsoft 365. But if not, RMS for individuals lets users create a free account, or a Microsoft account can be used for [applications that support this authentication for Azure Information Protection](secure-collaboration-documents.md#supported-scenarios-for-opening-protected-documents). In comparison, to share AD RMS protected content with another organization, you must configure explicit trusts with each organization.

- **Built-in support for mobile devices**: No deployment changes are needed for Azure Information Protection to support mobile devices and Mac computers. To support these devices with AD RMS, you must install the mobile device extension, configure AD FS for federation, and create additional records for your public DNS service.

- **Default templates**: Azure Information Protection automatically creates default templates that restrict access of the content to your own organization. These templates make it easy to start protecting sensitive data immediately. There are no default templates for AD RMS.

- **Departmental templates**: Also known as scoped templates. Azure Information Protection supports departmental templates for additional templates that you create. This configuration lets you specify a subset of users to see specific templates in their client applications. Limiting the number of templates that users see makes it easier for them to select the correct policy that you define for different groups of users. AD RMS doesn't support departmental templates.

- **Document tracking and revocation**: Azure Information Protection supports these features with the Azure Information Protection client (classic), whereas AD RMS does not.

- **Classification and labeling**: Azure Information Protection supports labels that apply classification, and optionally, protection. These capabilities are provided with the [Azure Information Protection client (classic) and the Azure Information Protection unified labeling client](./rms-client/use-client.md#choose-which-labeling-client-to-use-for-windows-computers). Using these clients, classification and labeling can be integrated with Office applications, File Explorer, PowerShell, and a scanner for on-premises data stores. AD RMS does not support these classification and labeling capabilities.

In addition, because Azure Information Protection is a cloud service, it can deliver new features and fixes more quickly than an on-premises server-based solution. There are no new features planned for AD RMS in Windows Server.

For other differences, use the following table for a side-by-side comparison. If you have security-specific comparison questions, see the [Cryptographic controls for signing and encryption](#cryptographic-controls-for-signing-and-encryption) section in this article.

|Azure Information Protection|AD RMS|
|-----------------------------------------------------------------------------------------|--------------------------------------------------------|
|Supports information rights management (IRM) capabilities in both Microsoft Online services and on-premises Microsoft server products.|Supports information rights management (IRM) capabilities for on-premises Microsoft server products, and Exchange Online.|
|Automatically enables secure collaboration on documents with any organization that also uses Azure AD for authentication.|Secure collaboration on documents outside the organization requires authentication trusts to be explicitly defined in a direct point-to-point relationship between two organizations. You must configure either trusted user domains (TUDs) or federated trusts that you create by using Active Directory Federation Services (AD FS).|
|Send a protected email (optionally, with Office document attachments that are automatically protected) to users when no authentication trust relationship exists. This scenario is made possible by using federation with social providers or a one-time passcode and web browser for viewing.|Does not support sending protected email when no authentication trust relationship exists.|
|Supports the Azure Information Protection client (classic) and the Azure Information Protection unified labeling client for both protection and consumption activities.|Supports the Azure Information Protection client (classic) for protection and consumption activities. <br /><br />Supports the Azure Information Protection unified labeling client for consumption only, and you must install the [Active Directory Rights Management Services Mobile Device Extension](./active-directory-rights-manage-mobile-device.md).
|Supports multi-factor authentication (MFA) for computers and mobile devices.<br /><br />For more information, see the [Multi-factor authentication (MFA) and Azure Information Protection](./requirements-azure-ad.md#multi-factor-authentication-mfa-and-azure-information-protection).|Supports smart card authentication if IIS is configured to request certificates.|
|Supports Cryptographic Mode 2 by default to provide a recommended level of security for key lengths and encryption algorithms.|Supports Cryptographic Mode 1 by default and requires additional configuration to support Cryptographic Mode 2 for a recommended level of security.<br /><br />For more information, see [AD RMS Cryptographic Modes](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh867439(v=ws.10)).|
|Requires an Azure Information Protection license or Azure Rights Management license with Microsoft 365 to protect content. <br /><br />No license is required to consume content that has been protected by Azure Information Protection (includes users from another organization).<br /><br />For more information about licensing, including the differences between a P1 and P2 license, see the [feature list](https://www.microsoft.com/cloud-platform/azure-information-protection-features) from the Azure Information Protection site.|Requires an RMS license to protect content, and to consume  content that has been protected by AD RMS.<br /><br />For more information about licensing, see [Client Access Licenses and Management Licenses](https://www.microsoft.com/Licensing/product-licensing/client-access-license.aspx) for general information, but contact your Microsoft partner or Microsoft representative for specific information.|

## Cryptographic controls for signing and encryption
Azure Information Protection by default, uses RSA 2048 for all public key cryptography and SHA 256 for signing operations. In comparison, AD RMS supports RSA 1024 and RSA 2048, and SHA 1 or SHA 256 for signing operations.

Both Azure Information Protection and AD RMS use AES 128 for symmetric encryption.

Azure Information Protection is compliant with FIPS 140-2 when your tenant key size is 2048 bits, which is the default when the Azure Rights Management service is activated. 

For more information about the cryptographic controls, see [Cryptographic controls used by Azure RMS: Algorithms and key length](how-does-it-work.md#cryptographic-controls-used-by-azure-rms-algorithms-and-key-lengths).


## Next steps
For more detailed requirements to use Azure Information Protection, such as device support and minimum versions, see [Requirements for Azure Information Protection](requirements.md).

If you are looking to migrate from AD RMS to Azure Information Protection, see [Migrating from AD RMS to Azure Information Protection](migrate-from-ad-rms-to-azure-rms.md).

Get started with [Active Directory Rights Management Services Mobile Device Extension](./active-directory-rights-manage-mobile-device.md). 

You might be interested in the following FAQs:
- [What's the difference between Azure Information Protection and Microsoft Information Protection?](faqs.md#whats-the-difference-between-azure-information-protection-and-microsoft-information-protection)
- [What's the difference between Azure Information Protection and Azure Rights Management?](faqs.md#whats-the-difference-between-azure-information-protection-and-azure-rights-management)