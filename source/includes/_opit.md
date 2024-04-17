<h1 id='opit'>OPIT</h1>
<p>Odero POS Integration Toolkit for Developers<br/></p>

<p>Welcome to OPit, the Odero POS Integration Toolkit designed to streamline and enhance the communication between a Windows device (PC, laptop) and an Odero POS terminal. OPit enables developers to send commands such as Payment Requests, Settlements, Reports, and more, directly to the Odero POS terminal over a TCP/IP socket connection. By leveraging this powerful toolkit, developers can seamlessly integrate their Windows applications with Odero POS, unlocking new possibilities for efficient and secure payment processing.<br/></p>

<p>At its core, OPit provides a reliable and secure channel for data exchange between the Windows device and the Odero POS terminal. This communication is facilitated through TCP/IP sockets, allowing for real-time, bidirectional communication over a local network. To ensure a seamless connection, both the Windows device and the Odero POS terminal must be connected to the same router or Wi-Fi network.<br/></p>

<p>With OPit, developers gain full control over the payment flow, enabling them to initiate payment requests from their Windows applications and receive responses from the Odero POS terminal. This integration empowers developers to create custom workflows, tailor the user experience, and incorporate Odero POS functionality seamlessly into their existing applications. Whether you&#39;re building a point-of-sale system, a payment gateway, or any other payment-related application, OPit provides the tools you need to connect and communicate effortlessly with Odero POS.<br/></p>

<p>By leveraging the power of OPit, developers can harness the extensive capabilities of Odero POS, such as processing transactions, generating reports, performing settlements, and more, directly from their Windows applications. This seamless integration ensures a cohesive experience for users and eliminates the need for manual data entry or redundant workflows. With OPit, you can empower your users with a unified and efficient payment processing solution, offering a streamlined experience that enhances productivity and accuracy.<br/></p>

<p>In this developer documentation, we will provide you with comprehensive guidance on utilizing OPit to integrate your Windows applications with Odero POS. You&#39;ll find detailed instructions, sample code snippets, and best practices to help you make the most of this powerful toolkit. Whether you&#39;re a seasoned developer or just getting started, this documentation will serve as your roadmap to successfully integrating OPit and unlocking the full potential of Odero POS in your applications.<br/></p>

<p>We&#39;re excited to have you embark on this journey with OPit, and we look forward to witnessing the innovative solutions you&#39;ll build with the power of seamless communication between Windows devices and Odero POS terminals. Let&#39;s dive in and get started!<br/></p>
<h2 id='introduction'>Introduction</h2>
<p>OPit Lan for Windows is a C++ library which allows for a Windows device (PC, laptop) to send commands (Payment Request, Settlement, Reports etc) to an Odero Pos terminal.<br/>
The communication between the two devices is performed through TCP/IP sockets.<br/>
Both devices have to be connected to the same router / wifi network.</p>

<p>Please follow the below instructions for setup:<br/>
<ol type="1">
  <li>To use OPIT Lan you will need to use static ips for both devices, on the client tablet and the Odero POS terminal, and to have a dedicated router with - potentially - DHCP off (depends on the router)<br/>
Tutorial on how to set up static ip on Windows can be found <a href="https://pureinfotech.com/set-static-ip-address-windows-10">here</a>
</li>
  <li>Connect to wifi on the dedicated router on both devices.</li>
  <li>The Odero POS terminal must have the ip 192.168.1.1 this is the ip where the client device expects to find the host.</li>
  <li>Install app-lan-host-release.apk on the Odero POS terminal and OpitDesktopSampleApp on the Windows device.</li>
  <li>Start the OPIT Lan app on the Odero POS terminal.</li>
  <li>Start Sample app on windows.</li>
  <li>Send payment request.</li>
</ol></p>

<p><br/>
Below you can find the OPit Lan for Windows diagram.
<p align="center">
<img src="../images/opit_lan_windows_diagram-08dd9580.jpeg" width=700></p></p>

<ol type="1">
  <li>OPit lan host on Odero POS terminal is started and the socket server is booted up.</li>
  <li>Client app (that implement OPit library) starts on windows device and connects to the socket server from the Odero POS terminal.</li>
  <li>Client app receives onConnected callback.</li>
  <li>Client app sends payment request via opened tcp/ip socket and is received on the Odero POS terminal.</li>
  <li>Client app terminates the socket connection.</li>
  <li>Client app receives onDisconnected callback.</li>
  <li>Odero POS terminal switchs from wifi to gsm data as required by the bank app in order to perform the transaction on secure connection. Also the host socket server is shut down.</li>
  <li>Odero POS terminal host awaits for response from the bank app.</li>
  <li>Client app sends connnection ping requests attempting to connect to the socket server on the host device. These attempts fail at this point because the socket server is shut down as described in previous step.</li>
  <li>Host app receives response from the bank app, network switch from gsm to wifi occurs and the socket server is started.</li>
  <li>Client app connection request is successful.</li>
  <li>Host app on Odero POS terminal sends the response from the bank app and is received on the client app (windows)</li>
</ol>

<p>The client app side implementation is rather simple, only step 4 is required to be perfomed, all other steps on the client side are performed under the hood inside the library.</p>
<h2 id='opit-setup'>Setup</h2>
<ol type="1">
  <li>Download the latest OPIT Lan for Windows <a href="https://github.com/OderoPos/opit-lan-windows/releases">release</a> and unzip the archive.</li>
  <li>Copy archive contents inside the sample project folder.</li>
  <li>Open the sample app project solution with Microsoft Visual Studio (make sure C++ module is installed).</li>
  <li>In Microsoft Visual Studio go to Project -> Properties -> C/C++ -> Command Line -> Additional Options -> add text: /std:c++latest
<p align="center"><img src="../images/1-c57c7976.png" width=700></p>
  </li>
  <li>C/C++ -> General -> Additional Include Directories -> click on the dropdown -> Edit -> 
in the popup press the 3 dots (...) button from the right -> Choose headers folder from the opit-desktop project -> Ok
<p align="center"><img src="../images/2a-f3a7b050.png" width=700></p>
<p align="center"><img src="../images/2b-e4d83d86.png" width=700></p>
  </li>

  <li>Linker -> Input -> Additional Dependencies -> click on the dropdown -> Edit -> add text: OpitDesktop.lib -> Ok
<p align="center"><img src="../images/3a-05caa5b3.png" width=700></p>
<p align="center"><img src="../images/3b-45cf7f51.png" width=700></p>
</li>

  <li>Linker -> General -> Additional Library Directories -> click on the dropdown -> Edit -> 
in the popup press the 3 dots (...) button from the right -> select the folder where the OpitDesktop.lib file is located -> Ok
<p align="center"><img src="../images/4a-d6d978e6.png" width=700></p>
<p align="center"><img src="../images/4b-5fcb9813.png" width=700></p>
</li>

  <li>That's it! The solution should now build.</li>
</ol>