Scenario: NovaTech's CTO says: "VNet Peering is too expensive for our use case. Can we just use public IPs on both VMs and communicate over the internet?" What are the security and performance risks? What alternative to peering exists for hybrid connectivity?
Answer : Using public IPs instead of VNet peering exposes your VMs to internet attacks and unstable performance, while a safer alternative for hybrid connectivity is an Azure VPN Gateway (or ExpressRoute for enterprise‑grade private links).



Scenario: Your Load Balancer distributes traffic to 2 VMs, but during peak hours the web application slows down. The CEO asks for automatic scaling. What Azure service would you recommend instead of manually adding VMs? How would autoscale rules work?
Answer : Instead of giving VMs public IPs, which is insecure and unreliable, use Azure VPN Gateway (or ExpressRoute for private links) as a safer alternative to VNet peering for hybrid connectivity.


Scenario: A developer stores the database connection string directly in their application code and pushes it to GitHub. How does Key Vault prevent this? Describe the flow: application → Key Vault → secret.
Answer : Key Vault prevents secrets from being exposed by storing the database connection string securely, so instead of hard‑coding it, the application requests the secret from Key Vault, which returns it only to authorized apps, keeping sensitive data out of the code and GitHub.


Scenario: The security team asks you to prove that no unauthorized traffic reached the DataSubnet in the last 30 days. Which Azure feature provides this data? What tool would you use to analyze it?
Answer : You would use Network Security Group (NSG flow logs) to capture traffic data, and then analyze it with Log Analytics / Azure Monitor to prove no unauthorized traffic reached the DataSubnet in the last 30 days.


Scenario: NovaTech wants to deploy a small API in Europe but doesn't want to manage VMs. You chose ACI. A colleague suggests Azure Kubernetes Service (AKS) instead. When would ACI be the wrong choice and AKS the right one?
Answer : ACI is wrong when you need to run many containers with orchestration, scaling, and updates, and AKS is right because it manages clusters, load balancing, and rolling upgrades automatically.
