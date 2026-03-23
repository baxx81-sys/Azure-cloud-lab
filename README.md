 Secure Azure Cloud Infrastructure Lab 
A fully operational enterprise-style cloud environment built from scratch in Microsoft Azure — covering identity management, network security, access control, monitoring, and cost governance. 
 About This Project 
This is an independently designed and built Microsoft Azure lab that replicates the core components of a real-world corporate IT infrastructure. Every phase was self-directed — no guided tutorials, no pre-built templates. Just Azure documentation, trial and error, and iteration. 
The goal was to build practical, provable skills in Systems Administration, Identity & Access Management, and Cloud Operations — and document the entire process the way a real IT team would. 
Status: Complete 
Platform: Microsoft Azure 
Duration: 2025 – 2026 
Location: Dallas–Fort Worth, TX 
What I Built 
1. Virtual Machine Deployment & Remote Access 
Provisioned Windows Server 2022 VMs in Azure with correct sizing, region selection, and authentication configuration 
Established secure Remote Desktop Protocol (RDP) access 
Configured Network Interface Cards (NICs) and verified connectivity 
2. Active Directory Domain Services (AD DS) 
Installed and promoted a dedicated Domain Controller with Active Directory Domain Services
Configured a functional corporate domain environment from scratch 
Set up DNS pointing to the DC private IP to enable domain resolution across the network 
3. Domain Join & Group Policy Objects (GPOs) 
Joined a second VM to the domain 
Created and applied Group Policy Objects across all domain-joined machines Verified policy inheritance and application using gpresult /r and rsop.msc 
4. Network Security Groups (NSGs) + Bastion + JIT Access Designed NSG inbound and outbound rules applying least-privilege principles Deployed Azure Bastion — eliminated public RDP exposure entirely (port 3389 closed) 
Configured Just-in-Time (JIT) VM access with IP restrictions and time-bound access windows 
Verified attack surface reduction via effective security rules audit 
5. Azure Monitor & Alerting 
Enabled guest-level diagnostic settings on all VMs 
Created CPU threshold alert rules tied to action groups 
Configured end-to-end email alerting pipeline and verified delivery 
Explored Log Analytics workspace integration 
6. Budget Alerts & Cost Management 
Configured multi-tier budget alerts in Azure Cost Management 
Set both actual spend and forecasted threshold triggers 
Implemented governance controls to prevent billing overruns in a live subscription 
Architecture Overview 
┌─────────────────────────────────────────────────────┐ 
│ Azure Subscription │ 
│ │ 
│ ┌─────────────────────────────────────────────┐ │
│ │ Virtual Network (VNet) │ │ 
│ │ │ │ 
│ │ ┌──────────────┐ ┌──────────────┐ │ │ 
│ │ │ VM-1 │ │ VM-2 │ │ │ 
│ │ │ Domain Ctrl │◄──►│ Domain Join │ │ │ 
│ │ │ AD DS / DNS │ │ GPO Client │ │ │ 
│ │ └──────────────┘ └──────────────┘ │ │ 
│ │ │ │ │ │ 
│ │ ┌──────▼───────────────────▼──────┐ │ │ 
│ │ │ Network Security Group │ │ │ 
│ │ │ Inbound rules (least-privilege)│ │ │ 
│ │ └─────────────────────────────────┘ │ │ 
│ │ │ │ 
│ │ ┌──────────────────────────────────┐ │ │ 
│ │ │ AzureBastionSubnet │ │ │ 
│ │ │ Azure Bastion (browser RDP) │ │ │ 
│ │ └──────────────────────────────────┘ │ │ 
│ └─────────────────────────────────────────────┘ │ 
│ │ 
│ ┌─────────────┐ ┌──────────────┐ ┌───────────┐ │ 
│ │Azure Monitor│ │ JIT Access │ │ Budget │ │ 
│ │ CPU Alerts │ │ Time-bounded │ │ Alerts │ │ 
│ └─────────────┘ └──────────────┘ └───────────┘ │ 
└─────────────────────────────────────────────────────┘ 
 Technologies Used
Technology 
Purpose
Microsoft Azure 
Cloud platform — all infrastructure hosted here
Windows Server 2022 
Operating system for both VMs
Active Directory Domain Services 
Identity management and domain environment
Azure Bastion 
Secure browser-based RDP — no public IP required
Just-in-Time (JIT) VM Access 
Time-bound, IP-restricted privileged access
Network Security Groups 
Inbound/outbound traffic control at subnet level
Azure Monitor 
Infrastructure monitoring and automated alerting
Azure Cost Management 
Budget alerts and spend governance







Group Policy Objects 
Policy enforcement across domain-joined machines



Key Outcomes 
Outcome 
Detail
Eliminated open RDP 
Port 3389 closed — replaced with Bastion and JIT access
Enforced domain 
policies 
GPOs verified across multiple machines with gpresult
Built alerting pipeline 
CPU alerts firing end-to-end from Monitor to inbox
Hardened network 
Least-privilege NSG rules — deny-all as default baseline
Controlled cloud spend 
Multi-tier budget alerts on live Azure subscription
Full documentation 
Issue log, task tracker, architecture notes maintained throughout



Challenges & How I Solved Them 
Challenge 1 — VM Could Not Join Domain 
Problem: Second VM threw an access denied error when attempting domain join. Root Cause: The NIC DNS setting was still pointing to Azure’s default DNS, not the Domain Controller’s private IP. 
Fix: Updated the DNS server on the NIC to the DC’s private IP address, restarted the VM, and domain join succeeded. 
Time to resolve: ~45 minutes 
Challenge 2 — Bastion Subnet Rejected 
Problem: Azure refused to create the Bastion resource. 
Root Cause: The subnet was named incorrectly and sized too small (/27 instead of minimum /26). 
Fix: Deleted the subnet, recreated it with the exact name AzureBastionSubnet and a /26 CIDR range.
Time to resolve: ~20 minutes 
Challenge 3 — GPO Not Applying to Second VM 
Problem: gpresult /r showed no GPOs applied on the domain-joined VM. Root Cause: VM had not been restarted after domain join — GPOs apply on startup. Fix: Restarted the VM, logged in with a domain account, ran gpupdate /force , confirmed GPOs applied. 
Time to resolve: ~15 minutes 
Challenge 4 — Alert Not Firing 
Problem: CPU alert rule was configured but no email was received during testing. Root Cause: Action group was created but not linked to the alert rule correctly. Fix: Edited the alert rule, re-attached the action group, stress tested CPU again, alert fired within 5 minutes. 
Time to resolve: ~30 minutes 

What I Learned 
DNS is everything in Active Directory. Most domain join failures trace back to DNS misconfiguration. Always point NICs to the DC private IP before attempting a join. 
Least-privilege is harder than it sounds. Building NSG rules that are genuinely minimal requires understanding what each service actually needs — not just blocking obvious ports. 
JIT and Bastion serve different purposes. Bastion removes the need for a public IP entirely. JIT controls when and who can access a port even when it needs to be open. Both together is the right answer. 
Alerting is only as good as your action group. The monitoring rule and the notification are two separate things — easy to configure one without properly linking the other. 
Documentation while building beats documentation after. Trying to reconstruct what you did from memory is painful. Notes taken in the moment are far more accurate and useful. 
What’s Next 
Expanding lab with AWS — working toward cloud-agnostic infrastructure skills Adding PowerShell scripts to automate AD user creation and GPO reporting Setting up Ubuntu Server VM to build Linux CLI proficiency 
AZ-900 certified
AZ-104 certified 
Portfolio 
A full one-page portfolio document summarising this project is available in this repository: Azure_Cloud_Portfolio.pdf
About Me 
I’m an IT professional based in Dallas–Fort Worth building practical cloud infrastructure skills through hands-on lab work. I’m actively looking for roles in: 
Systems Administration 
Identity & Access Management 
Cloud Operations 
Cloud Support Engineering 
Baxx81@gmail.com 
Built independently in Microsoft Azure — 2025–2026
