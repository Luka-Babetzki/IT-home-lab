# Home Lab Overview

## Introduction
This repository serves as the central hub for my home lab environment - a VMware-based infrastructure designed to develop practical skills across networking, system administration, security operations, and offensive security.

## Lab Environment
**Hypervisor:** VMware Workstation Pro 17.x  
**Network:** VMnet8 (NAT) - 192.168.10.0/24  
**Host OS:** Windows 11

## Network Topology
```
┌────────────────────────────────────────────────────────────────────────┐
│                          VMnet8 (NAT Network)                          │
│                          192.168.10.0/24                               │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  [Windows 11]    [Ubuntu 22.04 LTS Desktop]    [Windows Server 2022]   │
│  192.168.10.x    192.168.10.x                  192.168.10.x (AD/DS)    │
│                                                                        │
│  Future additions: Splunk SIEM, Kali Linux, etc.                       │
└────────────────────────────────────────────────────────────────────────┘
```
*Diagram will be updated as infrastructure expands*

## Phase 1: Networking
*Building foundational networking skills and understanding how systems communicate within a domain environment. Focus on remote administration, basic troubleshooting, and helpdesk workflows.*

**Related Projects:**
- [Ticketing with Peppermint](https://github.com/Luka-Babetzki/ticketing-peppermint)
- [Active Directory & User Management]()

## Phase 2: Security Operations
*Implementing monitoring and detection capabilities to understand how security events are logged, analysed, and responded to.*

**Related Projects:**
- Splunk SIEM Deployment (WIP)

<br>

---

## Getting Started:
The baseline infrastructure for all projects:

### VMware Workstation Pro Installation
1. Navigate to [Broadcom Support Portal](https://support.broadcom.com)
2. Select VMware Workstation Pro > Version 17.x
3. Download installer for your host OS
4. Run installer with default settings
5. Obtain free licence key for personal/educational use

### Operating System ISOs
- [Windows 11 Enterprise](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise)
- [Windows Server 2022 Standard](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)
- [Ubuntu 22.04 LTS Desktop](https://ubuntu.com/download/desktop)
- [Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines)

### NAT Network Configuration
1. Open VMware Workstation Pro
2. Edit > Virtual Network Editor
3. Verify VMnet8 (NAT) exists with desired subnet (default: 192.168.10.0/24)
4. For each VM: Settings > Network Adapter > NAT
5. Power on VMs - DHCP will assign addresses automatically
6. Test connectivity with `ping` between VMs

## Troubleshooting
### ICMP Connectivity Issues
**Windows → Linux:** Disable Ubuntu firewall (`sudo ufw disable`) or allow ICMP:
```bash
sudo ufw allow from 192.168.10.0/24
```

**Linux → Windows:** Windows Defender blocks ICMP by default. Create inbound rule in Windows Defender Firewall to allow ICMPv4.

## Additional Resources
- [VMware Workstation Pro Documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/)
- [VMware NAT Networking Guide](https://docs.vmware.com/en/VMware-Workstation-Pro/17/com.vmware.ws.using.doc/GUID-89311E3D-CCA9-4ECC-AF5C-C52BE6A89A95.html)

## 📹 Demonstration


---
**Last Updated:** December 2025