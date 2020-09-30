---
# required metadata

title: Use PowerShell with the Azure Information Protection unified labeling client
description: Instructions and information for admins to manage the Azure Information Protection unified labeling client by using PowerShell.
author: mlottner
ms.author: mlottner
manager: rkarlin
ms.date: 09/03/2020
ms.topic: how-to
ms.collection: M365-security-compliance
ms.service: information-protection

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.subservice: v2client
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: admin

---


# Admin Guide: Using PowerShell with the Azure Information Protection unified client

>*Applies to: [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection), Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012*
>
>*If you have Windows 7 or Office 2010, see [AIP for Windows and Office versions in extended support](../known-issues.md#aip-for-windows-and-office-versions-in-extended-support).*
>
> *Instructions for: [Azure Information Protection unified labeling client for Windows](../faqs.md#whats-the-difference-between-the-azure-information-protection-classic-and-unified-labeling-clients)*

When you install the Azure Information Protection unified labeling client, PowerShell commands are automatically installed. This lets you manage the client by running commands that you can put into scripts for automation.

The cmdlets are installed with the PowerShell module **AzureInformationProtection**, which has cmdlets for labeling. For example:

|Labeling cmdlet|Example usage|
|----------------|---------------|
|[Get-AIPFileStatus](/powershell/module/azureinformationprotection/get-aipfilestatus)|For a shared folder, identify all files with a specific label.|
|[Set-AIPFileClassification](/powershell/module/azureinformationprotection/set-aipfileclassification)|For a shared folder, inspect the file contents and then automatically label unlabeled files, according to the conditions that you have specified.|
|[Set-AIPFileLabel](/powershell/module/azureinformationprotection/set-aipfilelabel)|For a shared folder, apply a specified label to all files that do not have a label.|
|[Set-AIPAuthentication](/powershell/module/azureinformationprotection/set-aipauthentication)|Label files non-interactively, for example by using a script that runs on a schedule.|

This module installs in **\ProgramFiles (x86)\Microsoft Azure Information Protection** and adds this folder to the **PSModulePath** system variable. The .dll for this module is named **AIP.dll**.

> [!IMPORTANT]
> The AzureInformationProtection module doesn't support configuring advanced settings for labels or label policies. For these settings, you need the Office 365 Security & Compliance Center PowerShell. For more information, see [Custom configurations for the Azure Information Protection unified labeling client](clientv2-admin-guide-customizations.md).
> [!NOTE]
> If you've migrated from Azure RMS, note that RMS-related cmdlets have been deprecated for use in unified labeling. Some of these have been replaced with new cmdlets for unified labeling. For more information, see [RMS to unified labeling cmdlet mapping](#rms-to-unified-labeling-cmdlet-mapping).
>

> [!TIP]
> To use cmdlets with path lengths greater than 260 characters, use the following [group policy setting](/archive/blogs/jeremykuhne/net-4-6-2-and-long-paths-on-windows-10) that is available starting Windows 10, version 1607:<br /> **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **All Settings** > **Enable Win32 long paths** 
> 
> For Windows Server 2016, you can use the same group policy setting when you install the latest Administrative Templates (.admx) for Windows 10.
>
> For more information, see the [Maximum Path Length Limitation](/windows/desktop/FileIO/naming-a-file#maximum-path-length-limitation) section from the Windows 10 developer documentation.

### Prerequisites for using the AzureInformationProtection module

In addition to the prerequisites for installing the AzureInformationProtection module, there are additional prerequisites for when you use the labeling cmdlets for Azure Information Protection:

1. The Azure Rights Management service must be activated.

2. To remove protection from files for others using your own account: 

    - The super user feature must be enabled for your organization and your account must be configured to be a super user for Azure Rights Management.

#### Prerequisite 1: The Azure Rights Management service must be activated

If your Azure Information Protection tenant is not activated, see the instructions for [[Activating the protection service from Azure Information Protection](../activate-service.md).

#### Prerequisite 2: To remove protection from files for others using your own account

Typical scenarios for removing protection from files for others include data discovery or data recovery. If you are using labels to apply the protection, you could remove the protection by setting a new label that doesn't apply protection or by removing the label.

You must have a Rights Management usage right to remove protection from files, or be a super user. For data discovery or data recovery, the super user feature is typically used. To enable this feature and configure your account to be a super user, see [Configuring super users for Azure Information Protection and discovery services or data recovery](../configure-super-users.md).

## RMS to unified labeling cmdlet mapping

The following table maps RMS-related cmdlets with the updated cmdlets used for unified labeling.

For example, if you used **New-RMSProtectionLicense** with RMS protection and have migrated to unified labeling, use **New-AIPCustomPermissions** instead.

|RMS cmdlet  |Unified labeling cmdlet  |
|---------|---------|
|[Get-RMSFileStatus](/powershell/module/azureinformationprotection/get-rmsfilestatus)     |  [Get-AIPFileStatus](/powershell/module/azureinformationprotection/get-aipfilestatus)        |
|[Get-RMSServer](/powershell/module/azureinformationprotection/get-rmsserver)     |  Not relevant for unified labeling.      |
|[Get-RMSServerAuthentication](/powershell/module/azureinformationprotection/get-rmsserverauthentication)      |   [Set-AIPAuthentication](/powershell/module/azureinformationprotection/set-aipauthentication)       |
|[Clear-RMSAuthentication](/powershell/module/azureinformationprotection/clear-rmsauthentication)     | [Set-AIPAuthentication](/powershell/module/azureinformationprotection/set-aipauthentication)       |
|[Set-RMSServerAuthentication](/powershell/module/azureinformationprotection/set-rmsserverauthentication)     |  [Set-AIPAuthentication](/powershell/module/azureinformationprotection/set-aipauthentication)      |
|[Get-RMSTemplate](/powershell/module/azureinformationprotection/get-rmstemplate)     |       Not relevant for unified labeling  |
|[New-RMSProtectionLicense](/powershell/module/azureinformationprotection/new-rmsprotectionlicense)     |  [New-AIPCustomPermissions](/powershell/module/azureinformationprotection/new-aipcustompermissions), and [Set-AIPFileLabel](/powershell/module/azureinformationprotection/set-aipfilelabel), with the **CustomPermissions** parameter      |
|[Protect-RMSFile](/powershell/module/azureinformationprotection/protect-rmsfile) |[Set-AIPFileLabel](/powershell/module/azureinformationprotection/set-aipfilelabel), with the **RemoveProtection** parameter |
| | |


## How to label files non-interactively for Azure Information Protection

You can run the labeling cmdlets non-interactively by using the [Set-AIPAuthentication](/powershell/module/azureinformationprotection/set-aipauthentication) cmdlet.

By default, when you run the cmdlets for labeling, the commands run in your own user context in an interactive PowerShell session. To run them unattended, use a Windows account that can sign in interactively, and use an account in Azure AD that will be used for delegated access. For ease of administration, use a single account that's synchronized from Active Directory to Azure AD.

You also need to request an access token from Azure AD, which sets and stores credentials for the delegated user to authenticate to Azure Information Protection.

The computer running the AIPAuthentication cmdlet downloads the label policies with labels that are assigned to the delegated user account by using your labeling management center, such as the Office 365 Security & Compliance Center.

> [!NOTE]
> If you use label policies for different users, you might need to create a new label policy that publishes all your labels, and publish the policy to just this delegated user account.

When the token in Azure AD expires, you must run the cmdlet again to acquire a new token. You can configure the access token in Azure AD for one year, two years, or to never expire. The parameters for Set-AIPAuthentication use values from an app registration process in Azure AD, as described in the next section.

For the delegated user account:

- Make sure that you have a label policy assigned to this account and that the policy contains the published labels you want to use.

- If this account needs to decrypt content, for example, to reprotect files and inspect files that others have protected, make it a [super user](../configure-super-users.md) for Azure Information Protection and make sure the super user feature is enabled.

- If you have implemented [onboarding controls](../activate-service.md#configuring-onboarding-controls-for-a-phased-deployment) for a phased deployment, make sure that this account is included in your onboarding controls you've configured.

### To create and configure the Azure AD applications for Set-AIPAuthentication

> [!IMPORTANT]
> These instructions are for the current general availability version of the unified labeling client and also apply to the general availability version of the scanner for this client.

Set-AIPAuthentication requires an app registration for the *AppId* and *AppSecret* parameters. If you upgraded from a previous version of the client and created an app registration for the previous *WebAppId* and *NativeAppId* parameters, they won't work with the unified labeling client. You must create a new app registration as follows:

1. In a new browser window, sign in the [Azure portal](https://portal.azure.com/).

2. For the Azure AD tenant that you use with Azure Information Protection, navigate to **Azure Active Directory** > **Manage** > **App registrations**. 

3. Select **+ New registration**. On the **Register an application** pane, specify the following values, and then click **Register**:

   - **Name**: `AIP-DelegatedUser`
        
        If you prefer, specify a different name. It must be unique per tenant.
    
    - **Supported account types**: **Accounts in this organizational directory only**
    
    - **Redirect URI (optional)**: **Web** and `https://localhost`

4. On the **AIP-DelegatedUser** pane, copy the value for the **Application (client) ID**. The value looks similar to the following example: `77c3c1c3-abf9-404e-8b2b-4652836c8c66`. This value is used for the *AppId* parameter when you run the Set-AIPAuthentication cmdlet. Paste and save the value for later reference.

5. From the sidebar, select **Manage** > **Certificates & secrets**.

6. On the **AIP-DelegatedUser - Certificates & secrets** pane, in the **Client secrets** section, select **+ New client secret**.

7. For **Add a client secret**, specify the following, and then select **Add**:
    
    - **Description**: `Azure Information Protection unified labeling client`
    - **Expires**: Specify your choice of duration (1 year, 2 years, or never expires)

8. Back on the **AIP-DelegatedUser - Certificates & secrets** pane, in the **Client secrets** section, copy the string for the **VALUE**. This string looks similar to the following example: `OAkk+rnuYc/u+]ah2kNxVbtrDGbS47L4`. To make sure you copy all the characters, select the icon to **Copy to clipboard**. 
    
    It's important that you save this string because it is not displayed again and it cannot be retrieved. As with any sensitive information that you use, store the saved value securely and restrict access to it.

9. From the sidebar, select **Manage** > **API permissions**.

10. On the **AIP-DelegatedUser - API permissions** pane, select **+ Add a permission**.

11. On the **Request API permissions** pane, make sure that you're on the **Microsoft APIs** tab, and select **Azure Rights Management Services**. When you're prompted for the type of permissions that your application requires, select **Application permissions**.

12. For **Select permissions**, expand **Content** and select the following:
    
    -  **Content.DelegatedReader** 
    -  **Content.DelegatedWriter**

13. Select **Add permissions**.

14. Back on the **AIP-DelegatedUser - API permissions** pane, select **+ Add a permission** again.

15. On the **Request AIP permissions** pane, select **APIs my organization uses**, and search for **Microsoft Information Protection Sync Service**.

16. On the **Request API permissions** pane, select **Application permissions**.

17. For **Select permissions**, expand **UnifiedPolicy** and select the following:
    
    -  **UnifiedPolicy.Tenant.Read**

18. Select **Add permissions**.

19. Back on the **AIP-DelegatedUser - API permissions** pane, select **Grant admin consent for \<*your tenant name*>** and select **Yes** for the confirmation prompt.
    
    Your API permissions should look like the following:
    
    ![API permissions for the registered app in Azure AD](../media/api-permissions-app.png)

Now you've completed the registration of this app with a secret, you're ready to run [Set-AIPAuthentication](/powershell/module/azureinformationprotection/set-aipauthentication) with the parameters *AppId*, and *AppSecret*. Additionally, you'll need your tenant ID. 

> [!TIP]
>You can quickly copy your tenant ID by using Azure portal: **Azure Active Directory** > **Manage** > **Properties** > **Directory ID**.

1. Open Windows PowerShell with the **Run as administrator option**. 

2. In your PowerShell session, create a variable to store the credentials of the Windows user account that will run non-interactively. For example, if you created a service account for the scanner:

    ```ps
    $pscreds = Get-Credential "CONTOSO\srv-scanner"
    ```

    You're prompted for this account's password.

2. Run the Set-AIPAuthentication cmdlet, with the *OnBeHalfOf* parameter, specifying as its value the variable that you just created. Also specify your app registration values, your tenant ID, and the name of the delegated user account in Azure AD. For example:
    
    ```ps
    Set-AIPAuthentication -AppId "77c3c1c3-abf9-404e-8b2b-4652836c8c66" -AppSecret "OAkk+rnuYc/u+]ah2kNxVbtrDGbS47L4" -TenantId "9c11c87a-ac8b-46a3-8d5c-f4d0b72ee29a" -DelegatedUser scanner@contoso.com -OnBehalfOf $pscreds
    ```

> [!NOTE]
> If the computer cannot have internet access, there's no need to create the app in Azure AD and run Set-AIPAuthentication. Instead, follow the instructions for [disconnected computers](clientv2-admin-guide-customizations.md#support-for-disconnected-computers).  

## Next steps
For cmdlet help when you are in a PowerShell session, type `Get-Help <cmdlet name> -online`. For example: 

```ps
Get-Help Set-AIPFileLabel -online
```

See the following for additional information that you might need to support the Azure Information Protection client:

- [Customizations](clientv2-admin-guide-customizations.md)

- [Client files and usage logging](clientv2-admin-guide-files-and-logging.md)

- [File types supported](clientv2-admin-guide-file-types.md)