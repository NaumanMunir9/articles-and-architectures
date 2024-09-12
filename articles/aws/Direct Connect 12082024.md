# AWS Direct Connect (DX)

[LinkedIn - AWS Direct Connect (DX)](https://www.linkedin.com/pulse/aws-direct-connect-dx-nauman-munir-uf4zf/?trackingId=%2FUrckDAQSSGG2F%2B0geFrrA%3D%3D)

AWS Direct Connect (DX) offers a secure, private connection directly between your data center and an AWS region, bypassing the public Internet. This service provides consistent speeds, low latency, and improved security, making it an ideal solution for transferring large volumes of data. Using Direct Connect can also be more cost-effective than relying on Internet-based transfers.

AWS Direct Connect is available in over 100 locations worldwide, spanning nearly 70 cities in 30 different countries. These locations host AWS routers that interconnect with customer networks and telecom providers. It’s important to note that these Direct Connect facilities are not AWS-owned data centers; instead, they are located in colocation facilities or "carrier hotels," which serve as neutral points for interconnection. Currently, there are 99 partners, including major companies like AT&T, CoreSite, Digital Realty, Equinix, and Lumen.

![AWS Direct Connect private VIF](../../architecture-diagrams/aws/AWS%20Direct%20Connect%20private%20VIF.png)

![AWS Direct Connect public VIF](../../architecture-diagrams/aws/AWS%20Direct%20Connect%20public%20VIF.png)

To establish a Direct Connect connection, you need to install a router at the interconnection facility and provision a data circuit through a provider to the Direct Connect location. Connections are available at different bandwidths—1 Gbps, 10 Gbps, or 100 Gbps—based on your needs. Some service providers also offer subrate connections below 1 Gbps. Multiple connections can be aggregated using the Link Aggregation Control Protocol (LACP) to achieve higher speeds.

While AWS Direct Connect provides a private link, it does not inherently encrypt the data transferred over it. For added security, many organizations implement an IPsec VPN connection over the Direct Connect link to ensure data encryption in transit.

![AWS Direct Connect - VPN backup](../../architecture-diagrams/aws/AWS%20Direct%20Connect%20-%20VPN%20backup.png)

![AWS Direct Connect - Hardware high availability connections](../../architecture-diagrams/aws/AWS%20Direct%20Connect%20-%20Hardware%20high%20availability%20connections.png)

After establishing the physical connection, you can create Virtual Interfaces (VIFs), which can be either private or public:

1. **Private VIF:** Connects directly to a Virtual Private Cloud (VPC) using a Virtual Gateway (VGW). It supports 802.1Q VLAN tagging and uses BGP for dynamic route exchange.
2. **Public VIF:** Allows connections to AWS public services (such as DynamoDB, Route 53, S3, or CloudFront) in any AWS region. However, it cannot be used as an Internet connection.

A single Direct Connect connection can support multiple private VIFs, enabling connections to multiple VPCs within a single region. Additionally, a VIF can be shared across multiple AWS accounts, which is referred to as a hosted VIF.

To achieve high availability, it is advisable to have a backup Direct Connect link or use a VPN connection as a backup for the Direct Connect link. When using VPN as a backup, keep in mind that the capacity should not exceed 1 Gbps due to AWS VPN throughput limitations.

AWS ensures redundancy from its side with multiple data circuits between the hosted interconnection facilities and AWS regional data centers, utilizing BGP routing for automatic failover. However, as a customer, you are responsible for ensuring redundancy from your data center to the AWS cross-connect location. This may involve configuring a dedicated backup link or setting up a site-to-site VPN over the Internet to the AWS region where the primary Direct Connect is terminated.

Additionally, to prevent single points of failure, consider deploying redundant routers both in your data center and at the colocation facility. This approach provides resilience against potential router failures.
