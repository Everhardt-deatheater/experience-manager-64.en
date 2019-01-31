---
title: Set up the Xcode project and build the iOS app
seo-title: Set up the Xcode project and build the iOS app
description: null
seo-description: Explains how to build standard AEM Forms app for iOS.
uuid: 3eb0cdfc-af3c-42f1-896f-43f76277378f
acrolinxdate: 2016-05-31T06 50 03.547-0400
acrolinxlastcheckedby: vishgupt
acrolinxpagestatus: red
acrolinxreporturl: http //checkstyle.corp.adobe.com 8031/output/en/setup_xcode_project_build_installer_admin_5e12de0b318c6865_2155_report.xml
acrolinxsentences: 60
acrolinxwords: 767
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: forms-app
discoiquuid: ac08d794-4fb6-4272-b432-b41c15f6a3d4
firstpublishqadate: 2013-02-20T08 59 32.673-0500
isreadyforlocalization: false
lastpublishqadate: 2015-11-20T08 18 44.006-0500
publishqadate: 2015-11-20T08 18 44.006-0500
publishqaurl: https //helpx.stage.adobe.com/aem-forms/6-1/mobile-workspace/setup-xcode-project-build-installer.html
index: y
internal: n
snippet: y
---

# Set up the Xcode project and build the iOS app{#set-up-the-xcode-project-and-build-the-ios-app}

AEM Forms provides the complete source code of the AEM Forms app. The source contains all components to build custom AEM Forms app. The source code archive, `adobe-lc-mobileworkspace-src-<version>.zip` is a part of the `adobe-aemfd-forms-app-src-pkg-<version>.zip` package on package share.

To get the AEM Forms app source, perform the following steps:

1. Navigate to package share  
   URL: `http://<server>:<port>/crx/packageshare`.

1. Download the source package. When you download the package, it is added in your AEM Forms package manager.
1. After it is downloaded, navigate to: `http://<server>:<port>/crx/packmgr/index.jsp`, and install `adobe-aemfd-forms-app-src-pkg-<version>.zip`.

1. To download the source code archive, open `http://<server>:<port>/crx/de/content/forms/mobileapps/src/adobe-lc-mobileworkspace-src-<version>.zip` in your browser.   
   The source package is downloaded on your device.

The following image displays the extracted contents of the `adobe-lc-mobileworkspace-src-<version>.zip`.

![](assets/MWS-Content.png)

The following table details contents of the `adobe-lc-mobileworkspace-src-[version]/ios` folder.

<table cellpadding="4" cellspacing="0"> 
 <tbody> 
  <tr> 
   <th class="row-nocellborder" valign="top" width="NaN%"><p>Directory</p> </th> 
   <th class="cellrowborder" valign="top" width="NaN%"><p>Content</p> </th> 
  </tr> 
  <tr> 
   <td class="row-nocellborder" headers="d19e270 " valign="top" width="NaN%"><p><span class="code">CordovaLib</span></p> </td> 
   <td class="cellrowborder" headers="d19e273 " valign="top" width="NaN%"><p>PhoneGap SDK 6.4.0</p> </td> 
  </tr> 
  <tr> 
   <td class="row-nocellborder" headers="d19e270 " valign="top" width="NaN%"><p><span class="code">AEM Forms</span></p> </td> 
   <td class="cellrowborder" headers="d19e273 " valign="top" width="NaN%"><p>Resources, PhoneGap plug-ins, and application's main module</p> </td> 
  </tr> 
  <tr> 
   <td class="row-nocellborder" headers="d19e270 " valign="top" width="NaN%"><p><span class="code">AEM Forms.xcodeproj</span></p> </td> 
   <td class="cellrowborder" headers="d19e273 " valign="top" width="NaN%"><p>Xcode project for AEM Forms app</p> </td> 
  </tr> 
  <tr> 
   <td class="row-nocellborder" headers="d19e270 " valign="top" width="NaN%"><p><span class="code">www</span></p> </td> 
   <td class="cellrowborder" headers="d19e273 " valign="top" width="NaN%"><p>HTML, CSS, images, and JavaScript files for the AEM Forms app project</p> </td> 
  </tr> 
 </tbody> 
</table>

For detailed information about Code Signing and adding devices to the iOS Provisioning Portal, see [iOS Code Signing Setup, Process, and Troubleshooting](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html).

## Build standard AEM Forms app <br> {#set-up-the-xcode-project}

1. Perform the following steps to set up a project in Xcode and provide a signing identity:

   Log in to your Mac machine that has Xcode and iOS SDK installed and configured.

1. Copy the `adobe-lc-mobileworkspace-src-<version>.zip` archive from the downloads folder to `[*User_Home*]/Projects/`.
1. Extract the archive in the `[*User_Home*]/Projects/[your-project]`directory.
1. Navigate to the ` [*User_Home*]/Projects/ `[your-project]`/adobe-lc-mobileworkspace-src-[version]/ios` directory.
1. Open the `AEM Forms.xcodeproj` project in Xcode.
1. Click **AEM Forms**, under **TARGETS**, select **AEM Forms**. Select the **Build Settings **tab, locate the **Code Signing Entitlement** section, and in Debug and Release fields do one of the following:

    * Leave the fields unspecified to build a standard Mobile Workspace app
    * Specify the fields to as explained in [Building a Secure AEM Forms app for iOS](../../forms/using/building-secure-mobile-workspace-app.md) to build a secure AEM Forms app.

1. In the **Build Settings** tab, click **All** and then click **Combined**.
1. From the **Settings** list, expand **Code Signing**. 
1. For **Code Signing Identity**, select the appropriate signature. For detailed information about, creating new signatures, see [Creating and Downloading Development Provisioning Profiles](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/CreatingYourTeamProvisioningProfile/CreatingYourTeamProvisioningProfile.html).
1. Ensure that the same signature is selected for **Debug**, **Release**, and **Any iOS SDK**.
1. Replace the following code in the `AEM Forms-info.plist` file:

   ```java
   <key>NSAppTransportSecurity</key>
   <dict>
   <key>NSAllowsArbitraryLoads</key>
   <true/>
   </dict>
   ```

   with the following while replacing `yourserver.com` with an appropriate host name for your server.

   ```java
   <key>NSAppTransportSecurity</key>
   <dict>
   <key>NSExceptionDomains</key>
   <dict>
   <key>yourserver.com</key>
   <dict>
   <!-Include to allow subdomains->
   <key>NSIncludesSubdomains</key>
   <true/>
   <!-Include to allow HTTP requests->
   <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
   <true/>
   <!-Include to support forward secrecy->
   <key>NSExceptionRequiresForwardSecrecy</key>
   <false/>
   <!-Include to specify minimum TLS version->
   <key>NSTemporaryExceptionMinimumTLSVersion</key>
   <string>TLSv1.1</string>
   </dict>
   </dict>
   </dict>
   ```

   >[!NOTE]
   >
   >This step is required only if AEM Forms app needs to connect to a server that does not follow App Transport Security requirements.

1. Under **PROJECT**, select **AEM Forms **and ensure that the appropriate signature is selected for **Code Signing Identity**, **Debug**, **Release** and **Any iOS SDK**.
1. Connect a provisioned iPad to a Mac machine. 
1. Select the provisioned device for the **AEM Forms **project.

   ![](assets/Ipad.png)

1. Select **Product** &gt; **Clean**.
1. Select **Product** &gt; **Build**.

## Build the installer for the AEM Forms app <br> {#build-the-installer-for-the-mobile-workspace-app}

You need to archive the Xcode project to build the installer (an .ipa file) and a property list (a .plist file) file. The property list file contains configuration information of the hosted in-house app, such as the name and the hosting location of the app. For more information about property list file, see [About Information Property List Files](http://developer.apple.com/library/ios/#documentation/general/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html).

1. Connect a provisioned iPad to a Mac machine. For detailed information about provisioning an iPad, see [Creating and Downloading Development Provisioning Profiles](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/CreatingYourTeamProvisioningProfile/CreatingYourTeamProvisioningProfile.html)
1. Select the provisioned device for the **AEM Forms **project.

   ![](assets/Ipad-1.png)

1. Select **Product** &gt; **Clean**.
1. Select **Product** &gt; **Build**.
1. Select **Product** &gt; **Archive**.
1. In Organizer - Archives, select the latest archive of your project and click **Distribute**.
1. Select **Save for Enterprise or Ad-Hoc Deployment** as the method of distribution and click **Next**.
1. Select the appropriate **Code Signing Identity** and click **Next**. Click **Allow** to apply the signature.
1. Provide name of the app and select **Save for Enterprise Distribution**.
1. Provide the **Application URL** for the app. For example, to host the app on a CRX server, provide URL `http://[*LC_host*]:[*port*]/lc/content/distribution/mobileworkspace/APP_NAME.ipa`.
1. In the **Title** field, specify AEM Forms.
1. Click **Save** and close Xcode.

   An installer file, `AEM Forms.ipa`, and property list file, `AEM Forms-info.plist`, are created at the specified location.

1. Open the `AEM Forms-info.plist` file in an editor.
1. Replace all the spaces in the URL of your .ipa file with %20.
1. Save and close the `AEM Forms-info.plist` file.

[**Contact Support**](https://www.adobe.com/account/sign-in.supportportal.html)

>[!MORE_LIKE_THIS]
>
>* [Set up your environment](../../forms/using/setup-environment-mobile-workspace.md)
>* [Set up the Xcode project and build the iOS app](../../forms/using/setup-xcode-project-build-installer.md)
>* [Set up the Eclipse project and build the Android app](../../forms/using/setup-eclipse-project-build-installer.md)
>* [Set up the Visual Studio project and build the Windows app](../../forms/using/setup-visual-studio-project-build-installer.md)
>* [Distribute the AEM Forms app](../../forms/using/distribute-mobile-workspace-app.md)
>* [Building a secure AEM Forms app for iOS](../../forms/using/building-secure-mobile-workspace-app.md)