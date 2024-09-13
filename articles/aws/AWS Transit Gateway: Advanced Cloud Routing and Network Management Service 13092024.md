# AWS Transit Gateway: Advanced Cloud Routing and Network Management Service

[AWS Transit Gateway: Advanced Cloud Routing and Network Management Service](https://www.linkedin.com/pulse/enhancing-aws-site-to-site-vpn-global-accelerator-nauman-munir-cjxhf/?trackingId=AQuzQxgJQZOrfV9PIlJrlQ%3D%3D)

> AWS Transit Gateway service
![AWS Transit Gateway service](../../architecture-diagrams/aws/AWS%20Transit%20Gateway%20service.png "AWS Transit Gateway service")

AWS Transit Gateway is a powerful and scalable virtual routing service provided by AWS that simplifies the management of networks in the cloud. It acts as a central hub, facilitating communication between multiple Amazon Virtual Private Clouds (VPCs), on-premises networks, and other remote network connections. This regional service is highly available and offers robust scalability, making it a key component for organizations looking to implement complex networking architectures in AWS.

AWS Transit Gateway adopts a hub-and-spoke model that allows you to centralize your network architecture. All VPCs, site-to-site VPNs, and AWS Direct Connect connections can be connected to the Transit Gateway. This design simplifies network management by allowing new networks (VPCs, VPNs, or on-premises connections) to connect via a single attachment to the Transit Gateway. This centralized approach reduces the complexity associated with traditional VPC peering by replacing multiple point-to-point connections with a single managed service.

Traditionally, VPCs in AWS required multiple VPC peering connections for intercommunication. AWS Transit Gateway eliminates this complexity by acting as a central router. All VPCs only need a single connection to the Transit Gateway, allowing seamless VPC-to-VPC communication within a region. This reduces the need for mesh networks and mitigates the limitations imposed by the non-transitive nature of VPC peering.

AWS Transit Gateway can interconnect with other Transit Gateways across different AWS regions, enabling global network architectures. This feature allows you to create a worldwide network that connects all your VPCs, on-premises networks, and remote sites under a single, cohesive framework.
AWS Transit Gateway supports dynamic routing using the Border Gateway Protocol (BGP). BGP allows the Transit Gateway to dynamically learn and propagate routes as networks are added or removed, ensuring efficient and responsive network routing. In addition to dynamic routing, static routes can be configured to provide more control over network traffic flows.

AWS Transit Gateway supports dynamic routing using the Border Gateway Protocol (BGP). BGP allows the Transit Gateway to dynamically learn and propagate routes as networks are added or removed, ensuring efficient and responsive network routing. In addition to dynamic routing, static routes can be configured to provide more control over network traffic flows.

Transit Gateway supports both IPv4 and IPv6 using Multiprotocol BGP (MP-BGP). Even when routing solely IPv6 traffic, IPv4 is required for the BGP peering connection, and IPv6 prefixes are exchanged over the IPv4 BGP peering using MP-BGP. Additionally, AWS Transit Gateway supports the Generic Routing Encapsulation (GRE) protocol for tunnel establishment, providing flexibility for different network configurations.
s
AWS Transit Gateway offers native support for multicast traffic, allowing efficient one-to-many data distribution. This feature is particularly useful for applications like streaming media, live broadcasts, and software updates, where data needs to be sent to multiple recipients simultaneously. Multicast groups are supported within and across VPCs, but not over Direct Connect links.

AWS Transit Gateway provides network segmentation and isolation capabilities using multiple Virtual Router Forwarding (VRF) tables. Each VRF is a separate routing instance that can manage multiple VPCs and VPN connections, allowing for isolated network environments within a single AWS account. This segmentation is useful for creating isolated environments for different teams, departments, or applications.

AWS Transit Gateway integrates seamlessly with AWS Identity and Access Management (IAM) to control access to network resources. You can use IAM policies to manage who can create, modify, or delete Transit Gateway resources. The service also integrates with AWS Network Manager, providing a comprehensive view of your global network, including both AWS and on-premises resources, with detailed metrics for monitoring and troubleshooting.

AWS Transit Gateway supports integration with major SD-WAN vendors, allowing organizations to connect their remote sites using pre-configured VPNs. This flexibility is essential for enterprises that have existing SD-WAN deployments and want to integrate them with their AWS cloud network.

While Transit Gateway provides robust support for VPC-to-VPC and VPC-to-on-premises routing, it does not support transitive routing over peering connections between different Transit Gateways. This means that if you peer two Transit Gateways, you must manually configure static routes across the peering interconnections, as route propagation is not supported in this scenario.

Each Transit Gateway comes with a default route table, but you can create multiple route tables to control how traffic is routed between VPCs, VPNs, Direct Connect gateways, and other networks. Static routes must be manually created in the VPC route tables to direct traffic to the Transit Gateway, as Transit Gateway routes do not automatically populate VPC route tables.

While multicast is supported between VPCs, it is not supported over Direct Connect links. Additionally, specific requirements, such as the use of Internet Group Management Protocol version 2 (IGMPv2), must be met for multicast configurations.

Although AWS Transit Gateway supports both site-to-site VPNs and Direct Connect, there are certain restrictions. For instance, multicast traffic cannot traverse Direct Connect links, and existing site-to-site VPN connections must be reconfigured to integrate with the Transit Gateway for accelerated performance using AWS Global Accelerator.
