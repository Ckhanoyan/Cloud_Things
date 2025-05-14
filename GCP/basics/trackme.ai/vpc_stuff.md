# GCP VPC Design Checklist for trackme.ai

```
+--------------------------------------------------------------+
|                          VPC: trackme-ai                     |
|  CIDR: 10.0.0.0/16                                           |
|                                                              |
|  +--------------------+      +--------------------------+    |
|  | Public Subnet      |      | Private Subnet           |    |
|  | CIDR: 10.0.1.0/24  |      | CIDR: 10.0.2.0/24        |    |
|  |                    |      |                          |    |
|  |  +-------------+   |      |  +-------------------+   |    |
|  |  | API Server  |   |      |  | Database Server   |   |    |
|  |  | (VM w/ ext  |   |      |  | (VM w/ int only)  |   |    |
|  |  | IP)         |   |      |  +-------------------+   |    |
|  |  +-------------+   |      |                          |    |
|  +------|-------------+      +------------|-----------+ |
|         |                               NAT Gateway          |
|         | (For Private Subnet Internet Access)               |
|         +---------------+-----------------+                  |
|                         |                                    |
|                   Internet Gateway                           |
|                     (Public Access)                          |
+--------------------------------------------------------------+
```

## **1. Subnet Design**
- [ ] **Choose Private and Public Subnets**
  - **Private Subnets**: For backend services (e.g., databases, application servers) that don't need direct internet access.
  - **Public Subnets**: For frontend services (e.g., load balancers, bastion hosts) that require internet access.
  - **Best Practice**: Use **regional subnets** for high availability and scalability.
  - **Use Case for trackme.ai**: Place an API server on a public subnet to handle user traffic, while storing sensitive data in a private subnet.
  - [Related File: vpc_stuff.md](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/vpc_stuff.md)

- [ ] **Subnet IP Addressing (CIDR Blocks)**
  - Assign **non-overlapping CIDR ranges** (e.g., `10.0.0.0/16`) to avoid conflicts during future VPC peering.
  - Allocate smaller subnets (e.g., `/24`) for each availability zone based on resource requirements.
  - **Best Practice**: Plan for growth and avoid using the entire CIDR block upfront.
  - **Use Case for trackme.ai**: Assign `10.0.1.0/24` for public subnet and `10.0.2.0/24` for private subnet.

---

## **2. IP Address Management**
- [ ] **Reserve Static Internal/External IPs**
  - Use **static internal IPs** for critical backend VMs (e.g., database servers) to ensure persistent connectivity.
  - Use **external IPs** sparingly and only for resources that need to be internet-facing.
  - **Best Practice**: Use **Private Google Access** to allow private resources to access Google APIs without external IPs.
  - **Use Case for trackme.ai**: Enable Private Google Access for backend services hosted in the private subnet.

---

## **3. Firewall Rules**
- [ ] **Ingress Rules**
  - Define rules to allow traffic **into** resources based on source IP, ports, and protocols.
  - Example:
    - Allow SSH (`tcp:22`) only from trusted IPs (e.g., your office IP).
    - Allow HTTP/HTTPS (`tcp:80`, `tcp:443`) from `0.0.0.0/0` for public-facing resources.
  - **Use Case for trackme.ai**: Allow traffic to an API server from the internet while restricting access to internal services.

- [ ] **Egress Rules**
  - Define rules to allow traffic **out** of resources.
  - Example:
    - Allow all outbound traffic to the internet (`0.0.0.0/0`) for public resources.
    - Restrict outbound traffic from private resources to specific destinations (e.g., Google APIs or other internal IPs).
  - **Best Practice**: Use **least privilege** for both ingress and egress rules.

---

## **4. Routing**
- [ ] **Create Custom Routes**
  - Define routes to direct traffic between subnets and to/from the internet.
  - Example:
    - Add a route to the **internet gateway** for public subnet traffic.
    - Add a route to a **NAT gateway** for private subnet traffic to access the internet securely.
  - **Use Case for trackme.ai**: Use a NAT gateway for backend servers to fetch updates without exposing them to the internet.

```
+--------------------------------------------+
|         Internet                           |
|         (0.0.0.0/0)                        |
+----------------|----------------------------+
                 | Ingress Rule: Allow        |
                 | HTTP/HTTPS traffic         |
                 | (tcp:80, tcp:443)          |
                 v
+-------------------------------+             |
| Public Subnet (10.0.1.0/24)   |             |
|                               |             |
| +---------------------------+ |             |
| | API Server                |<--------------| Egress Rule:
| | External IP               | |             | Allow outbound
| +---------------------------+ |             | traffic to the
|                               |             | internet (0.0.0.0/0)
+-------------------------------+
                 |
                 | Internal Traffic Rule:
                 | Allow traffic to private
                 | subnet (10.0.2.0/24)
                 v
+-------------------------------+
| Private Subnet (10.0.2.0/24)  |
|                               |
| +---------------------------+ |
| | Database Server           | |
| | Internal IP               | |
| +---------------------------+ |
|                               |
| Egress Rule:                 |
| Allow outbound traffic       |
| to internet via NAT Gateway  |
+-------------------------------+
                 |
                 v
        +-------------------+
        | NAT Gateway       |
        |                   |
        +-------------------+
                 |
                 v
       +---------------------+
       | Internet Gateway    |
       +---------------------+
```

---

## **5. Best Practices for trackme.ai**
- [ ] **Enable VPC Flow Logs**
  - Use VPC Flow Logs to monitor traffic to/from VMs for security and troubleshooting.
  - **Use Case for trackme.ai**: Identify suspicious traffic patterns or troubleshoot connectivity issues.
  - **Best Practice**: Send logs to **Cloud Logging** for centralized analysis.

- [ ] **Leverage VPC Peering or Shared VPC**
  - If trackme.ai needs to interact with other projects, use **VPC Peering** or **Shared VPC** for secure inter-project communication.
  - **Best Practice**: Use Shared VPC for centralized management of networking resources.

- [ ] **Use Cloud NAT for Private Subnets**
  - Allows instances without external IPs to access the internet securely.
  - **Use Case for trackme.ai**: Backend services in private subnets can fetch security patches or interact with external APIs.

- [ ] **Restrict SSH Access**
  - Use **Identity-Aware Proxy (IAP)** for secure, audit-logged SSH access instead of public IPs.
  - **Use Case for trackme.ai**: Developers can securely access VMs without exposing SSH to the internet.

- [ ] **Implement Network Tagging**
  - Use network tags to apply specific firewall rules to groups of VMs.
  - **Use Case for trackme.ai**: Tag all backend VMs with `backend-tag` and apply restrictive ingress/egress rules.

---

## **Real-World Scenario**
Imagine trackme.ai is a SaaS platform that tracks user activity for analytics:
- **Frontend**: Hosts a web app on a public subnet with a global HTTP(S) load balancer.
- **Backend**: Processes and stores data in a private subnet. It communicates with the frontend using internal IPs and with Google APIs via Private Google Access.
- **Security**: Minimizes attack surface by using least-privilege firewall rules and restricting SSH access with IAP.
