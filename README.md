# Windows Server AD DS Automation

Automated installation and configuration of Windows Server 2019 with Active Directory Domain Services (AD DS) using Vagrant and VirtualBox. One command creates a fully functional Domain Controller — no manual setup required.

## What It Does

1. **Provisions** a Windows Server 2019 VM using a prebuilt Vagrant box (no ISO needed)
2. **Installs** Active Directory Domain Services
3. **Promotes** the server to a Domain Controller
4. **Configures** DNS automatically

```
vagrant up
    ↓
VirtualBox creates Windows Server 2019 VM
    ↓
PowerShell script (set-up.ps1) runs automatically
    ↓
AD DS feature installed
    ↓
New forest created (power.local)
    ↓
Server promoted to Domain Controller
    ↓
Ready to use
```

## What Gets Created

| Component | Value |
|-----------|-------|
| Domain Name | `power.local` |
| NetBIOS Name | `POWER` |
| DNS | Installed automatically |
| Domain Controller | The VM itself |

## Project Structure

```
Windows-server-ad-ds-automation/
├── Vagrantfile          # VM configuration (memory, CPU, network)
├── set-up.ps1           # PowerShell script — installs AD DS and promotes to DC
└── autounattend.xml     # Auto-logon configuration (optional)
```

## Tech Stack

| Tool | Purpose |
|------|---------|
| Vagrant | VM provisioning and management |
| VirtualBox | Virtualization platform |
| PowerShell | AD DS installation and configuration |
| Windows Server 2019 | Server OS (prebuilt Vagrant box) |

## Setup

### Prerequisites

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://developer.hashicorp.com/vagrant/downloads)

### Deploy

```bash
git clone https://github.com/0xShyam-Sec/Windows-server-ad-ds-automation.git
cd Windows-server-ad-ds-automation
vagrant up
```

The VM will boot, run the PowerShell script, and reboot as a Domain Controller. This takes 10-15 minutes depending on your hardware.

### Access the VM

```bash
vagrant rdp
```

Or open VirtualBox and access the VM directly.

**Default credentials:**

| Account | Username | Password |
|---------|----------|----------|
| Vagrant box | `vagrant` | `vagrant` |
| Domain Admin | `Administrator` | Defined in `set-up.ps1` |

> **Important:** Change the default passwords in `set-up.ps1` before using in any non-lab environment.

## Customization

| File | What to Change |
|------|---------------|
| `set-up.ps1` | Domain name (`-DomainName`), admin password, NetBIOS name |
| `Vagrantfile` | VM name, memory (`vb.memory`), CPUs (`vb.cpus`), network settings |
| `autounattend.xml` | Auto-logon settings, hostname |

## Troubleshooting

| Problem | Solution |
|---------|----------|
| VM fails to boot | Ensure VirtualBox and Vagrant are installed and up to date |
| Provisioning error | Run `vagrant reload --provision` |
| AD DS setup fails | Check `set-up.ps1` for correct domain name and password format |
| Cannot RDP into VM | Ensure RDP is enabled or use `vagrant rdp` |
| Need static IP | Edit `config.vm.network` in `Vagrantfile` |

## Cleanup

```bash
vagrant destroy -f
```

## License

MIT
