<h1> Get Familiar with DNS and DHCP</h1>
<h2>Description</h2>
<p>In this lab, I'll modify existing dnsmasq configuration to adapt it to the DNS and DHCP needs of your organization. Then I will proceed to verify that it works as expected. For the test instance, we have configured a virtual network interface (called eth_srv) which the DNS and DHCP server will listen to. Additionally, we have another virtual network interface that we will use to simulate a client talking to the server and requesting DNS or DHCP traffic. This interface is called eth_cli</p>
<br />
<br />
<h2>Utilities Used</h2>
<ul>
    <li><b>Ubuntu</b></li>
    <li><b>Wireshark</b></li>
</ul>
<br />
<h3>Task 1:</h3>
<p>Enabling debug logging: </p> 
<p align="center">
<img src="https://i.imgur.com/DSmEv7L.png" height="80%" width="80%" alt="pre debug"/>
</p>
<p align="left"> First, I stopped the service. Then I edited the configuration file in Nano.</p>
<br />
<p align="center">
<img src="https://i.imgur.com/2kzYlai.png" height="80%" width="80%" alt="nano"/>
</p>
<p>Then I make sure the syntax is correct in the file and restart the program.</p>
<br />
<p align="center">
<img src="https://i.imgur.com/FvFlf3G.png" height="80%" width="80%" alt="logging"/>
</p>
<br />
<br />
<h3>Task 2:</h3> 
<p>Experimenting with DNS queries:</p> 
<br /> 
<p align="center">
<img src="https://i.imgur.com/OGfnSGY.png" height="80%" width="80%" alt="max"/>
</p>
<p align="left"> I use the command dig, which allows me to request the IP address of a @localhost and make sure that it's logging the queries.</p> 
<br /> 
<br />
<h3>Task 3:</h3>
<p> Experimenting with a DHCP client:</p> 
<p align="center">
<img src="https://i.imgur.com/XWrkobj.png" height="80%" width="80%" alt="calc"/>
</p>
<p align="left"> I run a debugging script to see that the options set in the configuration file were correctly sent to the client</p> 
<br />
<br />
<p align="center">
<img src="https://i.imgur.com/6sZR4mM.png" height="80%" width="80%" alt="calc"/>
</p>
<p align="left"> I first stopped the service, then edit the configuration file once more to set static IP address, reserve the first 20 IPs, and edit the lease time of dynamically set IP address. After saving the configuration file I started the service to reflect the updates.</p> 
<br />
<br />
<p align="center">
<img src="https://i.imgur.com/nvtaU11.png" height="80%" width="80%" alt="calc"/>
</p>
<p> I experimented with DNS queries and modified the configuration of an existing DHCP server. I reserved the IPs to allow administrators to assign specific IP addresses to devices on the network in the future. Now that this testing has been done and works properly in a the test environment it can be rolled out to a production environment. Overall this was good to see how DNS and DHCP servers assign IPs and log queries. More to come as start to wrap up the Google IT support certification.</p> 
