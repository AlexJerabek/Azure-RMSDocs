---
# required metadata

title: Known issues - Azure Information Protection
description: Search and browse through known issues and limitations for Azure Information Protection.
author: batamig
ms.author: bagol
manager: rkarlin
ms.date: 11/15/2020
ms.topic: reference
ms.collection: M365-security-compliance
ms.service: information-protection

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: admin

---

# Known issues - Azure Information Protection

Use the lists and tables below to find details about known issues and limitations related to Azure Information Protection features.

> [!NOTE]
> This article refers to known issues in both the classic and unified labeling clients. Not sure of the difference between these clients? See [FAQs](faqs.md#whats-the-difference-between-the-azure-information-protection-classic-and-unified-labeling-clients).

## Client support for container files, such as .zip files

Container files are files that include other files, with a typical example being .zip files that contain compressed files. Other examples include .rar, .7z, .msg files, and PDF documents that include attachments.

You can classify and protect these container files, but the classification and protection is not applied to each file inside the container.

If you have a container file that includes classified and protected files, you must first extract the files to change their classification or protection settings. However, you can remove the protection for all files in supported container files by using the [Unprotect-RMSFile](/powershell/module/azureinformationprotection/unprotect-rmsfile) cmdlet.

The Azure Information Protection viewer cannot open attachments in a protected PDF document. In this scenario, when the document is opened in the viewer, the attachments are not visible.

For more information, see [Admin Guide: File types supported by the Azure Information Protection client](rms-client/client-admin-guide-file-types.md).

## Known issues for AIP and Exploit Protection

The Azure Information Protection client is not supported on machines that have .NET 2 or 3, where [Exploit protection](/windows/security/threat-protection/microsoft-defender-atp/enable-exploit-protection) is enabled.

If you have a .NET version 2 or 3 in addition to a .NET 4.x version required for your system, make sure to disable Exploit protection before installing AIP. 

To disable Exploit protection via PowerShell, run the following:

```PowerShell
Set-ProcessMitigation -Name "OUTLOOK.EXE" -Disable EnableExportAddressFilterPlus, EnableExportAddressFilter, EnableImportAddressFilter
```

For more information, see [Additional prerequisites for the Azure Information Protection unified labeling client](rms-client/clientv2-admin-guide-install.md#additional-prerequisites-for-the-azure-information-protection-unified-labeling-client).

## PowerShell support for the Azure Information Protection client

The current release of the **AzureInformationProtection** PowerShell module that's installed with the Azure Information Protection client has the following known issues:

- **Outlook personal folders (*.pst* files)**. Natively protecting *.pst* files is not supported using the **AzureInformationProtection** module.

- **Outlook protected email messages *(.rpmsg* files)**. Unprotecting Outlook protected email messages is supported by the **AzureInformationProtection** module only if they are inside an Outlook personal folder *(.pst* file).

    Unprotecting email messages outside of a *.pst* file is not supported.

For more information, see [Admin Guide: Using PowerShell with the Azure Information Protection client](rms-client/client-admin-guide-powershell.md).

## AIP known issues in Office applications

|Feature  |Known issues  |
|---------|---------|
|**Multiple versions of Office**    | The Azure Information Protection clients, including both classic and unified labeling, do not support multiple versions of Office on the same computer, or switching user accounts in Office.       |
|**Multiple displays** |If you're using multiple displays and have an Office application open: <br><br>- You may experience performance issues in your Office apps.<br>- The Azure Information Protection bar may appear to float in the middle of the Office screen, on one or both displays <br><br>To ensure consistent performance, and that the bar remains in the correct location, open the **Options** dialog for your Office application, and under **General,** select **Optimize for compatibility** instead of **Optimize for best appearance.**    |
|**IRM support in Office 2016**| The [DRMEncryptProperty](/deployoffice/security/protect-sensitive-messages-and-documents-by-using-irm-in-office#office-2016-irm-registry-key-options) registry setting, which controls metadata encryption in Office 2016, is not supported for Azure Information Protection labels.|
|**Content markings in Word**    | AIP [content markings](configure-policy-markings.md) in Microsoft Word headers or footers may be offset or placed incorrectly, or may be hidden entirely, when that same header or footer also contains a table.<br><br>For more information, see [When visual markings are applied](configure-policy-markings.md#when-visual-markings-are-applied). |
|**Files attached to emails** |Due to a limitation in recent Windows updates, when [Microsoft Outlook is protected by Azure Rights Management](office-apps-services-support.md), files attached to emails may be locked after opening the file. |
|**Mail merge**    |  The Office [mail merge](https://support.office.com/article/use-mail-merge-for-bulk-email-letters-labels-and-envelopes-f488ed5b-b849-4c11-9cff-932c49474705) feature is not supported with any Azure Information Protection feature.       |
| **S/MIME emails** | Opening S/MIME emails in Outlook's Reading Pane may cause performance issues. <br><br>To prevent performance issues with S/MIME emails, enable the [**OutlookSkipSmimeOnReadingPaneEnabled**](rms-client/clientv2-admin-guide-customizations.md#prevent-outlook-performance-issues-with-smime-emails) advanced property. <br><br>**Note:** Enabling this property prevents the AIP bar or the email classification from being displayed in Outlook's Reading Pane. |
|**Send to File Explorer option** |If you choose to right-click on any file in the File Explorer and select **Send to > Mail recipient**, the Outlook message that opens with the file attached may not display the AIP toolbar. <br><br>If this occurs and you need to use the AIP toolbar options, start your email from within Outlook and then browse to and attach the file you want to send.|
| | |

## Known issues in policies

Publishing policies may take up to 24 hours.

## Known issues in the AIP client

- **Maximum file sizes. Files** of over 2 GB are supported for protection, but not decryption.

- **AIP viewer.** The AIP viewer displays images in portrait mode, and some wide, landscape-view images may appear to be stretched.

    For example, an original image is shown below on the left, with a stretched, portrait version in the AIP viewer on the right. 
    
    :::image type="content" source="media/client-viewer-stretched-images.PNG" alt-text="Stretched image in client viewer":::
    
    For more information, see:

    - [**Unified labeling client**: View protected files with the Azure Information Protection viewer](rms-client/clientv2-view-use-files.md)
    - [**Classic client**: View protected files with the Azure Information Protection viewer](rms-client/client-view-use-files.md)


## AIP for Windows and Office versions in extended support

- [**Windows 7 extended supported ended on January 14, 2020**](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet). 

    We strongly encourage you to upgrade to a newer version of Windows 10. However, if you have Extended Security Updates (ESU) and a support contract, AIP support is available to continue keeping your Windows 7 systems secure.

    For more information, check with your support contact.

- [**Office 2010 is currently in extended support**](https://support.microsoft.com/lifecycle/search?alpha=office%202010). 

    This support will end on Oct. 13, 2020, and will not be extended. Additionally, ESU will not be offered for Office 2010, and we strongly encourage you to upgrade to a newer version of Office 365. 
    
    For customers who are currently running Office 2010 in extended support, AIP support is available until October 13, 2020. 

    For more information, check with your support contact.

## AIP-based Conditional Access policies

External users who receive content protected by [Conditional Access policies](/azure/active-directory/conditional-access/concept-conditional-access-policy-common) must have an Azure Active Directory (Azure AD) business-to-business (B2B) collaboration guest user account in order to view the content.

While you can invite external users to activate a guest user account, allowing them to authenticate and pass the conditional access requirements, it may be difficult to ensure that this occurs for all external users required.

We recommend enabling AIP-based conditional access policies for your internal users only.

**Enable conditional access policies for AIP for internal users only:**

1.	In the Azure portal, navigate to the **Conditional Access** blade, and select the conditional access policy you wish to modify. 
2.	Under **Assignments**, select **Users and groups**, and then select **All users**. Make sure that the **All guest and external users** option is *not* selected.
3.	Save your changes. 
 
You can also entirely disable CA within Azure Information Protection if the functionality is not required for your organization, in order to avoid this potential issue. 

For more information, see the [Conditional Access documentation](/azure/active-directory/conditional-access/concept-conditional-access-users-groups).

## More information

The following additional articles may be helpful in answering questions about known issues in Azure Information Protection:

- [Frequently asked questions for Azure Information Protection](faqs.md)
- [Frequently asked questions about data protection in Azure Information Protection](faqs-rms.md)
- [Frequently asked questions about classification and labeling in Azure Information Protection](faqs-infoprotect.md)
- [FAQs for Microsoft Azure Information Protection app for iOS and Android](rms-client/mobile-app-faq.md)