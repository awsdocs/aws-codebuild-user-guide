# Third party notices for AWS CodeBuild for Windows<a name="notice"></a>

When you use CodeBuild for Windows builds, you have the option to use some third party packages and modules to enable your built application to run on Microsoft Windows operating systems and to interoperate with some third party products\. The following list contains the applicable third\-party legal terms that govern your use of the specified third\-party packages and modules\.

**Topics**
+ [1\) base Docker image—windowsservercore](#base-docker-image)
+ [2\) windows\-base Docker image—choco](#3-windows-base-docker-image)
+ [3\) windows\-base Docker image—git \-\-version 2\.16\.2](#4-windows-base-docker-image-2-16-2)
+ [4\) windows\-base Docker image—microsoft\-build\-tools \-\-version 15\.0\.26320\.2](#5-windows-base-docker-image-15-x)
+ [5\) windows\-base Docker image—nuget\.commandline \-\-version 4\.5\.1](#6-windows-base-docker-image-4-5-1)
+ [7\) windows\-base Docker image—netfx\-4\.6\.2\-devpack](#7-windows-base-docker-image-4-6-2)
+ [8\) windows\-base Docker image—visualfsharptools, v 4\.0](#8-windows-base-docker-image-visualfsharptools)
+ [9\) windows\-base Docker image—netfx\-pcl\-reference\-assemblies\-4\.6](#9-windows-base-docker-image)
+ [10\) windows\-base Docker image—visualcppbuildtools v 14\.0\.25420\.1](#10-windows-base-docker-image)
+ [11\) windows\-base Docker image—microsoft\-windows\-netfx3\-ondemand\-package\.cab](#11-windows-base-docker-image)
+ [12\) windows\-base Docker image—dotnet\-sdk](#12-windows-base-docker-image)

## 1\) base Docker image—windowsservercore<a name="base-docker-image"></a>

\(license terms available at: [https://hub\.docker\.com/r/microsoft/windowsservercore/\)](https://hub.docker.com/r/microsoft/windowsservercore/)

License: By requesting and using this Container OS Image for Windows containers, you acknowledge, understand, and consent to the following Supplemental License Terms:

MICROSOFT SOFTWARE SUPPLEMENTAL LICENSE TERMS

CONTAINER OS IMAGE

Microsoft Corporation \(or based on where you live, one of its affiliates\) \(referenced as "us," "we," or "Microsoft"\) licenses this Container OS Image supplement to you \("Supplement"\)\. You are licensed to use this Supplement in conjunction with the underlying host operating system software \("Host Software"\) solely to assist running the containers feature in the Host Software\. The Host Software license terms apply to your use of the Supplement\. You may not use it if you do not have a license for the Host Software\. You may use this Supplement with each validly licensed copy of the Host Software\.

ADDITIONAL LICENSING REQUIREMENTS AND/OR USE RIGHTS

Your use of the Supplement as specified in the preceding paragraph may result in the creation or modification of a container image \("Container Image"\) that includes certain Supplement components\. For clarity, a Container Image is separate and distinct from a virtual machine or virtual appliance image\. Pursuant to these license terms, we grant you a restricted right to redistribute such Supplement components under the following conditions:

\(i\) you may use the Supplement components only as used in, and as a part of your Container Image,

\(ii\) you may use such Supplement components in your Container Image as long as you have significant primary functionality in your Container Image that is materially separate and distinct from the Supplement; and

\(iii\) you agree to include these license terms \(or similar terms required by us or a hoster\) with your Container Image to properly license the possible use of the Supplement components by your end\-users\.

We reserve all other rights not expressly granted herein\.

By using this Supplement, you accept these terms\. If you do not accept them, do not use this Supplement\.

As part of the Supplemental License Terms for this Container OS Image for Windows containers, you are also subject to the underlying Windows Server host software license terms, which are located at: [https://www\.microsoft\.com/en\-us/useterms\.](https://www.microsoft.com/en-us/useterms)

## 2\) windows\-base Docker image—choco<a name="3-windows-base-docker-image"></a>

\(license terms available at: [https://github\.com/chocolatey/chocolatey\.org/blob/master/LICENSE\.txt](https://github.com/chocolatey/chocolatey.org/blob/master/LICENSE.txt)\)

Copyright 2011 \- Present RealDimensions Software, LLC

Licensed under the Apache License, version 2\.0 \(the "License"\); you may not use these files except in compliance with the License\. You may obtain a copy of the License at

[http://www\.apache\.org/licenses/LICENSE\-2\.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or as agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied\. See the License for the specific language governing permissions and limitations under the License\.

## 3\) windows\-base Docker image—git \-\-version 2\.16\.2<a name="4-windows-base-docker-image-2-16-2"></a>

\(license terms available at: [https://chocolatey\.org/packages/git/2\.16\.2](https://chocolatey.org/packages/git/2.16.2)\)

Licensed under GNU General Public License, version 2, available at: [https://www\.gnu\.org/licenses/old\-licenses/gpl\-2\.0\.html](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)\.

## 4\) windows\-base Docker image—microsoft\-build\-tools \-\-version 15\.0\.26320\.2<a name="5-windows-base-docker-image-15-x"></a>

\(license terms available at: [https://www\.visualstudio\.com/license\-terms/mt171552/](https://www.visualstudio.com/license-terms/mt171552/)\)

MICROSOFT VISUAL STUDIO 2015 EXTENSIONS, VISUAL STUDIO SHELLS and C\+\+ REDISTRIBUTABLE

\-\-\-\-\-

These license terms are an agreement between Microsoft Corporation \(or based on where you live, one of its affiliates\) and you\. They apply to the software named above\. The terms also apply to any Microsoft services or updates for the software, except to the extent those have additional terms\.

\-\-\-\-\-

IF YOU COMPLY WITH THESE LICENSE TERMS, YOU HAVE THE RIGHTS BELOW\.

1. **INSTALLATION AND USE RIGHTS**\. You may install and use any number of copies of the software\.

1. **TERMS FOR SPECIFIC COMPONENTS**\.

   1. **Utilities**\. The software may contain some items on the Utilities List at [https://docs\.microsoft\.com/en\-us/visualstudio/productinfo/2015\-redistribution\-vs](https://docs.microsoft.com/en-us/visualstudio/productinfo/2015-redistribution-vs)\. You may copy and install those items, if included with the software, on to yours or other third party machines, to debug and deploy your applications and databases you developed with the software\. Please note that Utilities are designed for temporary use, that Microsoft may not be able to patch or update Utilities separately from the rest of the software, and that some Utilities by their nature may make it possible for others to access machines on which they are installed\. As a result, you should delete all Utilities you have installed after you finish debugging or deploying your applications and databases\. Microsoft is not responsible for any third party use or access of Utilities you install on any machine\.

   1. **Microsoft Platforms**\. The software may include components from Microsoft Windows; Microsoft Windows Server; Microsoft SQL Server; Microsoft Exchange; Microsoft Office; and Microsoft SharePoint\. These components are governed by separate agreements and their own product support policies, as described in the license terms found in the installation directory for that component or in the "Licenses" folder accompanying the software\.

   1. **Third Party Components**\. The software may include third party components with separate legal notices or governed by other agreements, as described in the ThirdPartyNotices file accompanying the software\. Even if such components are governed by other agreements, the disclaimers and the limitations on and exclusions of damages below also apply\. The software may also include components licensed under open source licenses with source code availability obligations\. Copies of those licenses, if applicable, are included in the ThirdPartyNotices file\. You may obtain this source code from us, if and as required under the relevant open source licenses, by sending a money order or check for $5\.00 to: Source Code Compliance Team, Microsoft Corporation, 1 Microsoft Way, Redmond, WA 98052\. Please write source code for one or more of the components listed below in the memo line of your payment: 
      + Remote Tools for Visual Studio 2015;
      + Standalone Profiler for Visual Studio 2015;
      + IntelliTraceCollector for Visual Studio 2015;
      + Microsoft VC\+\+ Redistributable 2015;
      + Multibyte MFC Library for Visual Studio 2015;
      + Microsoft Build Tools 2015;
      + Feedback Client;
      + Visual Studio 2015 Integrated Shell; or
      + Visual Studio 2015 Isolated Shell\.

      We may also make a copy of the source code available at [http://thirdpartysource\.microsoft\.com](http://thirdpartysource.microsoft.com)\.

1. **DATA**\. The software may collect information about you and your use of the software, and send that to Microsoft\. Microsoft may use this information to provide services and improve our products and services\. You may opt\-out of many of these scenarios, but not all, as described in the product documentation\. There are also some features in the software that may enable you to collect data from users of your applications\. If you use these features to enable data collection in your applications, you must comply with applicable law, including providing appropriate notices to users of your applications\. You can learn more about data collection and use in the help documentation and the privacy statement at [https://privacy\.microsoft\.com/en\-us/privacystatement](https://privacy.microsoft.com/en-us/privacystatement)\. Your use of the software operates as your consent to these practices\.

1. **SCOPE OF LICENSE**\. The software is licensed, not sold\. This agreement only gives you some rights to use the software\. Microsoft reserves all other rights\. Unless applicable law gives you more rights despite this limitation, you may use the software only as expressly permitted in this agreement\. In doing so, you must comply with any technical limitations in the software that only allow you to use it in certain ways\. You may not
   + work around any technical limitations in the software;
   + reverse engineer, decompile or disassemble the software, or attempt to do so, except and only to the extent required by third party licensing terms governing the use of certain open\-source components that may be included with the software;
   + remove, minimize, block or modify any notices of Microsoft or its suppliers in the software;
   + use the software in any way that is against the law; or
   + share, publish, rent or lease the software, or provide the software as a stand\-alone hosted as solution for others to use\.

1. **EXPORT RESTRICTIONS**\. You must comply with all domestic and international export laws and regulations that apply to the software, which include restrictions on destinations, end users, and end use\. For further information on export restrictions, visit \(aka\.ms/exporting\)\.

1. **SUPPORT SERVICES**\. Because this software is "as is," we may not provide support services for it\.

1. **ENTIRE AGREEMENT**\. This agreement, and the terms for supplements, updates, Internet\-based services and support services that you use, are the entire agreement for the software and support services\.

1. **APPLICABLE LAW**\. If you acquired the software in the United States, Washington law applies to interpretation of and claims for breach of this agreement, and the laws of the state where you live apply to all other claims\. If you acquired the software in any other country, its laws apply\.

1. **CONSUMER RIGHTS; REGIONAL VARIATIONS**\. This agreement describes certain legal rights\. You may have other rights, including consumer rights, under the laws of your state or country\. Separate and apart from your relationship with Microsoft, you may also have rights with respect to the party from which you acquired the software\. This agreement does not change those other rights if the laws of your state or country do not permit it to do so\. For example, if you acquired the software in one of the below regions, or mandatory country law applies, then the following provisions apply to you:

   1. **Australia**\. You have statutory guarantees under the Australian Consumer Law and nothing in this agreement is intended to affect those rights\.

   1. **Canada**\. If you acquired this software in Canada, you may stop receiving updates by turning off the automatic update feature, disconnecting your device from the Internet \(if and when you re\-connect to the Internet, however, the software will resume checking for and installing updates\), or uninstalling the software\. The product documentation, if any, may also specify how to turn off updates for your specific device or software\.

   1. **Germany and Austria**\.

      1. **Warranty**\. The properly licensed software will perform substantially as described in any Microsoft materials that accompany the software\. However, Microsoft gives no contractual guarantee in relation to the licensed software\.

      1. **Limitation of Liability**\. In case of intentional conduct, gross negligence, claims based on the Product Liability Act, as well as, in case of death or personal or physical injury, Microsoft is liable according to the statutory law\.Subject to the foregoing clause \(ii\), Microsoft will only be liable for slight negligence if Microsoft is in breach of such material contractual obligations, the fulfillment of which facilitate the due performance of this agreement, the breach of which would endanger the purpose of this agreement and the compliance with which a party may constantly trust in \(so\-called "cardinal obligations"\)\. In other cases of slight negligence, Microsoft will not be liable for slight negligence\.

1. **DISCLAIMER OF WARRANTY\. THE SOFTWARE IS LICENSED “AS\-IS\.” YOU BEAR THE RISK OF USING IT\. MICROSOFT GIVES NO EXPRESS WARRANTIES, GUARANTEES OR CONDITIONS\. TO THE EXTENT PERMITTED UNDER YOUR LOCAL LAWS, MICROSOFT EXCLUDES THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON\-INFRINGEMENT\.**

1. **LIMITATION ON AND EXCLUSION OF DAMAGES\. YOU CAN RECOVER FROM MICROSOFT AND ITS SUPPLIERS ONLY DIRECT DAMAGES UP TO U\.S\. $5\.00\. YOU CANNOT RECOVER ANY OTHER DAMAGES, INCLUDING CONSEQUENTIAL, LOST PROFITS, SPECIAL, INDIRECT OR INCIDENTAL DAMAGES\.** This limitation applies to \(a\) anything related to the software, services, content \(including code\) on third party Internet sites, or third party applications; and \(b\) claims for breach of contract, breach of warranty, guarantee or condition, strict liability, negligence, or other tort to the extent permitted by applicable law\.

   It also applies even if Microsoft knew or should have known about the possibility of the damages\. The above limitation or exclusion may not apply to you because your country may not allow the exclusion or limitation of incidental, consequential or other damages\.

EULA ID: VS2015\_Update3\_ShellsRedist\_<ENU>

## 5\) windows\-base Docker image—nuget\.commandline \-\-version 4\.5\.1<a name="6-windows-base-docker-image-4-5-1"></a>

\(license terms available at: [https://github\.com/NuGet/Home/blob/dev/LICENSE\.txt](https://github.com/NuGet/Home/blob/dev/LICENSE.txt)\)

Copyright \(c\) \.NET Foundation\. All rights reserved\.

Licensed under the Apache License, version 2\.0 \(the "License"\); you may not use these files except in compliance with the License\. You may obtain a copy of the License at 

[http://www\.apache\.org/licenses/LICENSE\-2\.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or as agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied\. See the License for the specific language governing permissions and limitations under the License\.

## 7\) windows\-base Docker image—netfx\-4\.6\.2\-devpack<a name="7-windows-base-docker-image-4-6-2"></a>

**MICROSOFT SOFTWARE SUPPLEMENTAL LICENSE TERMS**

**\.NET FRAMEWORK AND ASSOCIATED LANGUAGE PACKS FOR MICROSOFT WINDOWS OPERATING SYSTEM**

\-\-\-\-\-

Microsoft Corporation \(or based on where you live, one of its affiliates\) licenses this supplement to you\. If you are licensed to use Microsoft Windows operating system software \(the "software"\), you may use this supplement\. You may not use it if you do not have a license for the software\. You may use this supplement with each validly licensed copy of the software\.

The following license terms describe additional use terms for this supplement\. These terms and the license terms for the software apply to your use of the supplement\. If there is a conflict, these supplemental license terms apply\.

**BY USING THIS SUPPLEMENT, YOU ACCEPT THESE TERMS\. IF YOU DO NOT ACCEPT THEM, DO NOT USE THIS SUPPLEMENT\.**

\-\-\-\-\-

**If you comply with these license terms, you have the rights below\.**

1. **DISTRIBUTABLE CODE\. ** The supplement is comprised of Distributable Code\. "Distributable Code" is code that you are permitted to distribute in programs you develop if you comply with the terms below\.

   1. **Right to Use and Distribute**\.
      + You may copy and distribute the object code form of the supplement\.
      + *Third Party Distribution\.* You may permit distributors of your programs to copy and distribute the Distributable Code as part of those programs\.

   1. **Distribution Requirements\. For any Distributable Code you distribute, you must**
      + add significant primary functionality to it in your programs;
      + for any Distributable Code having a filename extension of \.lib, distribute only the results of running such Distributable Code through a linker with your program;
      + distribute Distributable Code included in a setup program only as part of that setup program without modification; 
      + require distributors and external end users to agree to terms that protect it at least as much as this agreement;
      + display your valid copyright notice on your programs; and
      + indemnify, defend, and hold harmless Microsoft from any claims, including attorneys' fees, related to the distribution or use of your programs\.

   1. **Distribution Restrictions\. You may not**
      + alter any copyright, trademark or patent notice in the Distributable Code;
      + use Microsoft's trademarks in your programs' names or in a way that suggests your programs come from or are endorsed by Microsoft;
      + distribute Distributable Code to run on a platform other than the Windows platform;
      + include Distributable Code in malicious, deceptive or unlawful programs; or
      + modify or distribute the source code of any Distributable Code so that any part of it becomes subject to an Excluded License\. An Excluded License is one that requires, as a condition of use, modification or distribution, that
        + the code be disclosed or distributed in source code form; or
        + others have the right to modify it\.

1. **SUPPORT SERVICES FOR SUPPLEMENT**\. Microsoft provides support services for this software as described at [www\.support\.microsoft\.com/common/international\.aspx](http://www.support.microsoft.com/common/international.aspx)\.

## 8\) windows\-base Docker image—visualfsharptools, v 4\.0<a name="8-windows-base-docker-image-visualfsharptools"></a>

\(license terms available at: [https://github\.com/dotnet/fsharp/blob/main/License\.txt](https://github.com/dotnet/fsharp/blob/main/License.txt)\)

Copyright \(c\) Microsoft Corporation\. All rights reserved\.

Licensed under the Apache License, version 2\.0 \(the "License"\); you may not use these files except in compliance with the License\. You may obtain a copy of the License at

[http://www\.apache\.org/licenses/LICENSE\-2\.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or as agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied\. See the License for the specific language governing permissions and limitations under the License\.

## 9\) windows\-base Docker image—netfx\-pcl\-reference\-assemblies\-4\.6<a name="9-windows-base-docker-image"></a>

**MICROSOFT SOFTWARE LICENSE TERMS**

**MICROSOFT \.NET PORTABLE CLASS LIBRARY REFERENCE ASSEMBLIES – 4\.6**

\-\-\-\-\-

These license terms are an agreement between Microsoft Corporation \(or based on where you live, one of its affiliates\) and you\. Please read them\. They apply to the software named above\. The terms also apply to any Microsoft
+ updates,
+ supplements,
+ Internet\-based services, and
+ support services

for this software, unless other terms accompany those items\. If so, those terms apply\.

**BY USING THE SOFTWARE, YOU ACCEPT THESE TERMS\. IF YOU DO NOT ACCEPT THEM, DO NOT USE THE SOFTWARE\.**

\-\-\-\-\-

**IF YOU COMPLY WITH THESE LICENSE TERMS, YOU HAVE THE PERPETUAL RIGHTS BELOW\.**

1. **INSTALLATION AND USE RIGHTS**\. You may install and use any number of copies of the software to design, develop and test your programs\.

1. **ADDITIONAL LICENSING REQUIREMENTS AND/OR USE RIGHTS**\.

   1. **Distributable Code**\. You may distribute the software in developer tool programs you develop, to enable customers of your programs to develop portable libraries for use with any device or operating system, if you comply with the terms below\.

      1. **Right to Use and Distribute\. The software is "Distributable Code\."** 
         + *Distributable Code\.* You may copy and distribute the object code form of the software\.
         + *Third Party Distribution\.* You may permit distributors of your programs to copy and distribute the Distributable Code as part of those programs\. 

      1. **Distribution Requirements\. For any Distributable Code you distribute, you must**
         + add significant primary functionality to it in your programs;
         + require distributors and your customers to agree to terms that protect it at least as much as this agreement; 
         + display your valid copyright notice on your programs; and
         + indemnify, defend, and hold harmless Microsoft from any claims, including attorneys' fees, related to the distribution or use of your programs\.

      1. **Distribution Restrictions\. You may not**
         + alter any copyright, trademark or patent notice in the Distributable Code;
         + use Microsoft's trademarks in your programs' names or in a way that suggests your programs come from or are endorsed by Microsoft;
         + include Distributable Code in malicious, deceptive or unlawful programs; or
         + modify or distribute the Distributable Code so that any part of it becomes subject to an Excluded License\. An Excluded License is one that requires, as a condition of use, modification or distribution, that
           + the code be disclosed or distributed in source code form; or
           + others have the right to modify it\.

1. **SCOPE OF LICENSE**\. The software is licensed, not sold\. This agreement only gives you some rights to use the software\. Microsoft reserves all other rights\. Unless applicable law gives you more rights despite this limitation, you may use the software only as expressly permitted in this agreement\. In doing so, you must comply with any technical limitations in the software that only allow you to use it in certain ways\. You may not
   + work around any technical limitations in the software;
   + reverse engineer, decompile or disassemble the software, except and only to the extent that applicable law expressly permits, despite this limitation;
   + publish the software for others to copy; or
   + rent, lease or lend the software\.

1. **FEEDBACK**\. You may provide feedback about the software\. If you give feedback about the software to Microsoft, you give to Microsoft, without charge, the right to use, share and commercialize your feedback in any way and for any purpose\. You also give to third parties, without charge, any patent rights needed for their products, technologies and services to use or interface with any specific parts of a Microsoft software or service that includes the feedback\. You will not give feedback that is subject to a license that requires Microsoft to license its software or documentation to third parties because we include your feedback in them\. These rights survive this agreement\.

1. **TRANSFER TO A THIRD PARTY**\. The first user of the software may transfer it, and this agreement, directly to a third party\. Before the transfer, that party must agree that this agreement applies to the transfer and use of the software\. The first user must uninstall the software before transferring it separately from the device\. The first user may not retain any copies\.

1. **EXPORT RESTRICTIONS**\. The software is subject to United States export laws and regulations\. You must comply with all domestic and international export laws and regulations that apply to the software\. These laws include restrictions on destinations, end users and end use\. For additional information, see [www\.microsoft\.com/exporting](http://www.microsoft.com/exporting)\.

1. **SUPPORT SERVICES**\. Because this software is "as is," we may not provide support services for it\.

1. **ENTIRE AGREEMENT**\. This agreement, and the terms for supplements, updates, Internet\-based services and support services that you use, are the entire agreement for the software and any support services we provide\.

1. **APPLICABLE LAW**\.

   1. **United States**\. If you acquired the software in the United States, Washington state law governs the interpretation of this agreement and applies to claims for breach of it, regardless of conflict of laws principles\. The laws of the state where you live govern all other claims, including claims under state consumer protection laws, unfair competition laws, and in tort\.

   1. **Outside the United States**\. If you acquired the software in any other country, the laws of that country apply\.

1. **LEGAL EFFECT**\. This agreement describes certain legal rights\. You may have other rights under the laws of your country\. You may also have rights with respect to the party from whom you acquired the software\. This agreement does not change your rights under the laws of your country if the laws of your country do not permit it to do so\.

1. **DISCLAIMER OF WARRANTY\. THE SOFTWARE IS LICENSED "AS\-IS\." YOU BEAR THE RISK OF USING IT\. MICROSOFT GIVES NO EXPRESS WARRANTIES, GUARANTEES OR CONDITIONS\. YOU MAY HAVE ADDITIONAL CONSUMER RIGHTS OR STATUTORY GUARANTEES UNDER YOUR LOCAL LAWS WHICH THIS AGREEMENT CANNOT CHANGE\. TO THE EXTENT PERMITTED UNDER YOUR LOCAL LAWS, MICROSOFT EXCLUDES THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON\-INFRINGEMENT\.** 

   **FOR AUSTRALIA—YOU HAVE STATUTORY GUARANTEES UNDER THE AUSTRALIAN CONSUMER LAW AND NOTHING IN THESE TERMS IS INTENDED TO AFFECT THOSE RIGHTS\.** 

1. **LIMITATION ON AND EXCLUSION OF REMEDIES AND DAMAGES\. YOU CAN RECOVER FROM MICROSOFT AND ITS SUPPLIERS ONLY DIRECT DAMAGES UP TO U\.S\. $5\.00\. YOU CANNOT RECOVER ANY OTHER DAMAGES, INCLUDING CONSEQUENTIAL, LOST PROFITS, SPECIAL, INDIRECT OR INCIDENTAL DAMAGES\.**

   This limitation applies to
   + anything related to the software, services, content \(including code\) on third party Internet sites, or third party programs; and
   + claims for breach of contract, breach of warranty, guarantee or condition, strict liability, negligence, or other tort to the extent permitted by applicable law\.

   It also applies even if Microsoft knew or should have known about the possibility of the damages\. The above limitation or exclusion may not apply to you because your country may not allow the exclusion or limitation of incidental, consequential or other damages\.

## 10\) windows\-base Docker image—visualcppbuildtools v 14\.0\.25420\.1<a name="10-windows-base-docker-image"></a>

\(license terms available at: [https://www\.visualstudio\.com/license\-terms/mt644918/](https://www.visualstudio.com/license-terms/mt644918/)\)

MICROSOFT VISUAL C\+\+ BUILD TOOLS

**MICROSOFT SOFTWARE LICENSE TERMS**

**MICROSOFT VISUAL C\+\+ BUILD TOOLS**

\-\-\-\-\-

These license terms are an agreement between Microsoft Corporation \(or based on where you live, one of its affiliates\) and you\. They apply to the software named above\. The terms also apply to any Microsoft services or updates for the software, except to the extent those have different terms\.

\-\-\-\-\-

**IF YOU COMPLY WITH THESE LICENSE TERMS, YOU HAVE THE RIGHTS BELOW\.**

1. **INSTALLATION AND USE RIGHTS**\.

   1. One user may use copies of the software to develop and test their applications\.

1. **DATA**\. The software may collect information about you and your use of the software, and send that to Microsoft\. Microsoft may use this information to provide services and improve our products and services\. You may opt\-out of many of these scenarios, but not all, as described in the product documentation\. There are also some features in the software that may enable you to collect data from users of your applications\. If you use these features to enable data collection in your applications, you must comply with applicable law, including providing appropriate notices to users of your applications\. You can learn more about data collection and use in the help documentation and the privacy statement at [http://go\.microsoft\.com/fwlink/?LinkID=528096](http://go.microsoft.com/fwlink/?LinkID=528096)\. Your use of the software operates as your consent to these practices\.

1. **TERMS FOR SPECIFIC COMPONENTS**\.

   1. **Build Server**\. The software may contain some Build Server components listed in BuildServer\.TXT files, and/or any files listed on the BuildeServer list located following this Microsoft Software License Terms\. You may copy and install those items, if included in the software, onto your build machines\. You and others in your organization may use these items on your build machines solely for the purpose of compiling, building, verifying and archiving your applications or running quality or performance tests as part of the build process\.

   1. **Microsoft Platforms**\. The software may include components from Microsoft Windows; Microsoft Windows Server; Microsoft SQL Server; Microsoft Exchange; Microsoft Office; and Microsoft SharePoint\. These components are governed by separate agreements and their own product support policies, as described in the license terms found in the installation directory for that component or in the "Licenses" folder accompanying the software\.

   1. **Third Party Components**\. The software may include third party components with separate legal notices or governed by other agreements, as described in the ThirdPartyNotices file accompanying the software\. Even if such components are governed by other agreements, the disclaimers and the limitations on and exclusions of damages below also apply\.

   1. **Package Managers**\. The software may include package managers, like Nuget, that give you the option to download other Microsoft and third party software packages to use with your application\. Those packages are under their own licenses, and not this agreement\. Microsoft does not distribute, license or provide any warranties for any of the third party packages\.

1. **SCOPE OF LICENSE**\. The software is licensed, not sold\. This agreement only gives you some rights to use the software\. Microsoft reserves all other rights\. Unless applicable law gives you more rights despite this limitation, you may use the software only as expressly permitted in this agreement\. In doing so, you must comply with any technical limitations in the software that only allow you to use it in certain ways\. For more information, see [https://docs\.microsoft\.com/en\-us/legal/information\-protection/software\-license\-terms\#1\-installation\-and\-use\-rights](https://docs.microsoft.com/en-us/legal/information-protection/software-license-terms#1-installation-and-use-rights)\. You may not
   + work around any technical limitations in the software;
   + reverse engineer, decompile or disassemble the software, or attempt to do so, except and only to the extent required by third party licensing terms governing use of certain open source components that may be included with the software;
   + remove, minimize, block or modify any notices of Microsoft or its suppliers;
   + use the software in any way that is against the law; or
   + share, publish, rent or lease the software, or provide the software as a stand\-alone hosted as solution for others to use\.

1. **EXPORT RESTRICTIONS**\. You must comply with all domestic and international export laws and regulations that apply to the software, which include restrictions on destinations, end users and end use\. For further information on export restrictions, visit \([aka\.ms/exporting](http://aka.ms/exporting)\)\.

1. **SUPPORT SERVICES**\. Because this software is "as is," we may not provide support services for it\.

1. **ENTIRE AGREEMENT**\. This agreement, and the terms for supplements, updates, Internet\-based services and support services that you use, are the entire agreement for the software and support services\.

1. **APPLICABLE LAW**\. If you acquired the software in the United States, Washington law applies to interpretation of and claims for breach of this agreement, and the laws of the state where you live apply to all other claims\. If you acquired the software in any other country, its laws apply\.

1. **CONSUMER RIGHTS; REGIONAL VARIATIONS**\. This agreement describes certain legal rights\. You may have other rights, including consumer rights, under the laws of your state or country\. Separate and apart from your relationship with Microsoft, you may also have rights with respect to the party from which you acquired the software\. This agreement does not change those other rights if the laws of your state or country do not permit it to do so\. For example, if you acquired the software in one of the below regions, or mandatory country law applies, then the following provisions apply to you:
   + **Australia**\. You have statutory guarantees under the Australian Consumer Law and nothing in this agreement is intended to affect those rights\.
   + **Canada**\. If you acquired this software in Canada, you may stop receiving updates by turning off the automatic update feature, disconnecting your device from the Internet \(if and when you re\-connect to the Internet, however, the software will resume checking for and installing updates\), or uninstalling the software\. The product documentation, if any, may also specify how to turn off updates for your specific device or software\.
   + **Germany and Austria**\.
     + **Warranty**\. The properly licensed software will perform substantially as described in any Microsoft materials that accompany the software\. However, Microsoft gives no contractual guarantee in relation to the licensed software\.
     + *Limitation of Liability*\. In case of intentional conduct, gross negligence, claims based on the Product Liability Act, as well as, in case of death or personal or physical injury, Microsoft is liable according to the statutory law\.

       Subject to the foregoing clause \(ii\), Microsoft will only be liable for slight negligence if Microsoft is in breach of such material contractual obligations, the fulfillment of which facilitate the due performance of this agreement, the breach of which would endanger the purpose of this agreement and the compliance with which a party may constantly trust in \(so\-called "cardinal obligations"\)\. In other cases of slight negligence, Microsoft will not be liable for slight negligence\. 

1. **LEGAL EFFECT**\. This agreement describes certain legal rights\. You may have other rights under the laws of your state or country\. This agreement does not change your rights under the laws of your state or country if the laws of your state or country do not permit it to do so\. Without limitation of the foregoing, for Australia, **YOU HAVE STATUTORY GUARANTEES UNDER THE AUSTRALIAN CONSUMER LAW AND NOTHING IN THESE TERMS IS INTENDED TO AFFECT THOSE RIGHTS**

1. **DISCLAIMER OF WARRANTY\. THE SOFTWARE IS LICENSED "AS\-IS\." YOU BEAR THE RISK OF USING IT\. MICROSOFT GIVES NO EXPRESS WARRANTIES, GUARANTEES OR CONDITIONS\. TO THE EXTENT PERMITTED UNDER YOUR LOCAL LAWS, MICROSOFT EXCLUDES THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON\-INFRINGEMENT\.**

1. **LIMITATION ON AND EXCLUSION OF DAMAGES\. YOU CAN RECOVER FROM MICROSOFT AND ITS SUPPLIERS ONLY DIRECT DAMAGES UP TO U\.S\. $5\.00\. YOU CANNOT RECOVER ANY OTHER DAMAGES, INCLUDING CONSEQUENTIAL, LOST PROFITS, SPECIAL, INDIRECT OR INCIDENTAL DAMAGES\.**

   This limitation applies to \(a\) anything related to the software, services, content \(including code\) on third party Internet sites, or third party applications; and \(b\) claims for breach of contract, breach of warranty, guarantee or condition, strict liability, negligence, or other tort to the extent permitted by applicable law\.

   It also applies even if Microsoft knew or should have known about the possibility of the damages\. The above limitation or exclusion may not apply to you because your country may not allow the exclusion or limitation of incidental, consequential or other damages\. 

## 11\) windows\-base Docker image—microsoft\-windows\-netfx3\-ondemand\-package\.cab<a name="11-windows-base-docker-image"></a>

**MICROSOFT SOFTWARE SUPPLEMENTAL LICENSE TERMS**

**MICROSOFT \.NET FRAMEWORK 3\.5 SP1 FOR MICROSOFT WINDOWS OPERATING SYSTEM**

\-\-\-\-\-

Microsoft Corporation \(or based on where you live, one of its affiliates\) licenses this supplement to you\. If you are licensed to use Microsoft Windows operating system software \(for which this supplement is applicable\) \(the "software"\), you may use this supplement\. You may not use it if you do not have a license for the software\. You may use a copy of this supplement with each validly licensed copy of the software\.

The following license terms describe additional use terms for this supplement\. These terms and the license terms for the software apply to your use of the supplement\. If there is a conflict, these supplemental license terms apply\.

**BY USING THIS SUPPLEMENT, YOU ACCEPT THESE TERMS\. IF YOU DO NOT ACCEPT THEM, DO NOT USE THIS SUPPLEMENT\.**

\-\-\-\-\-

**If you comply with these license terms, you have the rights below\.**

1. **SUPPORT SERVICES FOR SUPPLEMENT**\. Microsoft provides support services for this software as described at [www\.support\.microsoft\.com/common/international\.aspx](http://www.support.microsoft.com/common/international.aspx)\.

1. **MICROSOFT \.NET BENCHMARK TESTING**\. The software includes the \.NET Framework, Windows Communication Foundation, Windows Presentation Foundation, and Windows Workflow Foundation components of the Windows operating systems \(\.NET Components\)\. You may conduct internal benchmark testing of the \.NET Components\. You may disclose the results of any benchmark test of the \.NET Components, provided that you comply with the conditions set forth at [http://go\.microsoft\.com/fwlink/?LinkID=66406](http://go.microsoft.com/fwlink/?LinkID=66406)\.

   Notwithstanding any other agreement you may have with Microsoft, if you disclose such benchmark test results, Microsoft shall have the right to disclose the results of benchmark tests it conducts of your products that compete with the applicable \.NET Component, provided it complies with the same conditions set forth at [http://go\.microsoft\.com/fwlink/?LinkID=66406](http://go.microsoft.com/fwlink/?LinkID=66406)\.

## 12\) windows\-base Docker image—dotnet\-sdk<a name="12-windows-base-docker-image"></a>

\(available at [https://github\.com/dotnet/core/blob/master/LICENSE\.TXT](https://github.com/dotnet/core/blob/master/LICENSE.TXT)\) 

The MIT License \(MIT\)

Copyright \(c\) Microsoft Corporation

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files \(the "Software"\), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software\.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT\. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE\.