# AWS Multi-VPC Peering & Secure Bastion Proxy Architecture

## Project Overview
This project demonstrates the implementation of a secure, isolated multi-VPC architecture on AWS. It connects two distinct Virtual Private Clouds (**VPC A** and **VPC B**) using an AWS VPC Peering connection and enforces secure administrative access via a public **Bastion Host** utilizing SSH `ProxyJump`.

---

## Architecture & Components

* **VPC A (`10.10.0.0/16`):**
  * **Public Subnet:** Hosts the Bastion instance (`ewb-bastion`) with an Internet Gateway (`IGW`) for public ingress.
  * **Private Subnet:** Hosts application instance `ewb-app-a` (`10.10.2.10`).
* **VPC B (`20.20.0.0/16` or `10.20.0.0/16`):**
  * **Private Subnet:** Hosts application instance `ewb-app-b` (`10.20.1.10`).
* **VPC Peering Connection (`ewb-a-b-peer`):** Establishes bidirectional private traffic flow between VPC A and VPC B.
* **Security & Access Control:** 
  * Strict security groups restricting inbound SSH access to the bastion.
  * Private instances protected from direct internet exposure, accessible exclusively via SSH `ProxyJump` through the bastion.

---

## Implementation Steps

1. **VPC Provisioning:** Created VPC A and VPC B with custom CIDR blocks, public/private subnets, and an Internet Gateway.
2. **Peering Setup:** Created and accepted the VPC Peering connection between both VPCs.
3. **Route Table Configuration:** Updated route tables across public and private subnets to route peered traffic seamlessly across the peering link.
4. **Security Group Hardening:** Configured `ewb-sg-bastion`, `ewb-sg-app-a`, and `ewb-sg-app-b` to permit necessary SSH and ICMP traffic.
5. **Bastion Proxy Configuration:** Configured local SSH client settings (`~/.ssh/config`) to utilize `ProxyJump` for securely tunneling into private application servers.

---

## Verification & Testing

Connectivity was successfully verified by proxying into private instance `ewb-app-a` through the bastion host and executing an ICMP ping test to private instance `ewb-app-b` (`10.20.1.10`).

* **Result:** **0% packet loss** with sub-millisecond round-trip times.

<img width="1917" height="867" alt="image" src="https://github.com/user-attachments/assets/6fe490be-16f1-4c0e-b41a-3dc330515c38" />
<img width="1917" height="707" alt="image" src="https://github.com/user-attachments/assets/ee1c6a2c-5e71-4760-9ab9-f744bfc133db" />
<img width="1917" height="627" alt="image" src="https://github.com/user-attachments/assets/8e425884-0d8c-4f45-a5b9-9fcfaf914f5b" />
<img width="805" height="598" alt="image" src="https://github.com/user-attachments/assets/3bf90ed1-627c-4726-aa2e-14282a63e05c" />



