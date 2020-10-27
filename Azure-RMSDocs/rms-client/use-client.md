---
# required metadata

title: The client for Azure Information Protection - AIP
description: Microsoft Azure Information Protection provides a client-server solution that helps to protect an organization's data. The client (the Azure Information Protection client or the Rights Management client) is integrated with applications that you run on computers and mobile devices.
author: batamig
ms.author: bagol
manager: rkarlin
ms.date: 10/25/2020
ms.topic: conceptual
ms.collection: M365-security-compliance
ms.service: information-protection

# optional metadata

#audience:
#ms.devlang:
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: admin
search.appverid:
- MET150

---

# The client side of Azure Information Protection

>*Applies to: Active Directory Rights Management Services, [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection), Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012*

>[!NOTE] 
> To provide a unified and streamlined customer experience, **Azure Information Protection client (classic)** and **Label Management** in the Azure Portal are being **deprecated** as of **March 31, 2021**. This time-frame allows all current Azure Information Protection customers to transition to our unified labeling solution using the Microsoft Information Protection Unified Labeling platform. Learn more in the official [deprecation notice](https://aka.ms/aipclassicsunset).


Azure Information Protection provides a client-server solution that helps to protect an organization's documents and emails:

- The client can be the built-in labeling client for Office, the Azure Information Protection unified labeling client for Windows, the Azure Information Protection client (classic) for Windows, or the Rights Management client.
    
    These clients are often referred to as the **Office built-in labeling client**, the **unified labeling client**, the **classic client**, and the **RMS client**, respectively. Whichever client you use, it integrates with applications that you run on computers and mobile devices.

- The service resides in the cloud or on-premises. The cloud service is Azure Information Protection, which uses the Azure Rights Management service for the data protection. The on-premises service is Active Directory Rights Management Services, more commonly known as AD RMS. 

All these clients integrate with Office applications but the unified labeling client and the classic client must be installed separately and support additional features and components. For example, these clients include support for File Explorer, so you can classify and protect files outside Office. Additional components include a viewer for protected PDF documents and protected images, and a scanner for on-premises data stores.

The RMS client provides protection only. This client is automatically installed with some applications, such as Office applications, the Azure Information Protection clients, and RMS-enlightened applications from software vendors. However, it can also be [installed by itself](https://www.microsoft.com/download/details.aspx?id=38396), to support [synchronizing files from IRM-protected libraries and OneDrive](/onedrive/deploy-on-windows), and for developers who want to integrate rights management protection into line-of-business applications.

## Choose which labeling client to use for Windows computers

Where possible, use one of the labeling clients because labels abstract the complexity of applying protection for users, and labels also provide classification so you can track and manage your data.

Your choice of labeling client for your Windows computers might be influenced by which management portal you use:

- The Office built-in labeling client and the Azure Information Protection unified labeling client download labels and policy settings from the following admin centers: 
    - Office 365 Security & Compliance Center
    - Microsoft 365 security center
    - Microsoft 365 compliance center

- The Azure Information Protection client (classic) downloads label and policy settings from the Azure portal.

Because the unified labeling client and the classic client require a separate installation to Office, you must download and install these clients from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53018). 

Use the following sections to help you determine which client is best for your organization:

- [Built-in Office labeling client](#built-in-office-labeling-client)
- [Azure Information Protection unified labeling client](#azure-information-protection-unified-labeling-client)
- [Azure Information Protection classic client](#azure-information-protection-classic-client)
- [Using multiple clients in the same environment](#using-multiple-clients-in-the-same-environment)

For more information, see:
[Detailed comparisons for the AIP clients](#detailed-comparisons-for-the-azure-information-protection-clients) and [Features not planned for the unified labeling client](#features-not-planned-to-be-in-the-azure-information-protection-unified-labeling-client).

> [!NOTE]
> The latest version of the unified labeling client brings it to close parity in features with the classic client. As this gap closes, you can expect new features to be added only to the unified labeling client. 
>
> We recommend that you deploy the unified labeling client if its current feature set and functionality meet your business requirements.
> 

### Built-in Office labeling client

The labeling client that's built in to Microsoft Office:

- Requires a Windows computer with Microsoft 365 applications, minimum version 1910
- Enables you to share labels and policy settings that can also be used by macOS, iOS, and Android
- Supports switching accounts
- Provides better performance in Office applications
- Does not require a separate installation and maintenance
- Cannot be disabled.

**Don't use** the built-in Office labeling client if you need features provided only by the classic or unified labeling clients, such as the Information Protection bar under the ribbon. This bar provides easier label selection and visibility.

### Azure Information Protection unified labeling client

The unified labeling client requires a Windows computer, and enables you to share labels and policy settings that can also be used by macOS, iOS, and Android.

**Don't use** the unified labeling client if the current unified labeling features not meet your business requirements, or if you have configured labels in the Azure portal that you haven't yet [migrated to the unified labeling store](../configure-policy-migrate-labels.md).

### Azure Information Protection classic client

The classic client:

- Requires a Windows computer
- Provides access to features not yet available on the unified labeling client, such as holding your own on-premises key (HYOK), and a general availability version of the scanner for on-premises data stores. 
- Enables you to share labels with macOS, iOS, and Android

However, the classic client has different policy settings for macOS, iOS, and Android. So, while you may want to use the additional features, you'll have to work with a separate management portal and user experience to protect content across operating systems.

**Don't use** the classic client if you want newer features available only in the unified labeling client, or to provide a centralized, unified user experience.

### Using multiple clients in the same environment

You can use different clients in the same environment to support different business requirements, as demonstrated in the following deployment example. In a mixed client environment, we recommend you use unified labels so that clients share the same set of labels for ease of administration. New customers have unified labels by default because their tenants are on the unified labeling platform. For more information, see [How can I determine if my tenant is on the unified labeling platform?](../faqs.md#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform)

When you have a Windows computer that runs Microsoft 365 apps that are a minimum version 1910 and one of the Azure Information Protection clients is installed, by default the built-in labeling client is disabled in Office apps. However, you can change this behavior to use the built-in labeling client for just your Office apps. With this configuration, the Azure Information Protection client (classic or unified labeling) remains available for labeling in File Explorer, PowerShell, and the scanner. For instructions to disable the Azure Information Protection client in Microsoft 365 apps, see the section [Office built-in labeling client and the Azure Information Protection client](/microsoft-365/compliance/sensitivity-labels-office-apps#office-built-in-labeling-client-and-the-azure-information-protection-client) from the Microsoft 365 Compliance documentation.

##### Example deployment strategy:

- For the majority of users, you deploy the Azure Information Protection unified labeling client because this client meets the business needs for these users. 
    
    For these users, their labeling experience is similar across Windows, Mac, iOS, and Android because they have the same labels published to them and the same policy settings. As an admin, you manage these labels and policy settings in the same management center.

- You also install the unified labeling client for yourself, to test the Azure Information Protection scanner.

- For a subset of users, you deploy the classic client because these users require labels that apply hold your own key (HYOK) protection.
    
    For these users, they have a slightly different labeling experience when they use this client. For example, they see a **Protect** button rather than a **Sensitivity** button in Office apps. As an admin, you need to manage their labels for HYOK settings and policy settings in a different management center to the labels and settings for the other client platforms.

- You have on-premises data stores with documents that need to be scanned for sensitive information, or classified and protected. For production use, you deploy the classic client on servers to run the Azure Information Protection scanner.

## Compare the labeling clients for Windows computers

Use the following table to help compare which features are supported by the three labeling clients for Windows computers.

To compare the Office built-in sensitivity labeling features across different operating system platforms (Windows, macOS, iOS, and Android) and for the web, see the Microsoft 365 Compliance documentation, [Support for sensitivity label capabilities in apps](/microsoft-365/compliance/sensitivity-labels-office-apps#support-for-sensitivity-label-capabilities-in-apps). This documentation also includes the Office build numbers or Office update channel information for the supported features.

|Feature|Classic client|Unified labeling client|Office built-in labeling client|
|:------|:------------:|:---------------------:|:-----------------------------:|
|Manual labeling:| **Yes** | **Yes** |**Yes** |
|Default label:| **Yes** | **Yes** | **Yes** |
|Recommended or automatic labeling: <br />- For Word, Excel, PowerPoint, Outlook| **Yes** | **Yes** | **Yes** |
|Mandatory labeling:| **Yes** | **Yes** | No |
|User-defined permissions for a label: <br />- Do Not Forward for emails| **Yes** | **Yes** | **Yes** |
|User-defined permissions for a label: <br />- Custom permissions for Word, Excel, PowerPoint| **Yes** | **Yes** | **Yes** |
|Multilanguage support for labels:| **Yes** | **Yes** |**Yes** |
|Label inheritance from email attachments:| **Yes** | **Yes**  |No |
|Customizations that include:<br />- Default label for email<br />- Pop up messages in Outlook <br />- S/MIME support<br />- Report an Issue option| **Yes** <sup>1</sup> | **Yes** <sup>2</sup> | No |
|Scanner for on-premises data stores:| **Yes** | **Yes <br />** | No |
|Central reporting (analytics):| **Yes** | **Yes** | No |
|Custom permissions set independently from a label:| **Yes** | **Yes** <sup>3</sup>| No |
|Information Protection bar in Office apps:| **Yes** | **Yes**| No |
|Visual markings as a label action (header, footer, watermark):| **Yes** | **Yes** | **Yes**|
|Per app visual markings:| **Yes** | **Yes** | No |
|Dynamic visual markings with variables:| **Yes** | **Yes** | **Yes** <sup>9</sup>|
|Remove external content marking in app:| **Yes**| **Yes**| No|
|Label with File Explorer:| **Yes** | **Yes** | No |
|A viewer for protected files (text, images, PDF, .pfile):| **Yes** | **Yes** | No|
|PPDF support for applying labels:| **Yes** | No | No |
|PowerShell labeling cmdlets:| **Yes** | **Yes**  | No |
|Offline support for protection actions:| **Yes** | **Yes** <sup>4</sup> | **Yes** |
|Manual policy file management for disconnected computers:| **Yes** |**Yes**| No |
|HYOK support:| **Yes** | No | No |
|Usage logging in Event Viewer:| **Yes** | No |No |
|Display the Do Not Forward button in Outlook:| **Yes** | No | No |
|Track protected documented:| **Yes** | **Yes** <sup>5</sup> | No |
|Revoke protected documents:| **Yes** | No | No |
|Protection-only mode (no labels):| **Yes** | No | No |
|Support for account switching:| No | No | **Yes** |
|Support for Remote Desktop Services:| **Yes** | **Yes** | **Yes** |
|Support for AD RMS:| **Yes** | No <sup>6</sup> | No |
|Support for Microsoft Office 97-2003 formats| **Yes** | **Yes** | No <sup>8</sup>|
|Double Key Encryption:| No | **Yes** | No|
|Government Community Cloud: | **Yes** | **Yes** | No|
| | | | |

**Footnotes:**

<sup>1</sup>
These settings, and many more are supported as [advanced client settings that you configure in the Azure portal](client-admin-guide-customizations.md#how-to-configure-advanced-client-configuration-settings-in-the-portal).

<sup>2</sup>
These settings, and many more are supported as [advanced settings that you configure with PowerShell](clientv2-admin-guide-customizations.md#how-to-configure-advanced-settings-for-the-client-by-using-office-365-security--compliance-center-powershell).

<sup>3</sup>
Supported by File Explorer and PowerShell. In Office apps, users can select **File Info** > **Protect Document** > **Restrict Access**.

<sup>4</sup>
For File Explorer and PowerShell commands, the user must be connected to the internet to protect files.

<sup>5</sup>
The document tracking site that's supported by the classic client isn't supported by the unified labeling client. However, without the need to first register the document for tracking, administrators can use [central reporting](../reports-aip.md) to identify whether protected documented are accessed from Windows computers, and whether access was granted or denied. 

<sup>6</sup>
Labeling and protection actions aren't supported. However, for an AD RMS deployment, the viewer can open protected documents when you use the [Active Directory Rights Management Services Mobile Device Extension](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn673574\(v=ws.11\)).

<sup>8</sup>
While the AIP clients support both Microsoft Office 97-2003 file formats, such as **.doc,** as well as Office Open XML formats, such as **.docx,** the built-in labeling supports Open XML formats only.

<sup>9</sup>
For more information on support for dynamic content markings in the built-in labeling client, see the [Microsoft 365 documentation](/microsoft-365/compliance/sensitivity-labels-office-apps#dynamic-markings-with-variables).

### Detailed comparisons for the Azure Information Protection clients

When the Azure Information Protection client (classic) and the Azure Information Protection unified labeling client both support the same feature, use the following table to help identify some functional differences between the two clients.

|Functionality |Classic client|Unified labeling client|
|--------------|-----------------------------------|-----------------------------------------------------------|
|Setup:| Option to install local demo policy | No local demo policy|
|Label selection and display when applied in Office apps:|From the **Protect** button on the ribbon <br /><br /> From the Information Protection bar (horizontal bar under the ribbon)|From the **Sensitivity** button on the ribbon<br /><br /> From the Information Protection bar (horizontal bar under the ribbon)|
|Manage the Information Protection bar in Office apps:|For users: <br /><br />- Option to show or hide the bar from the **Protect** button on the ribbon<br /><br />- When a user selects to hide the bar, by default, the bar is hidden in that app, but continues to automatically display in newly opened apps <br /><br /> For admins: <br /><br />- Policy settings to automatically show or hide the bar when an app first opens, and control whether the bar automatically remains hidden for newly opened apps after a user selects to hide the bar|For users: <br /><br />- Option to show or hide the bar from the **Sensitivity** button on the ribbon<br /><br />- When a user selects to hide the bar, the bar is hidden in that app and also in newly opened apps <br /><br />For admins: <br /><br />- PowerShell setting to manage the bar |
|Label color: | Configure in the Azure portal | Retained after label migration and configurable with [PowerShell](clientv2-admin-guide-customizations.md#specify-a-color-for-the-label)|
|Labels support different languages:| Configure in the Azure portal | Configure by using [Office 365 Security & Compliance PowerShell](/microsoft-365/compliance/create-sensitivity-labels#additional-label-settings-with-office-365-security--compliance-center-powershell)|
|Policy update: | When an Office app opens <br /><br /> When you right-click to classify and protect a file or folder <br /><br />When you run the PowerShell cmdlets for labeling and protection<br /><br />Every 24 hours <br /><br />For the scanner: Every hour and when the service starts and the policy is older than one hour| When an Office app opens <br /><br /> When you right-click to classify and protect a file or folder <br /><br />When you run the PowerShell cmdlets for labeling and protection<br /><br />Every 4 hours <br /><br />For the scanner: Every 4 hours|
|Supported formats for PDF:| Protection: <br /><br /> - ISO standard for PDF encryption (default) <br /><br /> - .ppdf <br /><br /> Consumption: <br /><br /> - ISO standard for PDF encryption <br /><br />- .ppdf<br /><br />- SharePoint IRM protection| Protection: <br /><br /> - ISO standard for PDF encryption <br /><br /> <br /><br /> Consumption: <br /><br /> - ISO standard for PDF encryption <br /><br />- .ppdf<br /><br />- SharePoint IRM protection|
|Generically protected files (.pfile) opened with the viewer:| File opens in the original app where it can then be viewed, modified, and saved without protection | File opens in the original app where it can then be viewed and modified, but not saved|
|Supported cmdlets:| Cmdlets for labeling and cmdlets for protection-only | Cmdlets for labeling:<br /><br /> Set-AIPFileClassification and Set-AIPFileLabel don't support the *Owner* parameter <br /><br /> In addition, there is a single comment of "No label to apply" for all scenarios where a label isn't applied <br /><br /> Set-AIPFileClassification supports the *WhatIf* parameter, so it can be run in discovery mode <br /><br /> Set-AIPFileLabel doesn't support the *EnableTracking* parameter <br /><br /> Get-AIPFileStatus doesn't return label information from other tenants and doesn't display the *RMSIssuedTime* parameter<br /><br />In addition, the *LabelingMethod* parameter for Get-AIPFileStatus displays **Privileged** or **Standard** instead of **Manual** or **Automatic**. For more information, see the [online documentation](/powershell/module/azureinformationprotection/get-aipfilestatus).|
|Justification prompts (if configured) per action in Office: | Frequency: Per file <br /><br /> Lowering the sensitivity level <br /><br /> Removing a label<br /><br /> Removing protection | Frequency: Per session <br /><br /> Lowering the sensitivity level<br /><br /> Removing a label|
|Remove applied label actions: | User is prompted to confirm <br /><br />Default label or automatic label (if configured) isn't automatically applied next time the Office app opens the file  <br /><br />| User isn't prompted to confirm<br /><br /> Default label or automatic label (if configured) is automatically applied next time the Office app opens the file|
|Automatic and recommended labels: | Configured as [label conditions](../configure-policy-classification.md) in the Azure portal with built-in information types and custom conditions that use phrases or regular expressions <br /><br />Configuration options include: <br /><br />- Unique / Not unique count <br /><br /> - Minimum count| Configured in the admin centers with built-in sensitive information types and [custom information types](/microsoft-365/compliance/create-a-custom-sensitive-information-type)<br /><br />Configuration options include:  <br /><br />- Unique count only <br /><br />- Minimum and maximum count <br /><br />- AND and OR support with information types <br /><br />- Keyword dictionary<br /><br />- Customizable confidence level and character proximity|
|Order support for sublabels on attachments: | Enabled with an [advanced client setting](client-admin-guide-customizations.md#enable-order-support-for-sublabels-on-attachments) | Enabled by default, no configuration required|
|Change the default protection behavior for file types: | You can use [registry edits](client-admin-guide-file-types.md#changing-the-default-protection-level-of-files) to override the defaults of native and generic protection | You can use [PowerShell](clientv2-admin-guide-customizations.md#change-which-file-types-to-protect) to change which file types get protected|
|Automatic rescans | Full rescans are automatically run every time the scanner detects a change in policy or labeling settings | Starting in version [2.8.85.0](unifiedlabelingclient-version-release-history.md#version-28850), administrators can choose to skip a full rescan after making changes to policy or content scan job settings. |
|Network discovery |Network discovery features are unavailable for the classic scanner | Administrators can discover additional risky repositories by scanning a specified IP address or range.|
| | | |

For a detailed comparison of behavior differences for specific protection settings, see [Comparing the behavior of protection settings for a label](../configure-policy-migrate-labels.md#comparing-the-behavior-of-protection-settings-for-a-label).

### Features not planned to be in the Azure Information Protection unified labeling client

Although the Azure Information Protection unified labeling client is still under development, the following features and behavior differences from the classic client are not currently planned to be available in future releases for the unified labeling client: 

- Custom permissions as a [separate option that users can select in Office apps: Word, Excel, and PowerPoint](client-classify-protect.md#set-custom-permissions-for-a-document)

- Information Protection bar title and tooltip

- [Protection-only mode](client-protection-only-mode.md) (no labels) using templates

- Protect PDF document as [.ppdf (older format)](client-admin-guide-customizations.md#dont-protect-pdf-files-by-using-the-iso-standard-for-pdf-encryption)

- Display the **Do Not Forward** button in Outlook

- Demo policy

- Separate PowerShell cmdlets to connect to a Rights Management service

- Display of the user identity that applied a label


### Parent labels and their sublabels 

The Azure Information Protection client (classic) doesn't support configurations that specify a parent label that has sublabels. These configurations include specifying a default label, and a label for recommended or automatic classification. When a label has sublabels, you can specify one of the sublabels but not the parent label.

For parity, the Azure Information Protection unified labeling client also doesn't support applying parent labels that have sublabels, even though you can select these labels in the admin centers. In this scenario, the Azure Information Protection unified labeling client will not apply the parent label.

## Next steps

To install and configure the Azure Information Protection clients, use the following documentation:

- [Azure Information Protection client](AIP-client.md)

- [Azure Information Protection unified labeling client](unifiedlabelingclient-version-release-history.md)

For more information about using the built-in labeling client for Microsoft 365 apps, see [Sensitivity labels in Office apps](/microsoft-365/compliance/sensitivity-labels-office-apps).
