<h1>Windows Active Directory Setup With Powershell</h1>

<h2>Description:</h2>

- <b>Active Directory Administration Powershell automating, maintaining and de-provisioning user accounts<b/>
- <b>Setting up Remote Access Server (RAS) features to support NAT/PAT<b/>
- <b>Implementation and maintenance of Windows DNS and DHCP servers<b/>
- <b>Configuring Windows File Servers with the implementation of quotas and NTFS permissions<b/>

<h2>Objectives:</h2>

- <b>To simulate an Active Directory infrastructure in a small enterprise<b/>
- <b>To gain practical experience on fundamental networking concepts<b/>

<h2>Languages and Utilities Used:</h2>

- <b>Oracle VirtualBox v7.0.10 </b> 
- <b>Active Directory Server Manager</b>
- <b>Windows Server 2019 ISO File [Domain Controller]</b>
- <b>Windows 10 ISO File [Client Workstation]</b>
- <b>Powershell</b>
- <b>Powershell Automation Script [Bulk user creation] credits to Josh Madakor</b> 

<h2>Environments Used: </h2>

- <b>Windows Server 2019 [Virtual Machine 1] [Domain Controller]</b> 
- <b>Windows 10 [Virtual Machine 2] [Client Workstation]</b> 

<h2>Program Overview:</h2>

<p align="center">
 <img src="https://i.imgur.com/VD4ENEC.png" height="80%" width="80%" />
 <br/>
 <b>Infrastructure layout</b><br/>
 <br/>
 
 Based on the infrastructure layout, the lab will be broken down into the following sections:

[Step 1] Oracle VirtualBox setup and configuration<br/> 
<br/>
Domain Controller Setup: <br/>
<br/>
[Step 2]Active Directory Domain Service [Stores devices and services on our network]<br/>
[Step 3]Remote Access Server/Network Address Translation [Eg employee accessing company network remotely]<br/>
[Step 4]DHCP Server Setup [Seamless integration with AD and DNS services]<br/>
<br/>
Endpoint Client Setup: <br/>
<br/>
[Step 5] Creating of users in Active Directory with Powershell script or manually [Simulating employees onboarding]
<br/>
[Step 6] Endpoint Client step [Simulating company's desktop/laptop]


<h2>Program walk-through:</h2>

<p align="center">
<b>[Step 1] Oracle VirtualBox Setup</b> <br/>
<br/>
 1. Create our Domain Controller machine [Server 2019] and add two adapters. <br/>
 2. Network adapter 1 will be the INTERNET adapter [Where the DC gets connection from the internet] <br/>
 3. Network adapter 2 will be our internal adapter [Where clients connect to] <br/>
<br/>
<img src="https://i.imgur.com/YPKJeQR.png" height="80%" width="80%" />
<img src="https://i.imgur.com/s80B3st.png" height="80%" width="80%" /><br/>
<br/>
 4. Install Windows Server 2019 ISO onto machine
<img src="https://i.imgur.com/DHerYBG.png" height="80%" width="80%" />
<img src="https://i.imgur.com/bhgANrQ.png" height="80%" width="80%" /> <br/>
<br/>
 5. Configuring Ethernet Settings and IP addressing<br/>
 Open Network settings > Change adapter options<br/>
<img src="https://imgur.com/MgaGiK9.png" height="80%" width="80%" /> <br/>
<br/>
 Rename Ethernet 1 and Ethernet to differentiate between internet and internal adapter<br/>
 To check which is which: Right click ethernet > Status > check IPv4 address <br/>
 Our internet IP usually starts with 10.X.X.X and internal IP is in DHCP format in this case its 169.254.38.166<br/>
 Rename to X_internal_X and _INTERNET_ for easy identification <br/>
<img src="https://imgur.com/WsiHxZ4.png" height="80%" width="80%" />
<img src="https://imgur.com/0U5JSA6.png" height="80%" width="80%" /> <br/> 
<br/>
 Now we will assign IP to our internal network that client will use <br/>
 Right-click internal adapter > Status > Internet Protocol IPv4 > Configure <br/>
<img src="https://imgur.com/6YqIuvE.png" height="80%" width="80%" /> <br/>  
<br/>
 Assign IP to internal adapter<br/>
 IP Address: 172.16.0.1 [VirtualBox private server IP]<br/>
 Subnet Mask: 255.255.0.0 [16 bit for network/16 bit for host] <br/>
 Preferred DNS Server: 127.0.0.1 [Loopback IP] <br/> <img src="https://imgur.com/tJP9DcS.png" height="80%" width="80%" />
<br/> 
 6. Rename DC System<br/>
 Right-click System Settings > Rename PC (Advanced)<br/>
<img src="https://imgur.com/hupiTEB.png" height="80%" width="80%" /> 
<br/>
 
<p align="center">
<b>[Step 2] Active Directory Domain Server Setup<br/>
<br/>
ADDS is where all information of devices, users and software connected to the network will be stored at. <br/>
 1. Open Server Manager > Add roles and features > Install ADDS Role <br/>
<img src="https://i.imgur.com/RGQoIQM.png" height="80%" width="80%" />
<img src="https://imgur.com/GJhyzj9.png" height="80%" width="80%" />
<img src="https://imgur.com/OeRwBRt.png" height="80%" width="80%" /> <br/>
<br/>
 2. Add domain [mydomain.com] into adds <br/>
 Click promote this server to domain controller > Add new forest [mydomain.com] > Install <br/>
<img src="https://imgur.com/Ds4nmZI.png" height="80%" width="80%" />
<img src="https://imgur.com/6tVtHRr.png" height="80%" width="80%" />
<img src="https://imgur.com/IQITVWI.png" height="80%" width="80%" /> <br/>
<br/>
 3. Reboot Machine [Domain name is now present in login screen]<br/>
<img src="https://imgur.com/Vh7hrka.png" height="80%" width="80%" /> <br/>
<br/>
 4. Creating Domain Administrator User Account [This account will be used to troubleshoot client endpoint]<br/>
 Start Menu > Windows Administrator Tools > Active Directory Users And Computers <br/>
<img src="https://imgur.com/DUlatg5.png" height="80%" width="80%" /> <br/>
<br/>
 5. Create New OU object under domain > Enter user creation details > Assign 'Domain Admin' role <br/>
<img src="https://imgur.com/Cz9Mvg7.png" height="80%" width="80%" />
<img src="https://imgur.com/mvnX8WH.png" height="80%" width="80%" />
<img src="https://imgur.com/OpwSYZj.png" height="80%" width="80%" />
<img src="https://imgur.com/F8GPlRN.png" height="80%" width="80%" /> <br/> 
 
<p align="center">
<b>[Step 3] Setting up RAS and NAT<br/>
Remote Access Server will allow endpoint clients to access the company network remotely. <br/>
Network Adress Translation will allow all private IP of client assigned by DHCP to be masked by our DC IP [172.16.0.1]<br/>
<br/>
 1. Install Remote Access Role in Server Manager <br/>
 Open Server Manager > Install RAS Role > Enable VPN(RAS) and Routing <br/>
<img src="https://imgur.com/ar9JPS5.png" height="80%" width="80%" />
<img src="https://imgur.com/XBqZObY.png" height="80%" width="80%" /> <br/>
<br/> 
 2. Enable Network Address Translation [NAT]<br/>
 Go to tools > Routing and Remote Access > Right click DC Server > Click NAT > Select INTERNET Adapter <br/>
<img src="https://imgur.com/gVbdI5O.png" height="80%" width="80%" />
<img src="https://imgur.com/ntrHjOh.png" height="80%" width="80%" />
<img src="https://imgur.com/bb6kKGI.png" height="80%" width="80%" /> <br/>

<p align="center">
<b>[Step 4]DHCP Server Setup<br/>
The DC DHCP Server will automatically assign a private IP address to clients connecting to the network. <br/>
<br/>
 1. Install DHCP Role in Server Manager <br/>
 Open Server Manager > Install DHCP Server <br/>
<img src="https://imgur.com/MJVPPKD.png" height="80%" width="80%" /> <br/>
<br/>
 2. Adding in DHCP Scope<br/>
 Go to tools > DHCP > Right click DC Server > Expand DC.domain > Right-click IPv4 > New Scope <br/>
<img src="https://imgur.com/VMgTQI1.png" height="80%" width="80%" />
<img src="https://imgur.com/Dy4SXgH.png" height="80%" width="80%" /><br/>
<br/>
 3. Configure DHCP Scope<br/>
 Add in DHCP Scope range 172.16.0.100-200 [This will be range of private IP address for lease]<br/>
 Subnet mask: 255:255:255:0 <br/>
 Select private IP lease duration [This will determine how long the private IP will stay assigned to the client device<br/>
<img src="https://imgur.com/p5Qhs1D.png" height="80%" width="80%" />
<img src="https://imgur.com/xkm8okn.png" height="80%" width="80%" /><br/> 
<br/>
 4. Add Router IP and DNS Server [172.16.0.1] <br/>
 This is an important step to ensure clients are able to access the internet<br/>
<img src="https://imgur.com/BZ5Ijcq.png" height="80%" width="80%" />
<img src="https://imgur.com/LbUQCBP.png" height="80%" width="80%" /><br/>  
<br/>
 

</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
