# IT Home Lab

## Overview
Basic virtualised environment for practising enterprise IT administration. Uses VMware Workstation Pro as the hypervisor of choice, Windows Server 2022, Windows 11, and Ubuntu 22.04 LTS connected via a NAT network.

## Objectives
- Deploy VMware infrastructure, configure networking, establish baseline for future labs
- Understand and compare individual OS internals (file system structure, networking stack, service management)
- Experiment with inter-machine communication to understand how data is transmitted over a network

## Technologies Used
- VMware Workstation Pro
- Windows Server 2022
- Windows 11 Enterprise
- Ubuntu 22.04 LTS

## Architecture

**Network Topology:**
- NAT Network (LabNet): 192.168.138.0/24
- Windows Server 2022: 192.168.138.10
- Windows 11: 192.168.138.11
- Ubuntu 22.04 LTS: 192.168.138.12
- Gateway: 192.168.138.2 (VMware default)

**Virtual Machine Specifications:**
- Windows Server 2022: 2 vCPUs, 2GB RAM, 60GB disk
- Windows 11: 2 vCPUs, 3GB RAM, 60GB disk
- Ubuntu 22.04 LTS: 2 vCPUs, 2GB RAM, 40GB disk

All disks configured with thin provisioning to conserve host storage.

## Implementation Steps

### 1. Download Hypervisor (VMware Workstation Pro)

Downloaded VMware Workstation Pro 17 from Broadcom's support portal (requires free Broadcom account).

- Navigation path: Broadcom Support Portal > VMware Cloud Foundation > My Downloads > VMware Workstation Pro > Select version 17.x
- Installed on Windows host with default settings
- Licence key obtained via Broadcom portal for personal/educational use

### 2. Download and Configure Host OS ISOs

Downloaded ISOs for all three operating systems:

**Windows Server 2022:**
- Downloaded ISO from Microsoft Evaluation Centre
- Created VM with 2 vCPUs, 2GB RAM, 60GB disk
- Installed Server 2022 Standard
- Configured hostname: `WINAD-SRV`

**Windows 11:**
- Downloaded Windows 11 Enterprise evaluation ISO
- Created VM with 2 vCPUs, 4GB RAM, 60GB disk
- Install Windows 11 Enterprise version
- Configured hostname: `WIN11-CLIENT`

**Ubuntu 22.04 LTS:**
- Downloaded Ubuntu 22.04.3 LTS Desktop ISO from Ubuntu's official site
- Created VM with 2 vCPUs, 2GB RAM, 40GB disk
- Installed with default packages
- Configured hostname: `ubuntu-client`

**Challenge encountered:** Windows 11 initially failed installation due to TPM 2.0 requirement. Resolved by modifying registry during setup (Shift+F10 to open CMD, added `LabConfig` registry keys to bypass checks).

### 3. Add All 3 Clients to the Same NAT Network

Configured all VMs to use VMware's NAT network (VMnet8) for shared subnet connectivity whilst maintaining internet access through host machine.

**Configuration steps:**
- Opened VMware Virtual Network Editor (Edit > Virtual Network Editor)
- Verified VMnet8 (NAT) settings: subnet 192.168.10.0/24
- For each VM: VM Settings > Network Adapter > NAT (shared with host)
- Ensured all VMs received DHCP addresses in same subnet

**Decision rationale:** NAT network chosen over bridged mode to isolate lab traffic from physical network whilst maintaining internet connectivity for updates and package downloads.

### 4. Test Client Connectivity

Verified both internet connectivity and inter-VM communication for all three machines.

#### Testing Internet Connectivity

**Windows Server 2022:**
```cmd
ping 8.8.8.8
nslookup google.com
```

**Windows 11:**
```cmd
ping 8.8.8.8
nslookup google.com
```

**Ubuntu 22.04 LTS:**
```bash
ping 8.8.8.8
```

All machines successfully resolved DNS and reached external hosts.

#### Testing Inter-VM Connectivity

Identified IP addresses for each machine:
- Windows Server 2022: `ipconfig`
- Windows 11: `ipconfig`
- Ubuntu: `ip addr show`

**Tested connectivity:**
- From Windows 11 to Server 2022: `ping <server-ip>`
- From Ubuntu to Windows 11: `ping <win11-ip>`
- From Server 2022 to Ubuntu: `ping <ubuntu-ip>`

All ICMP echo requests succeeded, confirming layer 3 connectivity between all hosts.

**Challenge encountered:** Initial ping from Windows machines to Ubuntu failed. Resolved by disabling Ubuntu's firewall temporarily (`sudo ufw disable`) to confirm connectivity, then re-enabled with ICMP allowed (`sudo ufw allow from 192.168.138.0/24`).

**Challenge encountered:** Initial ping from Ubuntu to Windows machines failed. Resolved by disabling Windows Defender's (Start > Settings > Update & Security > Windows Security > Virus & threat protection) as ICMP is blocked by default. Tested to confirm connectivity.

## Key Learnings

- **Virtualisation fundamentals:** Understanding resource allocation, thin vs thick provisioning, snapshot functionality for lab state management
- **Network configuration:** How NAT networks function, DHCP address assignment, subnet isolation vs bridged networking
- **Cross-platform connectivity testing:** Different tools and commands for verifying network connectivity across Windows and Linux systems (ping, Test-NetConnection, dig, curl)
- **OS-specific firewall behaviour:** Windows Firewall and Ubuntu's UFW blocks by default ICMP by default on private networks

## Future Enhancements

- [ ] Add network architecture diagram showing VM topology
- [ ] Add screenshots illustrating VM configuration steps
- [ ] Document common troubleshooting steps for connectivity issues
- [ ] Create snapshots of baseline configurations for easy reset

## Resources

- [Download VMware Workstation Pro](https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware+Workstation+Pro)
- [Download Windows Server 2022 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)
- [Download Windows 11 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise)
- [Download Ubuntu 22.04 LTS ISO](https://ubuntu.com/download/desktop)
- [VMware NAT Networking Documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/17/com.vmware.ws.using.doc/GUID-89311E3D-CCA9-4ECC-AF5C-C52BE6A89A95.html)
