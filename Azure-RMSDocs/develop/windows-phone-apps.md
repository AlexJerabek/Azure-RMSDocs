---
# required metadata

title: Windows Phone setup | Azure RMS
description: Windows Phone applications can use the Microsoft Rights Management SDK 4.2 to enable integrated information protection in their application.
keywords:
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 12/10/2018
ms.topic: how-to
ms.collection: M365-security-compliance
ms.service: information-protection
ms.assetid: e25a446e-b977-4736-9c65-7711171fb0e1
# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: shubhamp
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: dev

---

# Windows Phone setup

[!INCLUDE [deprecation notice](../includes/deprecation-warning.md)]

Windows Phone applications can use the Microsoft Rights Management SDK 4.2 to enable integrated information protection in their application by using the Azure Active Directory Rights Management (AAD RM).

This topic will guide you through setting up your environment for creating your own new apps .

-   [Prerequisites](#prerequisites)
-   [Configuring your development environment](#configuring-your-development-environment)
-   [See Also](#see-also)

## Prerequisites


You must have the following software on your development system:

-   The [Windows 8.1](https://windows.microsoft.com/windows-8/meet) operating system.
-   [Windows Phone 8.1 Development Tools (SDK)](https://developer.microsoft.com/windows/downloads/sdk-archive)
-   Microsoft [Visual Studio 2012](https://visualstudio.microsoft.com/vs/older-downloads/)or above, or Visual Studio Express 2012, which is included in the Windows Phone SDK 8.0/8.1
-   The MS RMS SDK 4.2 package for Windows Phone. For more information see, [Get started](get-started.md).
-   Authentication library: We recommend that you use the [Azure AD Authentication Library](/previous-versions/azure/jj573266(v=azure.100)) and other authentication libraries can be used.

Read the [What's new](release-notes.md) topic for information on API updates, device and environment information, release notes and frequently asked questions (FAQ).

Review the information in the [Windows Phone development](/previous-versions/windows/apps/ff402535(v=vs.105)) guide at the Windows Phone Dev Center.

## Configuring your development environment


-   Open *Visual Studio*.
-   Click **File**. On the **File** menu, click **New**, and then click **Project**.
-   In the **New Project** dialog box, select **Visual C\#**, select **Blank App (Windows Phone)**, and then click **OK**.

    ![Create new project](../media/wpsetup-newproj.png)

-   In Solution Explorer, right-click your project, and then select **Add Reference** to open the **Add Reference** dialog box.

    ![Add reference](../media/wpsetup-addref.png)

-   Click **Browse** on the lower left of the **Add Reference** dialog box and select the *Microsoft.RightsManagment.dll* file that is located in the folder you extracted the package in.
-   **Managed Apps** - For building a managed app, you will have to add this reference; select **Windows 8.1**-&gt;**Extensions** and check the box for **Windows Visual C++ Runtime Package for Windows**

    ![Add extensions](../media/wpsetup-refmngr.png)

-   **Adding Capabilities** - Your application will need the "Internet (Client & Server)" capability to use the SDK. To add this capability to your app, open the *Package.appxmanifest* file in the project and navigate to the **Capabilities** tab to add.

You are now ready to create your own new Windows Phone apps.

### See Also

[Get started](get-started.md)

[What's new](release-notes.md)

[Core concepts](core-concepts.md)

[Windows Phone development](/previous-versions/windows/apps/ff402535(v=vs.105))

[Windows API Reference](/previous-versions/windows/desktop/msipcthin2/winrt)

[Visual Studio 2012](https://visualstudio.microsoft.com/vs/older-downloads/)

[Windows Phone SDK](https://developer.microsoft.com/windows/downloads/sdk-archive)