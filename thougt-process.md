 Task 1: Create Europe Resource Group
 Why I Chose This Configuration

I created the resource group in West Europe to support a multi-region architecture and improve availability and latency for users in that region. Using the same tagging strategy as Use Case 1 ensures consistency in governance, cost tracking, and resource organization across environments. Updating the CostCenter to 1002 reflects regional cost allocation for production workloads.

Challenges I Encountered

There were no major technical challenges, but ensuring tag consistency across all resources required attention to detail. It’s easy to miss one tag, which can affect cost tracking and governance policies in real environments.

 How This Builds on Use Case 1

This builds on Use Case 1 by extending from a single-region deployment (US) to a multi-region architecture, which is essential for high availability, disaster recovery, and global scalability.

 Task 2: Create Europe Virtual Network
 Why I Chose This Configuration

I used a non-overlapping address space (172.16.0.0/16) to ensure compatibility with VNet peering. The subnet structure (Web, App, Data) mirrors the US environment to maintain architectural consistency and enforce network segmentation, which improves security and scalability.

 Challenges I Encountered

The key challenge was ensuring that the address space did not overlap with the US VNet (10.0.0.0/16). Overlapping ranges would prevent peering entirely. Careful IP planning was required to avoid future conflicts.

 How This Builds on Use Case 1

This builds on Use Case 1 by replicating the network architecture in another region, enabling cross-region communication and preparing the environment for failover and redundancy.

 Task 3: Configure VNet Peering
 Why I Chose This Configuration

I configured bidirectional peering to allow seamless communication between US and Europe VNets. Enabling forwarded traffic supports advanced routing scenarios, while disabling gateway transit aligns with the lab setup (no VPN gateway). This ensures secure, low-latency communication over Microsoft’s backbone network.

 Challenges I Encountered

One challenge was remembering that VNet peering is not transitive and must be configured from both sides. Additionally, missing settings like “allow forwarded traffic” can silently break connectivity even when peering shows as “Connected.”

 How This Builds on Use Case 1

This builds on Use Case 1 by connecting previously isolated environments, transforming the architecture into a distributed system capable of cross-region communication.

 Task 4: Create Private DNS Zone
 Why I Chose This Configuration

I created a Private DNS Zone to enable name-based communication between resources instead of relying on IP addresses. Enabling auto-registration ensures that VMs automatically register their DNS records, simplifying management and improving reliability.

 Challenges I Encountered

A key challenge was ensuring that both VNets were properly linked with auto-registration enabled. Without this, DNS records would not populate automatically, leading to failed name resolution between resources.

 How This Builds on Use Case 1

This enhances Use Case 1 by introducing service discovery, making the system more maintainable and production-ready by eliminating dependency on static IP addresses.

 Task 5: Deploy Load Balancer with Two VMs
 Why I Chose This Configuration

I used a Standard Load Balancer to distribute traffic across two backend VMs for high availability and fault tolerance. Removing public IPs from the VMs improves security by enforcing access through the load balancer only. The health probe ensures traffic is only sent to healthy instances, improving reliability.

 Challenges I Encountered

Health probe failures were a potential issue — if Nginx wasn’t properly installed or port 80 wasn’t open, the VMs would show as unhealthy, and no traffic would be routed. Also, understanding that Standard LB requires VMs without public IPs required careful configuration.

 How This Builds on Use Case 1

This builds on Use Case 1 by moving from a single VM deployment to a highly available, load-balanced architecture, which is a key requirement for production systems.

 Task 6: Deploy Azure Container Instance
 Why I Chose This Configuration

I used Azure Container Instances (ACI) to quickly deploy a containerized Nginx application without managing infrastructure. ACI is ideal for lightweight, fast deployments and testing scenarios. Assigning a public IP allows easy external access for validation.

 Challenges I Encountered

Networking configuration required attention, especially ensuring the container was accessible via its public IP. Additionally, understanding when to use ACI vs. VMs or AKS required conceptual clarity.

 How This Builds on Use Case 1

This introduces containerization into the architecture, expanding beyond traditional VMs and demonstrating a more modern, flexible deployment approach.

 Task 7: Create Key Vault and Store Secret
 Why I Chose This Configuration

I used Azure Key Vault to securely store sensitive information instead of hardcoding it. Enabling purge protection ensures that secrets cannot be permanently deleted, even accidentally, which is critical for production security.

 Challenges I Encountered

Accessing the secret from the VM required proper authentication setup and Azure CLI installation. Permissions also needed to be correctly configured to allow retrieval of secrets.

 How This Builds on Use Case 1

This enhances Use Case 1 by introducing secure secret management, a critical component of any production-grade system.

 Task 8: Create Log Analytics Workspace & Diagnostics
 Why I Chose This Configuration

I created a Log Analytics Workspace to centralize logs and metrics from multiple resources. Enabling diagnostics on VMs and the VNet provides visibility into system health and network activity, which is essential for monitoring and troubleshooting.

 Challenges I Encountered

There was a delay in log data appearing after configuration. Additionally, ensuring the correct diagnostic settings (especially for VNet flow logs) required careful selection of categories.

 How This Builds on Use Case 1

This builds on Use Case 1 by adding observability and monitoring, transforming the environment into a fully operational, production-ready system.







SECURITY REVIEW
Multi-Region Azure Deployment – Review

This project demonstrates a multi-region setup across East US and West Europe, focusing on availability, security, and cost optimization.

Security (Azure Advisor)

Security scores may differ due to:

More resources in East US (VMs, Load Balancer)
Different exposure levels
Missing or varied configurations (NSGs, diagnostics)

 More complexity = more potential risks.

 Load Balancer Probes
Current: HTTP
More secure: HTTPS (encrypted, verified)

Trade-off: HTTPS is safer but requires SSL setup.

Key Vault Access
Used: Access Policies
Recommended: Azure RBAC

RBAC is more scalable and aligns with best practices.

 NSG Flow Logs
Track network traffic (IP, ports, allow/deny)
Important for security investigations

 Should be enabled in production.

 Container Image (nginx:latest)

Risks:

Unverified source
Vulnerabilities
Unpredictable updates

Best practice:

Use versioned images
Use private registry (e.g., Microsoft Azure Container Registry)
 Cost Optimization

Advisor may suggest:

Resize VMs
Remove unused resources

 Apply carefully — don’t reduce availability or monitoring.

Takeaways
Multi-region improves resilience
Security requires continuous tuning
Balance cost, performance, and security
