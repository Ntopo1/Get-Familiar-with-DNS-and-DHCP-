<h1> Get Familiar with DNS and DHCP</h1>
<h2>Description</h2>
<p>In this lab, I'll modify an existing dnsmasq configuration to adapt it to the DNS and DHCP needs of your organization. Domain Name System (DNS) is a system that translates human-readable domain names (like www.example.com) into machine-readable IP addresses (like 192.0.2.1). This process, known as "resolving," enables users to connect to websites and other services on the internet by using easy-to-remember names instead of numeric addresses. Dynamic Host Configuration Protocol (DHCP) is a network protocol used to automatically assign IP addresses and other configuration settings to devices (known as clients) on a network. Dnsmasq is a program that a program that provides DNS, DHCP, TFTP, and PXE services in a simple package. Then I will proceed to verify that it works as expected. For the test instance, we have configured a virtual network interface (called eth_srv) which the DNS and DHCP server will listen to. Additionally, we have another virtual network interface that we will use to simulate a client talking to the server and requesting DNS or DHCP traffic. This interface is called eth_cli.</p>
<br />
<br />
<h2>Utilities Used</h2>
<ul>
    <li><b>Ubuntu</b></li>
    <li><b>Dnsmasq</b></li>
</ul>
<br />
<h3>Task 1: Enabling debug logging</h3>
<p> Check state of that interface and show information about the network configuration.</p> 

```
ip address show eth_srv
```
```
ip address show eth_cli
```
<p>We can see that the eth_srv interface is configured to have the IPv4 addres: 192.168.1.1. The eth_cli interface does not have an IPv4 address yet, but the MAC address is 1a:29:54:14:b1:f6.
<p align="center">
<img src="https://i.imgur.com/DSmEv7L.png" height="80%" width="80%" alt="pre debug"/>
</p>

```
cat /etc/dnsmasq.d/mycompany.conf
```
```
sudo service dnsmasq status
```
```
sudo service dnsmasq stop
```
```
nano /etc/dnsmasq.d/mycompany.conf
```
<p align="left"> The 'cat' command display the configuration file information I checked the status of the server with the 'status' command. Followed up by the 'stop' service command. Then I edited the configuration file in Nano.</p>
<br />

<p align="center">
<img src="https://i.imgur.com/2kzYlai.png" height="80%" width="80%" alt="nano"/>
</p>

```
log-queries
log-facility=/var/log/dnsmasq.log
```
<p>I add the two lines above to allow dnsmasq to start logging and added a file destination for the logs.</p>
<br />  
<p>I make sure the syntax is correct in the file with the 'test command then restart the service.</p>
<br />

```
sudo dnsmasq --test -c /etc/dnsmasq.d/mycompany.conf
```
```
sudo service dnsmasq start
```
<p align="center">
<img src="https://i.imgur.com/FvFlf3G.png" height="80%" width="80%" alt="logging"/>
</p>
<br />
<br />
<h3>Task 2:Experimenting with DNS queries</h3> 
<p>Once the service has restarted I use the 'dig' command, which allows us to request the IP address of a certain hostname.</p> 

```
dig example.com @localhost
```
<br /> 
<p align="center">
<img src="https://i.imgur.com/OGfnSGY.png" height="80%" width="80%" alt="max"/>
</p>

```
sudo tail /var/log/dnsmasq.log
```
<p align="left"> I use the 'tail' command, which allows me to view the end of log files (most recent query).</p> 
<br /> 
<br />
<h3>Task 3: Experimenting with the DHCP client</h3>
<p> I run dhclient, on the eth_cli interface. Additionally, I tell it to run in verbose mode and run a debugging script to see why the eth_cli virtual interface doesn't have an IPv4 addres</p> 

```
sudo dhclient -i eth_cli -v -sf /root/debug_dhcp.sh
```
<p align="center">
<img src="https://i.imgur.com/XWrkobj.png" height="80%" width="80%" alt="calc"/>
</p>
<p align="left"> I first stopped the service, then edited the configuration file once more to set static IP address(1), reserve the first 20 IPs for future use (2) by removing them from the usable pool, and edit the lease time of dynamically set IP address(3) from 24 hours -> 6 hours. </p> 
<br />
(1)

```
dhcp-host=aa:bb:cc:dd:ee:b2,192.168.1.2
dhcp-host=aa:bb:cc:dd:ee:c3,192.168.1.3
dhcp-host=aa:bb:cc:dd:ee:d4,192.168.1.4
```
(2)

```
dhcp-range=192.168.1.20,192.168.1.254
```

(3)

```
6h
```

<br />
<p align="center">
<img src="https://i.imgur.com/6sZR4mM.png" height="80%" width="80%" alt="calc"/>
</p>

<p align="left"> After saving the configuration file I started the service to reflect the updates. We see that the MAC address (shown in the link/ether line) is now set to the one that corresponds to one of the new servers</p> 

```
sudo service dnsmasq start
```
```
sudo dhclient -i eth_cli -v  -sf /root/debug_dhcp.sh
```

<br />
<br />
<p align="center">
<img src="https://i.imgur.com/nvtaU11.png" height="80%" width="80%" alt="calc"/>
</p>
<h3>Conclusion:</h3>
<p> I experimented with DNS queries and modified the configuration of an existing DHCP server. I reserved the IPs to allow administrators to assign specific IP addresses to devices on the network in the future. Now that this testing has been done and works properly in a the test environment, it can be rolled out to a production environment. Overall this was good to see how DNS and DHCP servers assign IPs and log queries. More to come as I start to wrap up the Google IT support certification.</p> 
