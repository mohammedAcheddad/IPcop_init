# IPCop Workshop Report

## Introduction

This workshop report provides an overview and detailed instructions on setting up and configuring IPCop, an open-source Linux-based firewall distribution. It covers the architecture used, firewall settings, proxy settings, and various other aspects of IPCop.

## IPCop Overview

IPCop is a Linux-based firewall distribution that aims to provide secure networking and firewall capabilities for small to medium-sized networks. It offers a range of features, including network address translation (NAT), virtual private networking (VPN), proxy server, intrusion detection system (IDS), and more. IPCop is known for its user-friendly web-based interface, making it accessible even for those without advanced technical knowledge.

## Architecture

In this workshop, the following architecture was used:

-   **RED Interface**: The RED interface represents the external interface connected to the internet and uses NAT to translate internal IP addresses to the external IP address.
-   **GREEN Interface**: The GREEN interface represents the internal LAN network and connects to the internal devices.
-   **ORANGE Interface**: The ORANGE interface represents the DMZ network, providing an additional layer of security for servers accessible from the internet.

***in our case:***
-   **RED Interface**: The RED interface is a NAT device because we need internet connection 
it has the IP: **192.168.205.141**

-   **GREEN Interface**: The GREEN interface represents the internal LAN network and connects to the internal devices.
IP: **192.168.1.1** this is the default gateway

-   **ORANGE Interface**: The ORANGE interface in our case is also connected to another LAN segment in which we will give adresses manually to each server (we have only a web server in our example)
IP: **192.168.3.1** --- WEB server IP: **192.168.3.2** 

![WhatsApp Image 2023-05-16 at 18 00 11](https://github.com/mohammedAcheddad/IPcop_init/assets/105829473/745f6041-be95-48bd-a760-5685e0a11ff2)


## Installation

To install IPCop, follow these steps:

1.  Download the IPCop ISO image from the official website.
2.  Create a new virtual machine in VMware, assigning appropriate resources.
3.  Mount the IPCop ISO image to the virtual machine's CD/DVD drive.
4.  Start the virtual machine and follow the on-screen instructions to install IPCop.
5.  During the installation, configure the RED, GREEN, and ORANGE interfaces with appropriate IP addresses and network settings.

## Firewall Settings

IPCop provides a robust firewall configuration to control traffic between different network zones. To configure firewall rules in IPCop, follow these steps:

the Aim is to:
*  Accept ***http*** traffic from green zone to orange to make the web server accessible for the local network.
* Deny all ***https*** traffic from green zone to red zone (internet).
* Deny all ***ping***'s from green zone to the orange zone, to stop attackers that has access to the local network from scanning and accessing the orange zone 

![firewallruleshttps](https://github.com/mohammedAcheddad/IPcop_init/assets/105829473/81e270b9-8184-4365-81d2-b24b7b3a8563)

1.  Access the IPCop web interface by entering the IP address 192.168.1.1 (GREEN interface) in a web browser.
    
2.  Navigate to `Firewall -> Firewall Rules`.
    
3.  Create a new rule to allow HTTP traffic from the GREEN network (192.168.1.0/24) to the ORANGE network (192.168.3.0/24), specifically to the web server with the IP address 192.168.3.2 on the ORANGE network.
    
    -   Source: GREEN (192.168.1.0/24)
    -   Destination: ORANGE (192.168.3.2)
    -   Protocol: TCP
    -   Destination Port: 80 (HTTP)
4.  Create another rule to deny HTTPS traffic from the GREEN network (192.168.1.0/24) to the RED interface.
    
    -   Source: GREEN (192.168.1.0/24)
    -   Destination: RED
    -   Protocol: TCP
    -   Destination Port: 443 (HTTPS)  
    
 5.  Create another rule to deny ***ICMP*** traffic from the GREEN network (192.168.1.0/24) to the ORANGE network (192.168.3.0/24)
	    
	    -   Source: GREEN (192.168.1.0/24)
	    -   Destination: ORANGE (any)
	    -   Protocol: TCP
	    -   Destination Port: pings 
    
6.  Save the firewall rules and ensure that they are correctly ordered to allow the desired traffic.

![ping](https://github.com/mohammedAcheddad/IPcop_init/assets/105829473/d85b6e22-9a57-4e72-8d24-53524e2cdbb1)
![ping2](https://github.com/mohammedAcheddad/IPcop_init/assets/105829473/477aa902-65a2-4a9f-a7b7-bb5d8407fb1c)

## Proxy Settings

IPCop offers a built-in proxy server that allows caching and filtering of web traffic. 

the Aim is to:
*  Block the url of our web server in ***192.168.3.2***

1.  Access the IPCop web interface.
2.  Navigate to `Proxy -> Proxy Settings`.
3.  Configure general proxy settings such as cache size, cache options, and DNS settings.
4.  Customize proxy access controls, such as specifying allowed or denied sites, domains, or URLs.
5.  Save the settings and restart the proxy service for the changes to take effect.

## VPN Configuration

IPCop supports IPSec VPN for secure communication between networks. To configure VPN in IPCop, follow these steps:

I have created a vmnet and machine connected to it with the ip:192.168.37.128 

1.  Access the IPCop web interface.
2.  Navigate to `VPNs -> IPsec`.
3.  Configure IPSec settings, including local and remote subnets, authentication methods, encryption algorithms, and pre-shared keys.
4.  Save the settings and restart the VPN service.
5.  Test the VPN connection by attempting to establish communication between networks or machines.

if the green machine is able to ping the remote machine the connection is good

## DNS Configuration

IPCop provides DNS functionality to resolve domain names for the local network. To configure DNS in IPCop, follow these steps:

I used google DNS and It worked pretty fine

1.  Access the IPCop web interface.
2.  Navigate to `Services -> DNS`.
3.  Configure the DNS settings, including primary and secondary DNS servers.
4.  Specify local domains or overrides if needed.
5.  Save the settings and restart the DNS service.

## Conclusion

This workshop report has provided an overview of IPCop and covered various aspects, including the architecture, firewall settings,
