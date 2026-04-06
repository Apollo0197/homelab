\# Phase 1 - OPNsense Firewall Setup



\*\*Date:\*\* April 5, 2026  

\*\*Status:\*\* Complete



\## Objective

Deploy a virtual firewall using OPNsense on Hyper-V to serve as the 

perimeter of my home lab network, separating lab traffic from my 

host machine and the internet.



\## Environment

\- Hypervisor: Hyper-V on Windows 11

\- VM Specs: Gen 1, 1GB RAM, 20GB disk

\- OPNsense Version: 26.1.2



\## Network Design

| Interface | Virtual Switch | Purpose |

|---|---|---|

| WAN (hn0) | Default Switch | Internet-facing |

| LAN (hn1) | LabNetwork | Internal lab network |



\## What I Did

1\. Created two virtual switches in Hyper-V - Default Switch (external) 

&#x20;  and LabNetwork (internal)

2\. Created OPNsense VM with two NICs attached to each switch

3\. Installed OPNsense 26.1.2 using UFS filesystem

4\. Assigned WAN to hn0 and LAN to hn1

5\. Configured host machine with static IP 192.168.1.2 to access web GUI



\## Problems I Hit

\*\*pfSense bootloader failure\*\* - Originally attempted pfSense but hit 

bootloader issues with ZFS on Hyper-V Gen 1. Switched to OPNsense which 

has better Hyper-V compatibility.



\*\*Partition destroy failed\*\* - Leftover partition data from pfSense 

attempts caused OPNsense installer to fail. Fixed by deleting the old 

VHDX and creating a fresh virtual disk.



\*\*Host couldn't reach web GUI\*\* - Host machine had no IP on LabNetwork. 

Fixed by assigning static IP 192.168.1.2 to vEthernet (LabNetwork) adapter.



\## What I Learned

\- The difference between External, Internal, and Private virtual switches

&#x20; in Hyper-V and when to use each

\- Why a firewall needs two NICs - one facing WAN, one facing LAN

\- How subnets work - both devices need IPs in the same /24 range to 

&#x20; communicate

\- Why Gen 1 VMs have better FreeBSD compatibility than Gen 2 in Hyper-V

\- UFS vs ZFS filesystem tradeoffs for VM environments



\## Result

OPNsense is running with LAN at 192.168.1.1/24 and WAN receiving a 

DHCP address from the Default Switch. Web GUI accessible at 

https://192.168.1.1 from host machine.



\## Next Steps

\- Deploy Windows Server 2022 as Domain Controller at 192.168.1.10

\- Join Windows 10 client to the domain

\- Deploy Wazuh SIEM to ingest logs from AD environment

