---
title: API Reference

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

search: true

code_clipboard: true

---

# Introduction

Welcome to OPIT Lan 0.2.0 for Windows. Please follow the below instructions for setup:<br/>
1. To use OPIT Lan you will need a dedicated router with DHCP off and statics ips set up on the client tablet and TR400 (this is important)<br/>
Tutorial on how to set up static ip on Windows can be found [here](https://pureinfotech.com/set-static-ip-address-windows-10)<br/>
2. Connect to wifi on the dedicated router on both devices.<br/>
3. The TR400 must have the ip 192.168.1.1 this is the ip where the client device expects to find the host.<br/>
4. Install app-lan-host-release.apk on TR400 and OpitDesktopSampleApp on Windows.<br/>
5. Start the OPIT Lan app on TR400<br/>
6. Start Sample app on windows.<br/>
7. Send payment request.<br/>

# Setup

1. Download OPIT Lan [0.2.0](https://github.com/OderoPos/opit-windows/releases/tag/0.2.0) for Windows and unzip the archive.<br/>
2. Copy archive contents inside the sample project folder.<br/>
3. Open the sample app project solution with Microsoft Visual Studio (make sure C++ module is installed).<br/>
4. In Microsoft Visual Studio go to Project -> Properties -> C/C++ -> Command Line -> Additional Options -> add /std:c++latest<br/>
5. Go to Project -> Properties -> C/C++ -> General -> Additional Include Directories -> click on the dropdown -> Edit -> 
in the popup press the 3 dots (...) button from the right -> Choose headers folder from the opit-desktop project<br/>
6. Project -> Properties -> Linker -> Input -> Additional Dependencies -> click on the dropdown -> 
Edit -> OpitDesktop.lib -> Ok<br/>
7. Project -> Properties -> Linker -> General -> Additional Library Directories -> click on the dropdown -> Edit -> 
in the popup press the 3 dots (...) button from the right -> select x64\Debug folder from opit-desktop project<br/>