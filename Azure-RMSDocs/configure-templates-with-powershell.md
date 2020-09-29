---
# required metadata

title: PowerShell for protection templates - Azure Information Protection
description: Use PowerShell cmdlets to add, get, export, import, remove, and configure protection templates for Azure Information Protection.
author: mlottner
ms.author: mlottner
manager: rkarlin
ms.date: 11/03/2019
ms.topic: how-to
ms.collection: M365-security-compliance
ms.service: information-protection
ms.assetid: 30ee2f77-ce16-4113-bcda-6089131849ec

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.subservice: azurerms
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: admin

---



# PowerShell reference for protection templates

>*Applies to: [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection), [Office 365](https://download.microsoft.com/download/E/C/F/ECF42E71-4EC0-48FF-AA00-577AC14D5B5C/Azure_Information_Protection_licensing_datasheet_EN-US.pdf)*

Protection settings for Azure Information Protection are saved in protection templates. Everything that you can do in the Azure portal to create and manage protection settings, you can do from the command line by using PowerShell. 

In addition, you can export and import protection templates. These two actions let you copy protection templates between tenants or do bulk edits of complex properties, such as multilingual names and descriptions.

You can also use export and import to back up and restore your protection templates. As a best practice, regularly back up your templates. Then, if you make a change to the protection settings that wasn't intended, you can easily revert to a previous version.

For installation instructions, see [Installing the AIPService PowerShell module](install-powershell.md).

The cmdlets that support creating and managing protection templates:

- [Add-AipServiceTemplate](/powershell/module/aipservice/add-aipservicetemplate)

- [Export-AipServiceTemplate](/powershell/module/aipservice/export-aipservicetemplate)

- [Get-AipServiceTemplate](/powershell/module/aipservice/get-aipservicetemplate)

- [Get-AipServiceTemplateProperty](/powershell/module/aipservice/get-aipservicetemplateproperty)

- [Import-AipServiceTemplate](/powershell/module/aipservice/import-aipservicetpd)

- [New-AipServiceRightsDefinition](/powershell/module/aipservice/new-aipservicerightsdefinition)

- [Remove-AipServiceTemplate](/powershell/module/aipservice/remove-aipservicetemplate)

- [Set-AipServiceTemplateProperty](/powershell/module/aipservice/set-aipservicetemplateproperty)

## See Also
[Configuring and managing templates for Azure Information Protection](configure-policy-templates.md)

