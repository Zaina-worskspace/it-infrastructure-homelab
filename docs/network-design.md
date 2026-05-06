# Company profile

## Company name: 
EVO — a small 10 person law firm. 
## Departments:
**Legal team (6 people):**
paralegals and lawyers: They handle client files and need access to the shared file server. No have admin rights on their machines.
**IT/Admin (2 people):**
They manage all systems and have full administrative access.
**Management (2 people, including CEO):**
They have access to all file shares. The work is laptop-based, sometimes it's done remotely.
## Business requirements:
- Employees need a shared internal file storage system for case documents
- The company has a public website (hosted internally in the lab)
- Two employees work remotely and need secure access (VPN)
- Client data is confidential so access must be role-based (only the right people see the right files)
- All systems must be auditable, the company's professional regulations require logs of who accessed what


# Network zones
## Zone 0 — The Firewall 
The firewall (pfSense) that sits between all zones and the internet. It has one virtual network interface per zone (four interfaces). It routes traffic between zones according the rules I set.

## Zone 1 — Internal LAN (192.168.10.0/24)
The private company network. 
Devices here: 
- The Domain Controller (manages all user accounts and policies): 192.168.10.10
- The File Server (where all company documents live): 192.168.10.20
- The employee workstations:
Workstation 1 (Windows client): 192.168.10.101
Workstation 2 (Ubuntu client): 192.168.10.102
Nothing from the outside internet can reach this zone directly. Only the firewall can forward specific, permitted traffic into it, and only from the VPN or DMZ under controlled conditions.

## Zone 2 — DMZ (192.168.20.0/24)
The buffer zone where the company's web server lives. It's reachable from the internet, so clients can visit the website, but it's completely cut off from the Internal LAN (so the client's documents are safe). The firewall enforces this wall between DMZ and LAN.
Devices here:
- Web Server (Nginx): 192.168.20.10

## Zone 3 — Monitoring (192.168.30.0/24)
Where the security monitoring tools are. It can receive logs from all other zones, but nothing in other zones will be able to connect to it uninvited. 
Devices here:
- Wazuh SIEM: 192.168.30.10




