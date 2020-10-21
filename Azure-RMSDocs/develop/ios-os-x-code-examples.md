---
# required metadata

title: iOS/OS X code examples | Azure RMS
description: This topic will introduce you to important code elements for the iOS/OS X version of the RMS SDK.
keywords:
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 02/23/2017
ms.topic: conceptual
ms.collection: M365-security-compliance
ms.service: information-protection
ms.assetid: 7E12EBF2-5A19-4A8D-AA99-531B09DA256A
# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: shubhamp
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: dev, has-adal-ref

---

# iOS/OS X code examples

[!INCLUDE [deprecation notice](../includes/deprecation-warning.md)]

This topic will introduce you to important code elements for the iOS/OS X version of the RMS SDK.

**Note**  In the example code and descriptions that follow, we use the term MSIPC (Microsoft Information Protection and Control) to reference the client process.



## Using the Microsoft Rights Management SDK 4.2 - key scenarios


Following are **Objective C** code examples from a larger sample application representing development scenarios important to your orientation to this SDK. These demonstrate; use of Microsoft Protected File format referred to as protected file , use of custom protected file formats, and use of custom UI controls.

### Scenario: Consume an RMS protected file


- **Step 1**: Create an [MSProtectedData](/previous-versions/windows/desktop/msipcthin2/msprotecteddata-interface-objc) object

  **Description**: Instantiate an [MSProtectedData](/previous-versions/windows/desktop/msipcthin2/msprotecteddata-interface-objc) object, through its create method which implements service authentication using the [MSAuthenticationCallback](/previous-versions/windows/desktop/msipcthin2/msauthenticationcallback-protocol-objc) to get a token by passing an instance of **MSAuthenticationCallback**, as the parameter *authenticationCallback*, to the MSIPC API. See the call to [MSProtectedData protectedDataWithProtectedFile](/previous-versions/windows/desktop/msipcthin2/msprotecteddata-protecteddatawithprotectedfile-completionblock-method-objc) in the following example code section.

    ```objectivec
        + (void)consumePtxtFile:(NSString *)path authenticationCallback:(id<MSAuthenticationCallback>)authenticationCallback
        {
            // userId can be provided as a hint for authentication
            [MSProtectedData protectedDataWithProtectedFile:path
                                                 userId:nil
                                 authenticationCallback:authenticationCallback
                                                options:Default
                                        completionBlock:^(MSProtectedData *data, NSError *error)
            {
                //Read the content from the ProtectedData, this will decrypt the data
                NSData *content = [data retrieveData];
            }];
        }
    ```

- **Step 2**: Setup authentication using the Active Directory Authentication Library (ADAL).

  **Description**: In this step you will see ADAL used to implement an [MSAuthenticationCallback](/previous-versions/windows/desktop/msipcthin2/msauthenticationcallback-protocol-objc) with example authentication parameters. For more information on using ADAL, see the Azure AD Authentication Library (ADAL).

    ```objectivec
      // AuthenticationCallback holds the necessary information to retrieve an access token.
      @interface MsipcAuthenticationCallback : NSObject<MSAuthenticationCallback>

      @end

      @implementation MsipcAuthenticationCallback

      - (void)accessTokenWithAuthenticationParameters:
             (MSAuthenticationParameters *)authenticationParameters
                                    completionBlock:
             (void(^)(NSString *accessToken, NSError *error))completionBlock
      {
          ADAuthenticationError *error;
          ADAuthenticationContext* context = [
              ADAuthenticationContext authenticationContextWithAuthority:authenticationParameters.authority
                                                                error:&error
          ];
          NSString *appClientId = @"com.microsoft.sampleapp";
          NSURL *redirectURI = [NSURL URLWithString:@"local://authorize"];
          // Retrieve token using ADAL
          [context acquireTokenWithResource:authenticationParameters.resource
                                 clientId:appClientId
                              redirectUri:redirectURI
                                   userId:authenticationParameters.userId
                          completionBlock:^(ADAuthenticationResult *result) {
                              if (result.status != AD_SUCCEEDED)
                              {
                                  NSLog(@"Auth Failed");
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  completionBlock(result.accessToken, result.error);
                              }
                          }];
       }
    ```

- **Step 3**: Check if the Edit right exists for this user with this content via the [MSUserPolicy accessCheck](/previous-versions/windows/desktop/msipcthin2/msuserpolicy-accesscheck-method-objc) method of a [MSUserPolicy](/previous-versions/windows/desktop/msipcthin2/msuserpolicy-interface-objc) object.

    ```objectivec
      - (void)accessCheckWithProtectedData:(MSProtectedData *)protectedData
      {
          //check if user has edit rights and apply enforcements
          if (!protectedData.userPolicy.accessCheck(EditableDocumentRights.Edit))
          {
              // enforce on the UI
              textEditor.focusableInTouchMode = NO;
              textEditor.focusable = NO;
              textEditor.enabled = NO;
          }
      }
    ```

### Scenario: Create a new protected file using a template

This scenario begins with getting a list of templates, [MSTemplateDescriptor](/previous-versions/windows/desktop/msipcthin2/mstemplatedescriptor-interface-objc), selecting the first one to create a policy, then creating and writing to the new protected file.

-   **Step 1**: Get list of templates

    ```objectivec
        + (void)templateListUsageWithAuthenticationCallback:(id<MSAuthenticationCallback>)authenticationCallback
        {
            [MSTemplateDescriptor templateListWithUserId:@"user@domain.com"
                            authenticationCallback:authenticationCallback
                                   completionBlock:^(NSArray/*MSTemplateDescriptor*/ *templates, NSError *error)
                                   {
                                     // use templates array of MSTemplateDescriptor (Note: will be nil on error)
                                   }];
        }
    ```

-   **Step 2**: Create a [MSUserPolicy](/previous-versions/windows/desktop/msipcthin2/msuserpolicy-interface-objc) using the first template in the list.

    ```objectivec
        + (void)userPolicyCreationFromTemplateWithAuthenticationCallback:(id<MSAuthenticationCallback>)authenticationCallback
        {
            [MSUserPolicy userPolicyWithTemplateDescriptor:[templates objectAtIndex:0]
                                            userId:@"user@domain.com"
                                     signedAppData:nil
                            authenticationCallback:authenticationCallback
                                           options:None
                                   completionBlock:^(MSUserPolicy *userPolicy, NSError *error)
            {
            // use userPolicy (Note: will be nil on error)
            }];
        }
    ```

-   **Step 3**: Create a [MSMutableProtectedData](/previous-versions/windows/desktop/msipcthin2/msmutableprotecteddata-interface-objc) and write content to it.

    ```objectivec
        + (void)createPtxtWithUserPolicy:(MSUserPolicy *)userPolicy contentToProtect:(NSData *)contentToProtect
        {
            // create an MSMutableProtectedData to write content
            [contentToProtect protectedDataInFile:filePath
                        originalFileExtension:kDefaultTextFileExtension
                               withUserPolicy:userPolicy
                              completionBlock:^(MSMutableProtectedData *data, NSError *error)
            {
             // use data (Note: will be nil on error)
            }];
        }
    ```

### Scenario: Open a custom protected file


-   **Step 1**: Create a [MSUserPolicy](/previous-versions/windows/desktop/msipcthin2/msuserpolicy-interface-objc) from a *serializedContentPolicy*.

    ```objectivec
        + (void)userPolicyWith:(NSData *)protectedData
        authenticationCallback:(id<MSAuthenticationCallback>)authenticationCallback
        {
            // Read header information from protectedData and extract the  PL
            /*-------------------------------------------
            | PL length | PL | ContetSizeLength |
            -------------------------------------------*/
            NSUInteger serializedPolicySize;
            NSMutableData *serializedPolicy;
            [protectedData getBytes:&serializedPolicySize length:sizeof(serializedPolicySize)];
            [protectedData getBytes:[serializedPolicy mutableBytes] length:serializedPolicySize];

            // Get the user policy , this is an async method as it hits the REST service
            // for content key and usage restrictions
            // userId provided as a hint for authentication
            [MSUserPolicy userPolicyWithSerializedPolicy:serializedPolicy
                                              userId:@"user@domain.com"
                              authenticationCallback:authenticationCallback
                                             options:Default
                                     completionBlock:^(MSUserPolicy *userPolicy,
                                                       NSError *error)
            {

            }];
         }
    ```

-   **Step 2**: Create a [MSCustomProtectedData](/previous-versions/windows/desktop/msipcthin2/msmutablecustomprotecteddata-interface-objc) using the [MSUserPolicy](/previous-versions/windows/desktop/msipcthin2/msuserpolicy-interface-objc) from **Step 1** and read from it.

    ```objectivec
        + (void)customProtectedDataWith:(NSData *)protectedData
        {
            // Read header information from protectedData and extract the  protectedContentSize
            /*-------------------------------------------
            | PL length | PL | ContetSizeLength |
            -------------------------------------------*/
            NSUInteger protectedContentSize;
            [protectedData getBytes:&protectedContentSize
                         length:sizeof(protectedContentSize)];

            // Create the MSCustomProtector used for decrypting the content
            // The content start position is the header length
            [MSCustomProtectedData customProtectedDataWithPolicy:userPolicy
                                               protectedData:protectedData
                                        contentStartPosition:sizeof(NSUInteger) + serializedPolicySize
                                                 contentSize:protectedContentSize
                                             completionBlock:^(MSCustomProtectedData *customProtector,
                                                               NSError *error)
            {
             //Read the content from the custom protector, this will decrypt the data
             NSData *content = [customProtector retrieveData];
             NSLog(@"%@", content);
            }];
         }
    ```

### Scenario: Create a custom protected file using a custom (ad-hoc) policy


-   **Step 1**: With an email address provided by the user, create a policy descriptor.

    **Description**: In practice the following objects would be created by using user inputs from the device interface; [MSUserRights](/previous-versions/windows/desktop/msipcthin2/msuserrights-interface-objc) and [MSPolicyDescriptor](/previous-versions/windows/desktop/msipcthin2/mspolicydescriptor-interface-objc).

    ```objectivec
        + (void)policyDescriptor
        {
            MSUserRights *userRights = [[MSUserRights alloc] initWithUsers:[NSArray arrayWithObjects: @"user1@domain.com", @"user2@domain.com", nil] rights:[MSEmailRights all]];

            MSPolicyDescriptor *policyDescriptor = [[MSPolicyDescriptor alloc] initWithUserRights:[NSArray arrayWithObjects:userRights, nil]];
            policyDescriptor.contentValidUntil = [[NSDate alloc] initWithTimeIntervalSinceNow:NSTimeIntervalSince1970 + 3600.0];
            policyDescriptor.offlineCacheLifetimeInDays = 10;
        }
    ```

-   **Step 2**: Create a custom [MSUserPolicy](/previous-versions/windows/desktop/msipcthin2/msuserpolicy-interface-objc) from the policy descriptor, *selectedDescriptor*.

    ```objectivec
        + (void)userPolicyWithPolicyDescriptor:(MSPolicyDescriptor *)policyDescriptor
        {
            [MSUserPolicy userPolicyWithPolicyDescriptor:policyDescriptor
                                          userId:@"user@domain.com"
                          authenticationCallback:authenticationCallback
                                         options:None
                                 completionBlock:^(MSUserPolicy *userPolicy, NSError *error)
            {
              // use userPolicy (Note: will be nil on error)
            }];
        }
    ```

-   **Step 3**: Create and write content to the [MSMutableCustomProtectedData](/previous-versions/windows/desktop/msipcthin2/msmutablecustomprotecteddata-interface-objc) and then close.

    ```objectivec
        + (void)mutableCustomProtectedData:(NSMutableData *)backingData policy:(MSUserPolicy *)policy contentToProtect:(NSString *)contentToProtect
        {
            //Get the serializedPolicy from a given policy
            NSData *serializedPolicy = [policy serializedPolicy];

            // Write header information to backing data including the PL
            // ------------------------------------
            // | PL length | PL | ContetSizeLength |
            // -------------------------------------
            NSUInteger serializedPolicyLength = [serializedPolicy length];
            [backingData appendData:[NSData dataWithBytes:&serializedPolicyLength length:sizeof(serializedPolicyLength)]];
            [backingData appendData:serializedPolicy];
            NSUInteger protectedContentLength = [MSCustomProtectedData getEncryptedContentLengthWithPolicy:policy contentLength:unprotectedData.length];
            [backingData appendData:[NSData dataWithBytes:&protectedContentLength length:sizeof(protectedContentLength)]];

            NSUInteger headerLength = sizeof(serializedPolicyLength) + serializedPolicyLength + sizeof(protectedContentLength);

            // Create the MSMutableCustomProtector used for encrypting content
            // The content start position is the current length of the backing data
            // The encryptedContentSize content size is 0 since there is no content yet
            [MSMutableCustomProtectedData customProtectorWithUserPolicy:policy
                                                        backingData:backingData
                                             protectedContentOffset:headerLength
                                                    completionBlock:^(MSMutableCustomProtectedData *customProtector,
                                                                      NSError *error)
            {
                //Append data to the custom protector, this will encrypt the data and write it to the backing data
                [customProtector appendData:[contentToProtect dataUsingEncoding:NSUTF8StringEncoding] error:&error];

                //close the custom protector so it will flush and finalise encryption
                [customProtector close:&error];

            }];
          }
    ```