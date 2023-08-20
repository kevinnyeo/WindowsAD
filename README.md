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
[Step 2] Domain Controller setup <br/>
 - Active Directory Domain Service [Stores devices and services on our network]
 - Remote Access Server/Network Address Translation [Eg employee accessing company network remotely]
 - DHCP [Seamless integration with AD and DNS services]
<br/>
[Step 3] Creating of users in Active Directory with Powershell script or manually [Simulating employees onboarding]
<br/>
[Step 4] Endpoint CLient step [Simulating company's desktop/laptop]


<h2>Program walk-through:</h2>

<p align="center">
<b>Microsoft Windows Defender Antivirus</b>  <br/>
 1. Launch Windows Security Application <br/>
 2. Navigate to Virus and threat protection <br/>
 3. Click on Quick scan <br/>
<img src="https://i.imgur.com/2cgGtCu.png" height="80%" width="80%" />
<br />
 


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
