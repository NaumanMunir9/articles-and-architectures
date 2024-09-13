# Enhancing AWS Site-to-Site VPN with AWS Global Accelerator

[Enhancing AWS Site-to-Site VPN with AWS Global Accelerator - LinkedIn](https://www.linkedin.com/pulse/enhancing-aws-site-to-site-vpn-global-accelerator-nauman-munir-cjxhf/?trackingId=AQuzQxgJQZOrfV9PIlJrlQ%3D%3D)

To enhance the performance of AWS Site-to-Site VPN, AWS provides the option to use AWS Global Accelerator, which significantly reduces network latency and congestion that might occur when routing your VPN traffic over the public Internet. By leveraging AWS Global Accelerator, your VPN traffic is routed through AWS's global network, providing a more reliable and faster connection compared to the traditional path through the public Internet.

> Dual tunnel site-to-site VPN
![Dual tunnel site-to-site VPN](../../architecture-diagrams/aws/Dual%20tunnel%20site-to-site%20VPN.png "Dual tunnel site-to-site VPN")

---

> Accelerated site-to-site VPN
![Accelerated site-to-site VPN](../../architecture-diagrams/aws/Accelerated%20site-to-site%20VPN.png "Accelerated site-to-site VPN")

AWS Global Accelerator is designed to improve the performance of your Site-to-Site VPN by routing traffic to the nearest AWS edge location. Once the traffic reaches this edge location, it traverses AWS's dedicated, high-performance global network rather than the public Internet. This process reduces latency, increases throughput, and provides a more consistent network experience.

By routing traffic through the nearest AWS edge location, Global Accelerator minimizes the number of hops your data must travel over the public Internet. This reduction in distance and the use of AWS's high-speed backbone network decreases latency and improves overall VPN performance.

AWS's global network is designed for high availability and fault tolerance. By using this network for your VPN traffic, you reduce the risk of packet loss, jitter, and other issues that can occur on the less predictable public Internet. Although the Site-to-Site VPN already provides encryption, using AWS's private network for data transport further reduces the risk of exposure to potential threats on the public Internet.

To enable the AWS Global Accelerator for your Site-to-Site VPN, you need to configure the VPN to route traffic through the Global Accelerator's edge locations.

1. **Enable Acceleration During VPN Creation:** You can enable the AWS Global Accelerator option at the time of creating your Site-to-Site VPN connection. If you decide to enable acceleration after the VPN has already been created, you will need to create a new VPN connection with acceleration enabled.
2. **Configure VPN Tunnel Endpoints:** When you enable acceleration, the VPN tunnel endpoint IP addresses will automatically change to point to the Global Accelerator endpoints. This change is necessary to route traffic through the AWS global network instead of the public Internet.
3. **Update Customer Gateway:** After enabling acceleration, you must update the configuration on your Customer Gateway VPN appliance to reflect the new VPN tunnel endpoint IP addresses.
4. **Enable NAT Traversal:** Network Address Translation (NAT) traversal must be enabled on both ends of the VPN connection to allow the accelerated tunnels to remain up and running.
5. **Re-key Internet Key Exchange (IKE)**: To maintain secure connections over the accelerated tunnels, you must ensure that IKE re-keying is properly configured at the customer end of the VPN. This step is critical to keeping the VPN connection secure and active.

While AWS Global Accelerator can significantly improve the performance and reliability of your Site-to-Site VPN connection, there are some important limitations and considerations to keep in mind:

1. **Requires Transit Gateway:** The accelerated VPN feature requires a Transit Gateway to function. Virtual Private Gateways (VGWs) do not support accelerated VPN connections. This means that if your architecture currently uses VGWs, you will need to transition to using a Transit Gateway to take advantage of acceleration.
2. **No Support for DX Public Virtual Interfaces:** Accelerated VPN connections cannot be used in conjunction with Direct Connect public virtual interfaces. This limitation may affect certain hybrid cloud architectures that rely on Direct Connect for public connectivity.
3. **New Connection Required for Existing VPNs:** For existing Site-to-Site VPN connections, you cannot simply toggle acceleration on or off. Instead, you must create a new VPN connection with acceleration enabled and reconfigure your customer gateway to use the new connection. Once the new connection is established, the old non-accelerated VPN connection should be deleted.
