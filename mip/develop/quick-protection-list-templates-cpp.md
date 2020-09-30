---
title: Quickstart - List protection templates available to an authenticated user in a Microsoft Information Protection (MIP) tenant using C++ MIP SDK
description: A quickstart showing you how to use the Microsoft Information Protection C++ SDK Protection API to list the protection templates available to a user.
author: Pathak-Aniket
ms.service: information-protection
ms.topic: quickstart
ms.date: 01/18/2019
ms.author: v-anikep
ms.custom: has-adal-ref
#Customer intent: As a an application developer, I want to learn how to list protection templates for a user in the MIP SDK, so that I can use the SDK APIs to apply templates later on.
---

# Quickstart: List Protection Templates (C++)

This Quickstart shows you how to use the MIP Protection API, to protection templates available to the user.

## Prerequisites

If you haven't already, be sure to complete the following prerequisites before continuing:

- Complete [Quickstart - Client application initialization - Protection API (C++)](quick-protection-app-initialization-cpp.md) first, which builds a starter Visual Studio solution. This "List protection templates" Quickstart relies on the previous one, for proper creation of the starter solution.
- Optionally: Review [RMS Templates](/azure/information-protection/configure-policy-templates) concepts.

## Add logic to list the protection templates

Add logic to list protection templates available to a user, using the Protection engine object.

1. Open the Visual Studio solution you created in the previous "Quickstart - Client application initialization - Protection API (C++)" article.

2. Using **Solution Explorer**, open the .cpp file in your project that contains the implementation of the `main()` method. It defaults to the same name as the project containing it, which you specified during project creation.

3. Add the following `using` directive after `using mip::ProtectionEngine;`, near the top of the file:

   ```cpp
   using std::endl;
   ```

4. Toward the end of the `main()` body, below the closing brace `}` of the last `catch` block and above `return 0;` (where you left off in the previous Quickstart), insert the following code:

   ```cpp
    // List protection templates
    const shared_ptr<ProtectionEngineObserver> engineObserver = std::make_shared<ProtectionEngineObserver>();
    // Create a context to pass to 'ProtectionEngine::GetTemplateListAsync'. That context will be forwarded to the
    // corresponding ProtectionEngine::Observer methods. In this case, we use promises/futures as a simple way to detect
    // the async operation completes synchronously.
    auto loadPromise = std::make_shared<std::promise<vector<shared_ptr<mip::TemplateDescriptor>>>>();
    std::future<vector<shared_ptr<mip::TemplateDescriptor>>> loadFuture = loadPromise->get_future();
    engine->GetTemplatesAsync(engineObserver, loadPromise);
    auto templates = loadFuture.get();

    cout << "*** Template List: " << endl;

    for (const auto& protectionTemplate : templates) {
        cout << "Name: " << protectionTemplate->GetName() << " : " << protectionTemplate->GetId() << endl;
    }

   ```

## Create a PowerShell script to generate access tokens

Use the following PowerShell script to generate access tokens, which are requested by the SDK in your `AuthDelegateImpl::AcquireOAuth2Token` implementation. The script uses the `Get-ADALToken` cmdlet from the ADAL.PS module you installed earlier, in "MIP SDK Setup and configuration".

1. Create a PowerShell Script file (.ps1 extension), and copy/paste the following script into the file:

   - `$authority` and `$resourceUrl` are updated later, in the following section.
   - Update `$appId` and `$redirectUri`, to match the values you specified in your Azure AD app registration.

   ```powershell
   $authority = '<authority-url>'                   # Specified when SDK calls AcquireOAuth2Token()
   $resourceUrl = '<resource-url>'                  # Specified when SDK calls AcquireOAuth2Token()
   $appId = '<app-ID>'                              # App ID of the Azure AD app registration
   $redirectUri = '<redirect-uri>'                  # Redirect URI of the Azure AD app registration
   $response = Get-ADALToken -Resource $resourceUrl -ClientId $appId -RedirectUri $redirectUri -Authority $authority -PromptBehavior:RefreshSession
   $response.AccessToken | clip                     # Copy the access token text to the clipboard
   ```

2. Save the script file so you can run it later, when requested by your client application.

## Build and test the application

Finally, build and test your client application.

1. Use Ctrl+Shift+b (**Build Solution**) to build your client application. If you have no build errors, use F5 (**Start debugging**) to run your application.

2. If your project builds and runs successfully, the application prompts for an access token, each time the SDK calls your `AcquireOAuth2Token()` method. You can reuse a previously generated token, if prompted multiple times and the requested values are the same:

   ```console
   Run the PowerShell script to generate an access token using the following values, then copy/paste it below:
   Set $authority to: https://login.windows.net/common/oauth2/authorize
   Set $resourceUrl to: https://syncservice.o365syncservice.com/
   Sign in with user account: user1@tenant.onmicrosoft.com
   Enter access token:
   ```

3. To generate an access token for the prompt, go back to your PowerShell script and:

   - Update the `$authority` and `$resourceUrl` variables. They must match the values that are specified in the console output in step #2. These values are provided by the MIP SDK in the `challenge` parameter of `AcquireOAuth2Token()`:
     - `$authority` should be `https://login.windows.net/common/oauth2/authorize`
     - `$resourceUrl` should be `https://syncservice.o365syncservice.com/` or `https://aadrm.com`
   - Run the PowerShell script. The `Get-ADALToken` cmdlet triggers an Azure AD authentication prompt, similar to the example below. Specify the same account provided in the console output in step #2. After successful sign-in, the access token will be placed on the clipboard.

     [![Visual Studio acquire token sign-in](media/quick-file-list-labels-cpp/acquire-token-sign-in.png)](media/quick-file-list-labels-cpp/acquire-token-sign-in.png#lightbox)

   - You may also need to give consent, to allow the application to access the MIP APIs, while running under the sign-in account. This happens when the Azure AD application registration isn't pre-consented (as outlined in "MIP SDK setup and configuration"), or you're signing in with an account from a different tenant (other than the one where your application is registered). Simply click **Accept** to record your consent.

     [![Visual Studio consent](media/quick-file-list-labels-cpp/acquire-token-sign-in-consent.png)](media/quick-file-list-labels-cpp/acquire-token-sign-in-consent.png#lightbox)

4. After pasting the access token into the prompt from step #2, your console output should show the protection templates , similar to the following example:

   ```console
   *** Template List:
   Name: Confidential \ All Employees : a74f5027-f3e3-4c55-abcd-74c2ee41b607
   Name: Highly Confidential \ All Employees : bb7ed207-046a-4caf-9826-647cff56b990
   Name: Confidential : 174bc02a-6e22-4cf2-9309-cb3d47142b05
   Name: Contoso Employees Only : 667466bf-a01b-4b0a-8bbf-a79a3d96f720

   C:\MIP Sample Apps\ProtectionQS\Debug\ProtectionQS.exe (process 8252) exited with code 0.
   To automatically close the console when debugging stops, enable Tools->Options->Debugging->Automatically close the console when debugging stops.

   Press any key to continue . . .
   ```

   > [!NOTE]
   > Copy and save the ID of one or more of the protection templates (for example, `f42a3342-8706-4288-bd31-ebb85995028z`), as you will use it in the next Quickstart.

## Troubleshooting

### Problems during execution of PowerShell script

| Summary | Error message | Solution |
|---------|---------------|----------|
| Incorrect redirect URI in application registration or PowerShell script (AADSTS50011) |*AADSTS50011: The reply url specified in the request does not match the reply urls configured for the application: 'ac6348d6-0d2f-4786-af33-07ad46e69bfc'.* | Verify the redirect URI being used, by completing one of the following steps:<br><br><li>Update the Redirect URI in your Azure AD application configuration, to match your PowerShell script. See [MIP SDK setup and configuration](setup-configure-mip.md#register-a-client-application-with-azure-active-directory) to verify that you've correctly configured the Redirect URI property.<br><li>Update the `redirectUri` variable in your PowerShell script, to match your application registration. |
| Incorrect sign-in account (AADSTS50020) | *AADSTS50020: User account 'user@domain.com' from identity provider 'https://sts.windows.net/72f988bl-86f1-41af-91ab-2d7cd011db47/' does not exist in tenant 'Organization name' and cannot access the application '0edbblll-8773-44de-b87c-b8c6276d41eb' in that tenant.* | Complete one of the following steps:<br><br><li>Rerun the PowerShell script, but be sure to use an account from the same tenant where your Azure AD application is registered.<br><li>If your sign-in account was correct, your PowerShell host session may already be authenticated under a different account. In this case, exit the script host then reopen, then try running it again.<br><li>If you're using this Quickstart with a web app (instead of native), and need to sign in using an account from a different tenant, be sure your Azure AD application registration is enabled for multi-tenant use. You can verify by using the "edit Manifest" feature in the application registration, and ensure it specifies `"availableToOtherTenants": true,`. |
| Incorrect permissions in application registration (AADSTS65005) | *AADSTS65005: Invalid resource. The client has requested access to a resource, which is not listed in the requested permissions in the client's application registration. Client app ID: 0edbblll-8773-44de-b87c-b8c6276d41eb. Resource value from request: https://syncservice.o365syncservice.com/. Resource app ID: 870c4f2e-85b6-4d43-bdda-6ed9a579b725. List of valid resources from app registration: 00000002-0000-0000-c000-000000000000.* | Update the permission requests in your Azure AD application configuration. See [MIP SDK setup and configuration](setup-configure-mip.md#register-a-client-application-with-azure-active-directory) to verify that you've correctly configured the permission requests in your application registration. |

### Problems during execution of C++ application

| Summary | Error message | Solution |
|---------|---------------|----------|
| Bad access token | *An exception occurred... is the access token incorrect/expired?<br><br>Failed API call: profile_add_engine_async Failed with: [class mip::PolicySyncException] Failed acquiring policy, Request failed with http status code: 401, x-ms-diagnostics: [2000001;reason="OAuth token submitted with the request cannot be parsed.";error_category="invalid_token"], correlationId:[35bc0023-3727-4eff-8062-000006d5d672]'<br><br>C:\VSProjects\MipDev\Quickstarts\AppInitialization\x64\Debug\AppInitialization.exe (process 29924) exited with code 0.<br><br>Press any key to close this window . . .* | If your project builds successfully, but you see output similar to the left, you likely have an invalid or expired token in your `AcquireOAuth2Token()` method. Go back to [Create a PowerShell script to generate access tokens](#create-a-powershell-script-to-generate-access-tokens) and regenerate the access token, update `AcquireOAuth2Token()` again, and rebuild/retest. You can also examine and verify the token and its claims, using the [jwt.ms](https://jwt.ms/) single-page web application. |

## Next Steps

Now that you've learned how to list the protection templates available to an authenticated user, try the next quickstart:

> [!div class="nextstepaction"]
> [Encrypt and Decrypt text](quick-protection-encrypt-decrypt text-cpp.md)