# VNet Peering in Azure: Architecture, Use Cases, and Advanced Considerations

---

> VNet Peering in Azure
![VNet Peering in Azure](../../architecture-diagrams/azure/VNet%20Peering%20in%20Azure.png)

---

VNet peering is a foundational Azure networking feature that facilitates secure and efficient connectivity between Azure Virtual Networks (VNets), whether within the same Azure region (regional peering) or across different regions (global peering). This capability enables VNets to communicate seamlessly without the need for additional network appliances, such as gateways, and without routing traffic over the public internet. Instead, it leverages Microsoft's high-bandwidth, low-latency backbone infrastructure, ensuring that the traffic flow between workloads remains efficient and secure.

Given that VNet peering supports peering across Azure subscriptions, Azure Active Directory tenants, and even different address families (IPv4 and IPv6), its flexibility and ease of implementation make it an attractive solution for many enterprise-grade architectures. However, despite its simplicity, certain architectural considerations and limitations must be understood to effectively leverage this feature in complex network designs.

The primary advantage of VNet peering is that the traffic between peered VNets uses Azure’s internal backbone network rather than the internet or external gateways, offering several key benefits:

1. **Low Latency and High Throughput:** Traffic between peered VNets traverses Azure’s backbone network, avoiding the need for public internet routing. This guarantees low latency and high bandwidth, which is crucial for latency-sensitive and high-throughput applications.
2. **Ease of Implementation:** VNet peering can be established in seconds without requiring additional network resources such as virtual network gateways or NAT appliances. This reduces the operational complexity associated with connecting networks, allowing for rapid deployment of interconnected VNets.
3. **Cross-Subscription and Cross-Tenant Connectivity:** VNet peering supports both intra- and inter-subscription peering, as well as cross-Azure AD tenant peering. This flexibility is particularly useful for multi-team or multi-business unit environments that require isolation at the subscription or tenant level, while still enabling secure network communication between VNets.
4. **Dual Stack Support:** VNet peering supports address spaces that use IPv4 only or dual-stack configurations (IPv4 and IPv6). This ensures compatibility with modern networking standards and enables transition strategies for organizations migrating to IPv6.

VNet peering is suitable for a wide range of network scenarios:

1. **Hybrid Cloud Architectures:** VNet peering is frequently used in hybrid cloud architectures where different Azure VNets represent separate environments, such as development, staging, and production, or where VNets are located in different regions for disaster recovery and high availability purposes.
2. **Multi-Region Architectures:** Global VNet peering allows organizations to distribute workloads across Azure regions for resilience, load balancing, and optimized user experiences. Traffic between globally peered VNets continues to use the Microsoft backbone, ensuring low latency and high throughput despite geographical separation.
3. **Multi-Tenant Cloud Architectures:** Cross-tenant VNet peering enables secure communication between different organizational units, where each unit operates its resources in separate Azure Active Directory tenants. This is especially useful in scenarios like mergers, acquisitions, or collaboration across partner organizations.
4. **Hub-Spoke Network Architectures:** VNet peering can be used to implement hub-spoke architectures where a central hub VNet provides shared resources, such as security appliances or firewalls, while spoke VNets handle individual workloads. Peering enables efficient communication between the hub and the spokes without the complexity of managing multiple gateways.

Despite the ease of implementation, several prerequisites must be met before establishing a peering relationship between VNets:

1. **Non-Overlapping Address Spaces:** VNets involved in a peering relationship must have non-overlapping IP address spaces. This prevents routing conflicts that could arise from address space duplication. If overlapping address spaces exist, a peering connection cannot be established.
2. **Permissions Required:** The permissions to configure VNet peering are governed by role-based access control (RBAC). Specifically, the “Network Contributor” role or a custom role with the following permissions is required to establish peering. Since VNet peering must be established from both ends of the connection, the necessary permissions must be granted to both the source and target VNets.
3. **No Connectivity Downtime:** One of the operational advantages of VNet peering is that it introduces no downtime when creating or modifying peering relationships. However, establishing the peering connection will modify the effective route tables of both VNets. This automatic routing update ensures that traffic between the two VNets is handled through the peered connection.

While VNet peering is straightforward to implement, several architectural considerations should be kept in mind to ensure optimal scalability, performance, and security:

1. **Address Space Modifications:** One of the more recent improvements to VNet peering is that address spaces of the peered VNets can be modified without disrupting the peering connection. This is particularly beneficial for growing organizations that need to expand or resize their network address spaces without incurring downtime.
2. **Performance and Latency:** VNet peering does not introduce any additional latency for regional peering. Traffic between workloads in peered VNets within the same region remains as performant as if they were in the same VNet. Global peering similarly benefits from Azure’s optimized backbone infrastructure, but organizations should account for natural latency variations due to geographical distance in multi-region architectures.
3. **Network Security:** Security controls such as Network Security Groups (NSGs) can still be applied to subnets within peered VNets to enforce granular control over traffic flow. For example, administrators can block specific types of traffic or restrict access between subnet resources, even within peered VNets. This ensures that the flexibility offered by peering does not come at the cost of security.

One of the primary limitations of VNet peering is the lack of support for transitive routing. In other words, VNet peering is not a transit gateway; traffic cannot automatically flow between VNets that are not directly peered, even if they share a mutual peering relationship with an intermediary VNet.

Consider a scenario with three VNets: “CoreServices VNet”, “Engineering VNet”, and “Research VNet”. If CoreServices is peered with Engineering, and Engineering is peered with Research, the peering relationships are not transitive. This means that CoreServices cannot communicate with Research through Engineering. If direct communication between CoreServices and Research is needed, an explicit peering connection must be established between the two, resulting in a full-mesh topology.

However, managing peering relationships in large environments with many VNets can quickly become complex and may exceed Azure’s limit of 500 peering connections per VNet. As an alternative, a “network virtual appliance (NVA)” with routing capabilities can be deployed to act as an intermediary for routing traffic between VNets that do not have a direct peering relationship. While this approach introduces additional complexity and potential latency, it provides a scalable solution for large or complex network architectures.
