# AWS Site-to-Site VPN: Secure and Flexible Cloud Connectivity

[AWS Site-to-Site VPN: Secure and Flexible Cloud Connectivity](https://www.linkedin.com/pulse/aws-site-to-site-vpn-secure-flexible-cloud-nauman-munir-b7ohf/?trackingId=AQuzQxgJQZOrfV9PIlJrlQ%3D%3D)

An AWS Site-to-Site VPN creates an encrypted tunnel over the Internet, securely connecting your on-premises network to a Virtual Private Cloud (VPC) in an AWS region. This VPN uses the IPSec protocol suite to encrypt traffic in transit, ensuring secure communication between your corporate data center and your AWS environment.

---

> Single Site-to-Site VPN connection
![Single Site-to-Site VPN connection](../../architecture-diagrams/aws/Single%20Site-to-Site%20VPN%20connection.png "Single Site-to-Site VPN connection")

---

> Single Site-to-Site VPN connection with a transit gateway
![Single Site-to-Site VPN connection with a transit gateway](../../architecture-diagrams/aws/Single%20Site-to-Site%20VPN%20connection%20with%20a%20transit%20gateway.png)

---

> Multiple Site-to-Site VPN connections
![Multiple Site-to-Site VPN connections](../../architecture-diagrams/aws/Multiple%20Site-to-Site%20VPN%20connections.png)

---

> Multiple Site-to-Site VPN connections with a transit gateway
![Multiple Site-to-Site VPN connections with a transit gateway](../../architecture-diagrams/aws/Multiple%20Site-to-Site%20VPN%20connections%20with%20a%20transit%20gateway_.png)

---

> Site-to-Site VPN connection with AWS Direct Connect
![Site-to-Site VPN connection with AWS Direct Connect](../../architecture-diagrams/aws/Site-to-Site%20VPN%20connection%20with%20AWS%20Direct%20Connect.png)
Site-to-Site VPN connection with AWS Direct Connect

---

> Private IP Site-to-Site VPN connection with AWS Direct Connect
![Private IP Site-to-Site VPN connection with AWS Direct Connect](../../architecture-diagrams/aws/Private%20IP%20Site-to-Site%20VPN%20connection%20with%20AWS%20Direct%20Connect.png)

---

AWS Site-to-Site VPN provides a quick and flexible way to establish a secure connection to AWS.

1. **Rapid Deployment:** Unlike AWS Direct Connect, which can take weeks or even months to set up, a Site-to-Site VPN can be configured and activated in under an hour. This makes it an ideal choice for businesses that need a fast, temporary, or backup connection.
2. **Cost-Effective:** Site-to-Site VPNs utilize your existing Internet connections, eliminating the need for additional hardware or long-term contracts associated with dedicated connections like Direct Connect. This approach can be particularly beneficial for small businesses or those with limited budgets.
3. **Versatility:** Site-to-Site VPNs can serve multiple purposes, from acting as a primary connection to providing a backup for a Direct Connect link. This flexibility makes it a valuable tool for various network architectures.
4. **Encryption Over Direct Connect:** In scenarios where a Direct Connect link is in use, a Site-to-Site VPN can provide an additional layer of security by establishing an encrypted tunnel over the existing Direct Connect link. This ensures data confidentiality and integrity across the connection.

An AWS Site-to-Site VPN consists of several key components that facilitate secure communication between your on-premises network and AWS:

1. **Virtual Private Gateway (VGW):** This is an AWS-managed VPN router interface within the AWS region, associated with a specific VPC. The VGW acts as the termination point for your VPN connection in AWS.
2. **Customer Gateway (CGW):** This represents your on-premises network. The CGW is usually a physical or virtual router located in your data center that connects to the VGW. It must have an externally reachable IP address that AWS can use for establishing the VPN connection.
3. **VPN Connection:** The VPN connection itself is established between the VGW and the CGW, forming a secure IPSec tunnel over the Internet.

Setting up a Site-to-Site VPN with AWS involves a few key steps, depending on whether you’re using Direct Connect or the public Internet:

1. **Determine the IP Address:** Identify the external IP address of your Customer Gateway device that will be reachable by AWS. This address is critical as it serves as the endpoint for the VPN connection.
2. **Specify Routing Information:** You must provide AWS with the subnets and routes for your on-premises network that you want to make accessible through the VPN. AWS will route traffic to and from these subnets over the VPN connection.
3. **Configure the Virtual Private Gateway:** AWS creates a highly available VGW within the region you specify and associates it with the VPC you want to connect. The VGW has two endpoints, each located in different Availability Zones (AZs) for redundancy. These endpoints have publicly reachable IP addresses, enhancing the reliability and availability of the VPN connection.
4. **Ensure High Availability:** To achieve full high availability, you should deploy two separate VPN routers at your on-premises location, each connected to different Internet or Direct Connect links. This setup prevents a single point of failure and ensures continuous operation even if one router or connection fails.

AWS offers two routing options for Site-to-Site VPN connections:

1. **Static Routing:** In this mode, static routes are manually configured in the routing tables of both the on-premises router and the AWS VGW. This is simpler to implement but lacks the flexibility of dynamic route updates and is better suited for single connection setups.
2. **Dynamic Routing with BGP:** For more complex setups involving multiple paths between your on-premises network and AWS, Border Gateway Protocol (BGP) is the recommended routing protocol. BGP enables dynamic route updates, allowing for automatic failover and load balancing in response to changing network conditions. BGP support must be enabled on your Customer Gateway device.

AWS also supports route propagation within the VPC, which automatically updates the VPC's route tables based on the BGP routes received from the VPN connection, ensuring seamless and efficient routing.

There are several performance considerations to keep in mind when using an AWS Site-to-Site VPN:

1. **Bandwidth Limitations:** The Virtual Private Gateway in AWS has a maximum throughput of 1.25 Gbps and a maximum of 140,000 packets per second (PPS) per VPN tunnel. This limit is the maximum aggregate throughput for all VPN connections configured with a single VGW. Actual throughput can be lower, depending on the speed of your on-premises Internet connection and the capabilities of your on-premises router.
2. **Latency Considerations:** Latency over a Site-to-Site VPN is influenced by several factors, including the number of router hops, network congestion, and the geographical distance between your data center and AWS. Because the connection traverses the public Internet, latency can be inconsistent and is not directly controllable by AWS or the customer.

AWS Site-to-Site VPN is a pay-as-you-go service, with charges based on the number of hours the VPN connection is active and the amount of data transferred over the VPN. Pricing varies by region, and additional AWS data transfer charges may apply for traffic passing through the VPN connection.
