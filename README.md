# Active Directory Home Lab

A self-built Active Directory environment simulating a small company's IT infrastructure — a Domain Controller, a domain-joined client machine, organizational structure (OUs, users, groups), and Group Policy Objects enforcing real configuration and security settings.

Built entirely in VirtualBox on a single host machine to learn and demonstrate core Windows Server / Active Directory administration skills: DNS, domain services, user/group management, and Group Policy.

## Tech Stack
- **Oracle VirtualBox** — virtualization platform
- **Windows Server 2022 (Evaluation, Desktop Experience)** — Domain Controller
- **Windows 11 Pro** — domain-joined client machine
- **Active Directory Domain Services (AD DS)** — directory services, authentication
- **DNS** — hosted on the Domain Controller
- **Group Policy (GPO)** — desktop configuration and security policy enforcement

## Architecture

```
                 Host-Only Network (192.168.56.0/24)
    ┌─────────────────────┐         ┌─────────────────────┐
    │        DC02          │         │       CLIENT01        │
    │  Windows Server 2022 │◄───────►│     Windows 11 Pro     │
    │  192.168.56.10        │         │   Domain-joined client │
    │  Domain Controller    │         │                        │
    │  DNS Server            │         │                        │
    │  Domain: corp.local   │         │                        │
    └─────────────────────┘         └─────────────────────┘
```

**Domain structure:**
```
corp.local
├── IT
│   ├── 2 regular users
│   ├── 1 dedicated admin account
│   └── IT-Admins (security group)
├── Sales
│   └── 2 regular users
└── HR
    └── 2 regular users
```

## What This Demonstrates
- Standing up a Windows Server Domain Controller from scratch (new forest, new domain)
- Configuring DNS to support Active Directory service location (SRV records)
- Joining a client machine to a domain and verifying authentication
- Structuring an organization using Organizational Units, users, and security groups
- Separating privileged (admin) access from standard user accounts
- Creating and applying Group Policy Objects, including troubleshooting real AD-specific limitations (e.g., password policy GPO scope)
- Diagnosing and resolving real infrastructure issues (networking, boot configuration, OS setup, GPO application visibility)

## Project Phases

| Phase | Description |
|---|---|
| [01 - VirtualBox & ISO Setup](docs/01-virtualbox-iso-setup.md) | Host environment and installation media setup |
| [02 - Server VM Creation](docs/02-server-vm-creation.md) | Building and installing the Domain Controller VM |
| [03 - DC Promotion](docs/03-dc-promotion.md) | AD DS installation, forest/domain creation, DNS verification |
| [04 - Client VM Creation](docs/04-client-vm-creation.md) | Building and installing the Windows 11 client VM |
| [05 - Domain Join](docs/05-domain-join.md) | Joining the client to the domain and verifying authentication |
| [06 - OUs, Users & Groups](docs/06-ous-users-groups.md) | Organizational structure and account separation |
| [07 - GPO: Desktop Wallpaper](docs/07-gpo-wallpaper.md) | First Group Policy Object — verified end to end |
| [08 - GPO: Password Policy](docs/08-gpo-password-policy.md) | Second GPO — real AD scoping limitation discovered and resolved |

## Key Takeaways
Building this lab from scratch — including hitting and working through real errors (corrupted disk files, boot configuration issues, DNS diagnostic inconsistencies, and a genuine Active Directory limitation around password policy scope) — reinforced that troubleshooting is as much a part of systems administration as initial setup. Each issue had a specific, diagnosable root cause, and resolving them required verifying assumptions directly (checking DNS records manually, checking GPO application with the correct account/scope) rather than trusting a single tool's output at face value.

## Notes
This is a personal learning project built for skill development and portfolio purposes. It is not connected to any production environment or real organization.
