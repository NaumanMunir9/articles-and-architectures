# VNet Peering in Hub-and-Spoke Architecture

---

![VNet Peering in Hub-and-Spoke Architecture](../../architecture-diagrams/azure/VNet%20Peering%20in%20Hub-and-Spoke%20Architecture.png)

---

In a hub-and-spoke design, VNet peering allows spoke VNets to efficiently communicate with resources in the hub VNet, such as shared services (e.g., DNS, firewalls, and NVAs) or external networks (e.g., on-premises or the internet). However, the architecture imposes several key configurations and considerations to achieve optimal connectivity between the hub and the spokes, especially when using VPN gateways or routing services in the hub.

A common requirement in hub-and-spoke networks is the ability to allow spoke VNets to utilize a “VPN gateway” or “Azure Route Server” hosted in the hub VNet to connect to on-premises or remote networks. This scenario necessitates specific settings to propagate routes and facilitate proper traffic flow across the peering connections.

1. **Virtual network’s gateway or Route Server (Hub):** This setting is applied on the hub VNet’s peering connection, enabling the hub’s VPN gateway or Route Server to be shared with the peered spoke VNets. Essentially, it instructs the hub to expose its gateway or Route Server to the peered VNets.
2. **The remote virtual network’s gateway or Route Server (Spoke):** This setting is applied on the spoke VNet’s peering connection, allowing the spoke to leverage the gateway or routes provided by the hub. With this configuration, workloads in the spoke VNets can route their traffic through the hub’s VPN gateway to reach external networks or on-premises environments.

By enabling both settings in tandem, organizations can establish seamless connectivity from the spoke VNets to external networks through the centralized gateway in the hub, without the need to deploy separate gateways in each spoke.

Another critical use case in hub-and-spoke architectures is “service chaining”, where traffic from the spoke VNets is routed through a network virtual appliance (NVA) or VPN gateway located in the hub before reaching its destination. Service chaining can be configured without the need for network address translation (NAT), allowing for more transparent and flexible routing policies.

Traffic Forwarding Configuration: To enable traffic forwarding through the hub VNet, the peering relationship must be configured to support traffic redirection through the hub. This allows workloads in the spoke VNets to forward traffic to NVAs, firewalls, or VPN gateways hosted in the hub. Once forwarded, traffic can be inspected, filtered, or routed based on security or policy requirements before reaching its final destination, whether that’s another spoke VNet, an external network, or the internet.

This traffic forwarding capability is vital for scenarios where centralized security policies are enforced via firewalls or NVAs in the hub. It also enables the design of advanced routing topologies where traffic flows between spoke VNets can be monitored or modified without requiring direct peering between the spokes.

Implementing a hub-and-spoke architecture using VNet peering offers several key benefits, both in terms of cost savings and architectural efficiency:

1. **Cost Efficiency:** Centralizing shared services such as VPN gateways, firewalls, and NVAs in the hub VNet eliminates the need to replicate these services across each spoke. This not only reduces infrastructure costs but also simplifies network management by centralizing control in the hub. For example, instead of deploying a separate VPN gateway for each spoke VNet, the hub’s VPN gateway can be shared across all spokes, significantly reducing operational and capital expenditures.
2. **Overcoming Subscription Limits:** Azure imposes limits on the number of gateways, network interfaces, and other resources that can be deployed within a single subscription. By using a hub-and-spoke architecture, organizations can avoid hitting these limits by centralizing gateway and service deployments in the hub, which are then shared across multiple spoke VNets.
3. **Simplified Network Security Management:** The hub can serve as the central point for implementing network security controls, such as firewalls, network security groups (NSGs), and NVAs. By routing all traffic through the hub, organizations can enforce consistent security policies across all spoke VNets, ensuring that workloads in different spokes are subject to the same level of protection. This is particularly beneficial in large-scale environments where decentralized security configurations could lead to inconsistencies or gaps in coverage.
4. **Scalability and Flexibility:** The hub-and-spoke architecture supports highly scalable environments, allowing the addition of new spoke VNets without significantly modifying the overall architecture. Each new spoke VNet can be peered with the hub and immediately gain access to shared services and connectivity to external networks. This scalability makes the architecture ideal for multi-team or multi-project environments, where new workloads or environments are frequently spun up or decommissioned.

While VNet peering in a hub-and-spoke architecture is relatively simple to implement, certain routing considerations must be taken into account to avoid unintended network behavior:

1. **Transitive Routing Limitations:** Similar to standard VNet peering, hub-and-spoke architectures do not support transitive routing natively. This means that spoke VNets cannot automatically communicate with each other through the hub unless additional configurations, such as routing via an NVA or a full-mesh peering, are implemented.
2. **Effective Route Tables:** When configuring VNet peering with centralized gateways or traffic forwarding, it’s important to understand how Azure’s system routes and custom routes affect traffic flow. Custom user-defined routes (UDRs) can be used in combination with VNet peering to explicitly define the next-hop for traffic between the hub and spoke VNets. For example, UDRs can direct traffic from a spoke VNet through a firewall in the hub before it is routed to another spoke VNet or to the internet.
3. **Gateway Propagation and Propagation Limits:** When sharing a VPN gateway in the hub with multiple spoke VNets, care must be taken to manage gateway propagation and avoid exceeding Azure’s limits for the number of routes that can be propagated to peered VNets. In large-scale environments, route summarization or selective propagation may be necessary to prevent route table bloat.
