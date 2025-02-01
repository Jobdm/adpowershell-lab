<h1>Active Directory with Powershell</h1>
In this lab we will set up an active directory environment.<br />

<h2>Environments and Technologies Used</h2>

- QEMU/KVM with Virt-Manager (Kernal Virtual Machine)
- Active Directory
- Dynamic Host Connection Protocol (DHCP)
- Remote Access Services (RAS) & Network Address Translation (NAT)
- Powershell

<h2>Operating Systems Used </h2>

- Linux Mint (Host OS)
    - Desktop environment: KDE Plasma 5.27

- Windows Server 2022 (Guest OS)
- Windows 10 Pro (Guest OS)

</details>  
<hr/>
<details>
 <summary><b>- Configuring Internal Network NIC</b></summary>
 <p align="center">
<b>Go to Control Panel -> Network and Internet -> Network Connections. We should see two NICs, one connected to the internet and the other to the bridge connection.</b>
</p>
<p>Steps to take:</p>
<ul>
    <li>Rename the NICs to help identify them easier. </li>
    <li>Right-click the bridged connection -> `properties` -> `Internet Protocol Version 4 (TCP/IPv4)` -> click `properties`.</li>
    <li>Configure IPv4:
        <ul>
            <li>IP address is the same as what you used for the bridged connection. Subnet Mask: `255.255.255.0`.</li>
            <li>Leave 'Default gateway' blank. The domain controller itself will serve as the gateway.</li>
            <li>Enter 'loopback' address (127.0.0.1) as preferred DNS address. It will cause us to reference ourselves as the DNS server which is what we want.</li>
        </ul>
    </li>
    <li>Hit `OK` when done.</li>
</ul>
<p align="center">
<img src="https://i.imgur.com/CoaWm2s.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
</details>  
<hr/>
<details>
 <summary><b>- Creating Domain</b></summary>
<p align="center">
<b>Time to create a domain. We start by installing active directory domain services.</b>
</p>
<p>In Server Manager click "Add roles and features."</p>
<p align="center">
<img src="https://i.imgur.com/SlWHMO1.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>We will get this wizard. Click next until you get to 'Server Roles' and select `Active Directory Domain Services`, then hit `Add Features.`</p>
<p align="center">
<img src="https://i.imgur.com/fm61iTn.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/HRLACz9.png" height="50%" width="50%" alt="enter role name"/>
</p>
<p>Keep hitting `Next` until you get to 'Confirmation' screen and then hit `Install.`</p>
<p align="center">
<img src="https://i.imgur.com/JxgFTTU.png" height="60%" width="60%" alt="enter role name"/>
</p>
<br />
<hr/>
<p align="center"><b>With active directory now installed we can make our domain. Click the flag icon -> click `Promote this server to a domain controller.`</b></p>
<p align="center">
<img src="https://i.imgur.com/23yW0YT.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>Select `Add a new forest` and type in a name for the domain, then hit `Next.`</p>
<p align="center">
<img src="https://i.imgur.com/5NA89XD.png" height="50%" width="50%" alt="enter role name"/>
</p>
<p>Create a password for your domain and continue clicking `next` until you get to the 'prerequisite check' screen.</p>
<p align="center">
<img src="https://i.imgur.com/mcA7W4W.png" height="60%" width="60%" alt="enter role name"/>
</p>
<br />
<p>Click `Install` and wait until it's finished. You should be automatically signed out and rebooted after installation is completed.</p>
<p align="center">
<img src="https://i.imgur.com/bDn5Gcf.png" height="60%" width="60%" alt="enter role name"/>
</p>
<br />
<p>Next time you're at the login screen you'll see something like this. It means that your domain has been created. You can login with your password.</p>
<p align="center">
<img src="https://i.imgur.com/Icvr8Mg.png" height="60%" width="60%" alt="enter role name"/>
</p>
<br />
</details>  
<hr/>
<details>
 <summary><b>- Creating Domain Administrator</b></summary>
 <p align="center">
<b>Time to create an admin user.</b>
</p>
<p>Go to Start Menu -> Windows Administration Tools -> Active Directory Users and Computers.</p>
<p align="center">
<img src="https://i.imgur.com/kALXTGv.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>We can see our domain. Right-click it -> `New` -> `Organizational Unit.` This organizational unit (OU) is basically a folder where the admin user profile will be stored.</p>
<p align="center">
<img src="https://i.imgur.com/XjkG1nG.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>Right click the OU -> `New` -> `User.` Fill in the info and give it a password. For this lab I gave it a simple password and won't require it to be changed. I also added an "a-" to the username because it's an admin account.</p>
<p align="center">
<img src="https://i.imgur.com/Z71G8wp.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />

<p>Right-click the new user -> `properties.` In the "Member Of" tab click `add` and type in "Domain Admins" into the textbox, then click `OK. Click `OK` in the 'properties' window, then sign out.</p>
<p align="center">
<img src="https://i.imgur.com/WUJFMKY.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/DCRxU0Y.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />

<p align="center"><b>We can now sign in as the new admin.</b></p>
<p align="center">
<img src="https://i.imgur.com/RCkx46z.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/5vsZH4a.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />

</details>  
<hr/>
<details>
 <summary><b>- RAS and NAT Setup</b></summary>
 <p align="center">
<b>Next we will install Remote Access Server (RAS) and Network Address Translation (NAT). This will enable a client computer to access the internet through the domain controller while being in the internal network.</b>
</p>
<p>Go back into 'Add Roles and Features' and hit `Next` until we get to 'Sever Roles' and select `Remote Access` roles. Keep hitting `Next` until we get to 'Role Services' and install 'Routing'services.</p>
<p align="center">
<img src="https://i.imgur.com/lYAMODY.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/IrCHQlA.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>Confirm and install the services.</p>
<p align="center">
<img src="https://i.imgur.com/k0J79wI.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/IsHLNrc.png" height="50%" width="50%" alt="enter role name"/>
</p><br />
<p>Now we can configure our NAT. Go to 'Tools' -> `Routing and Remote Access` to bring up the service window. Right-click the server -> `Configure and Enable Routing and Remote Access.` Select `Network Address Translation (NAT)` and hit `next.`</p>
<p align="center">
<img src="https://i.imgur.com/zjOY4bh.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/s6Su0kG.png" height="50%" width="50%" alt="enter role name"/>
</p><br />
<p>We select our internet enabled NIC -> `next` and finish the configuration.</p>
<p align="center">
<img src="https://i.imgur.com/Dxflop5.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/o4xgYE7.png" height="50%" width="50%" alt="enter role name"/>
</p>
<br />

</details>  
<hr/>
<details>
 <summary><b>- DHCP Setup</b></summary>
 <p align="center">
<b>Now we will setup dynamic host connection protocol (DHCP) server for the DC. The DHCP server will enable client computers using the DC to automatically receive an IP address.</b>
</p>
<p>In 'Add roles and features' we select `DHCP` in server roles and add features, then install.</p>
<p align="center">
<img src="https://i.imgur.com/s5OXb0C.png" height="60%" width="60%" alt="UI to add Role"/>
<img src="https://i.imgur.com/FAfsV8k.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>Once installed right-click 'IPv4' -> `New Scope` to bring up the 'New Scope Wizard.' I named mine according to the IP range I plan on working within.</p>
<p align="center">
<img src="https://i.imgur.com/ps110Ex.png" height="60%" width="60%" alt="enter role name"/>
<img src="https://i.imgur.com/2OXwQ5c.png" height="60%" width="60%" alt="enter role name"/>
</p>
<p>We configure our new scope by specifying an IP range and Subnet Mask. We also add a default gateway.</p>
<p align="center">
<img src="https://i.imgur.com/NQvvFJj.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/A40rZMN.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/cNMlFXw.png" height="50%" width="50%" alt="enter role name"/>
</p>
<br />
<p>Lastly we disable Internet Explorer Enhanced Security Configuration so we can avoid warnings everytime we browse a new webpage. We don't need it for our lab.</p>
<p align="center">
<img src="https://i.imgur.com/YGMTAGN.png" height="70%" width="70%" alt="enter role name"/>
</p>
<br />

</details>  
<hr/>
<details>
 <summary><b>- Creating Users With Powershell</b></summary>
 <p align="center">
<b>Time to make our user accounts.</b>
</p>
<p>We open up powershell: Start menu -> Windows Powershell -> Windows Powershell ISE. open the '.ps1' script that will create our users. It will generate user profiles pulling names from the 'names' text file in same folder. We can even add more names by editting the text file.</p>
<p align="center">
<img src="https://i.imgur.com/nOHSixI.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/0wq9SI5.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/heUSTe9.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>In order to run the script we need to enable ' unrestricted execution policy'. Run command: Set-ExecutionPolicy Unrestricted and click 'Yes to All'.</p>
<p align="center">
<img src="https://i.imgur.com/L424QtE.png" height="50%" width="50%" alt="enter role name"/>
</p>
<p>We run the script and wait.</p>
<p align="center">
<img src="https://i.imgur.com/i7NyTe8.png" height="80%" width="80%" alt="enter role name"/>
<img src="https://i.imgur.com/qv4o8QB.png" height="80%" width="80%" alt="enter role name"/>
</p>
<br />
<p>And after a minute or two we have a over 1000+ users. We can also find the user we added prior to running the script.</p>
<p align="center">
<img src="https://i.imgur.com/QScWi3w.png" height="80%" width="80%" alt="enter role name"/>
<img src="https://i.imgur.com/WxY7I8b.png" height="80%" width="80%" alt="enter role name"/>
</p>
<br />


</details>   
<hr/>
<details>
 <summary><b>- Connecting Client to Server</b></summary>
 <p align="center">
<b>Now we finally get to use our client virtual machine!</b>
</p>
<p>Here we log onto our client PC admin account. Right now we took the server down just to show that the client PC cannot connect to the web on its own.</p>
<p align="center">
<img src="https://i.imgur.com/yujKGtF.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/pLzUiet.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>We put the server back online and reattempt to connect to the internet, and it works. Our client vm is able to access the internet using the server. Our infrastructure is operational!</p>
<p align="center">
<img src="https://i.imgur.com/GkDSTkv.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/tPthojf.png" height="50%" width="50%" alt="enter role name"/>
</p>
<p>Time to change the PC name and add ourselves to the domain using one of the 1000+ users we created. Changes will require a reboot to take effect.</p>
<p align="center">
<img src="https://i.imgur.com/A2n2RJg.png" height="80%" width="80%" alt="enter role name"/>
<img src="https://i.imgur.com/3bhzc7w.png" height="80%" width="80%" alt="enter role name"/>
</p>
<br />
<p>Now we login using the account we added ourselves as. Once the user account is built we are ready to go with internet access.</p>
<p align="center">
<img src="https://i.imgur.com/vKfKJTP.png" height="80%" width="80%" alt="enter role name"/>
<img src="https://i.imgur.com/VvsM91S.png" height="80%" width="80%" alt="enter role name"/>
</p>
<br />
<p>From the server side we can see our client PC, their IP address, and more.</p>
<p align="center">
<img src="https://i.imgur.com/GAMq2gL.png" height="80%" width="80%" alt="enter role name"/>
</p>
</details>   
<hr/>
<br />
