# Iris' Homelab
goofball learning IT in a real way
Enterprise Home Lab Infrastructure: Proxmox & QNAP

Project Overview

This repository documents the architecture, deployment, and ongoing administration of a bare-metal home lab environment. Built to mimic enterprise IT infrastructure, this lab serves as a practical sandbox for hypervisor management (Proxmox VE), robust network storage provisioning (QNAP), and network troubleshooting.

This project demonstrates hands-on proficiency in domains critical to IT Support and Systems Administration.

🖥️ Hardware & Network Stack

Compute Layer

Hypervisor: Bare-metal Proxmox VE installed on a Dell PowerEdge server. Got it for $200

Workloads: Configured to host lightweight Linux Containers (LXC) for localized network services (AdGuard) and media management (Plex/Jellyfin environments). Also hosted a successful Palworld server despite memory leak issues.

Storage Layer

NAS Chassis: QNAP TS-453Be (10GbE Expansion via PCIe).

System Drive: Dedicated M.2 NVMe volume for lightning-fast QTS OS operations and application hosting.

NAS Chassis: QNAP TS-251+ (Installation Soon)

Network Topology

Routing & Wireless: TP-Link Deco BE10000 Mesh Network.

Addressing: Static IP allocations for critical infrastructure (Proxmox host, NAS, and DNS endpoints) to ensure reliable local routing and avoid DHCP conflicts.

💾 Storage Architecture

To ensure data isolation and prevent complete array failure from a single degraded drive, the storage architecture was designed using independent Thick Volumes rather than a traditional unified RAID array.

Drive Provisioning:

Isolated 8TB Thick Volume (Primary Data)

Isolated 4TB Thick Volume (Disposable/Temp Data - isolated due to SMART sector warnings but will be replaced later)

Isolated 2TB Thick Volume (Secondary Data)

Network Integration: Configured cross-platform SMB/CIFS network shares. The storage arrays are mapped natively to Windows File Explorer for client-side access and mounted directly to the Proxmox Datacenter for LXC passthrough.

🛠️ Troubleshooting & Incident Logs

Documenting active resolutions to physical and network layer failures.

Incident 01: Catastrophic Network Switch Failure & Broadcast Storm

Symptom: Sudden, critical network timeouts resulting in total loss of connectivity to both the Proxmox hypervisor (10.0.0.50) and the QNAP NAS (10.0.0.113). DNS resolution (via AdGuard) failed simultaneously.

Diagnosis:

Ruled out software-level DNS failure by observing that the timeouts correlated strictly with the initialization of the 10GbE NICs.

Conducted hardware isolation tests by systematically disconnecting ethernet runs.

Traced the failure to the unmanaged 10GbE network switch. Identified severe thermal throttling and internal memory lockup (potentially induced by a broadcast loop or EEE crash), which caused the switch to forcefully drop all connected client traffic.

Resolution:

Executed a physical network bypass, severing the NAS and Hypervisor connections from the failing switch.

Rerouted localized traffic directly into the primary Deco router's backplane.

Result: Restored 100% system uptime and stability, maintaining gigabit transfer speeds while the degraded 10GbE switch hardware undergoes RMA/replacement.
