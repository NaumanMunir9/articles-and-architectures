# Subnetting Strategies in Azure Virtual Networks (VNets)

[Subnetting Strategies in Azure Virtual Networks (VNets) - LinkedIn](https://www.linkedin.com/pulse/subnetting-strategies-azure-virtual-networks-vnets-nauman-munir-v9qdf/?trackingId=5688fj4mR3yFjhIbZp6qlw%3D%3D)

---

> Subnetting Strategies in Azure Virtual Networks (VNets)
![Subnetting Strategies in Azure Virtual Networks (VNets)](../../architecture-diagrams/azure/Subnetting%20Strategies%20in%20Azure%20Virtual%20Networks%20(VNets).png)

---

In Azure Virtual Networks (VNets), subnets are critical components for achieving network segmentation and enhancing security through isolation. By dividing a VNet into multiple subnets, you create logical perimeters that help organize and manage different types of workloads, such as separating web services from data services. This segregation not only improves security but also optimizes network performance by controlling traffic flow and applying tailored security policies to each subnet.

Consider a scenario where a VNet is divided into two subnets: a "Web Tier Subnet" dedicated to web services and a "Data Tier Subnet" for data services. This configuration allows for precise control over how traffic is routed between these subnets. Azure Route Tables (UDRs) can be employed to direct traffic through specific routes, ensuring that only necessary communications occur between the subnets. For instance, you might route all traffic between the Web Tier and Data Tier through a network virtual appliance (NVA) for inspection or logging.

Additionally, Azure Network Security Groups (NSGs) or NVAs can be applied to each subnet to enforce strict inbound and outbound traffic rules. By defining these rules, you can limit exposure to potential threats and prevent unauthorized access, aligning with the Zero Trust security model. This approach minimizes the risk of lateral movement within the network, significantly reducing the impact of a potential security breach.

Azure VNets can accommodate up to 3,000 subnets, each requiring a unique IP address range that falls within the VNet's defined IP space. Overlapping IP ranges between subnets are not permitted, as this would lead to routing conflicts. For example, in a VNet with an IPv4 address space of 10.16.0.0/16, creating one subnet with a range of 10.16.1.0/24 and another with 10.16.1.0/25 would result in an error due to overlapping address spaces.

A User has 250 usable IP addresses within the subnet (10.16.0.4 – 10.16.0.254). Given these reservations, the smallest supported IPv4 subnet prefix in Azure is /29, which provides three usable IP addresses after the system reserves the first five.

For IPv6, Azure supports a standard address prefix of /64, in accordance with IETF guidelines. This prefix is the smallest subnet that supports auto-configuration. Any attempt to use a prefix larger than /64 for IPv6 will result in an error.

When designing subnets within an Azure VNet, it’s crucial to plan for future scalability. Avoid using the entire address space for existing workloads, as this leaves no room for expansion. Reserving additional IP space within subnets ensures that you can accommodate new resources as your network grows. This foresight is particularly important for services like Azure Virtual Machine Scale Sets (VMSS), which may require dynamic scaling to handle fluctuations in demand.

Modifying the IP address range of an existing subnet with deployed workloads is a complex and disruptive process. It necessitates the removal of all resources within the subnet, which can lead to significant downtime and operational challenges. To avoid this, carefully plan your subnet architecture from the outset, considering both current and future requirements.
