# Enhancing Content Delivery with AWS CloudFront Regional Edge Caches and Origin Shield

![Enhancing Content Delivery with AWS CloudFront Regional Edge Caches and Origin Shield](../../architecture-diagrams/aws/Enhancing%20Content%20Delivery%20with%20AWS%20CloudFront%20Regional%20Edge%20Caches%20and%20Origin%20Shield.png)

AWS CloudFront offers a robust content delivery network (CDN) that uses a combination of edge locations and regional edge caches to deliver content efficiently. This multi-layer caching strategy significantly reduces latency and improves user experience by keeping content closer to users.

While CloudFront edge locations are the primary points for caching content, AWS also provides regional edge caches, which have a larger storage capacity and longer retention times than edge locations. Regional edge caches serve as intermediate storage points between the edge locations and the origin server. They are designed to store less frequently accessed data, ensuring that even if the data times out or is no longer available in the edge location cache, it can still be retrieved quickly from a regional cache without going all the way back to the origin server.

This hierarchical caching model works as follows:

1. **Edge Location Caching:** When a user requests content, DNS routes the request to the nearest CloudFront edge location. If the requested content is cached in the edge location, it is immediately delivered to the user.
2. **Regional Cache Caching:** If the content is not available in the edge location (a "cache miss"), CloudFront checks the regional edge cache. If the content is stored there, it is delivered to the edge location and then to the user, resulting in a faster response compared to fetching directly from the origin.
3. **Origin Fetch:** If the content is also missing from the regional edge cache, CloudFront fetches the data from the origin server (such as an Amazon S3 bucket or an EC2 instance) to populate both the regional edge cache and the edge location cache. This minimizes the distance data must travel, thus reducing latency and improving delivery speed.

This layered approach ensures efficient data delivery by minimizing the need to frequently access the origin server, thereby reducing load and potential bottlenecks.

To manage content updates, CloudFront supports cache invalidation, allowing you to remove specific objects from the cache when they become outdated or need to be refreshed. Each distribution supports up to 3,000 invalidation requests at a time, with up to 15 wildcard invalidations simultaneously. However, to optimize performance and cost, it’s best practice to minimize invalidations and use object versioning or shorten cache expiration times instead. Versioning involves updating the object's file name or URL whenever the content changes, ensuring users always receive the latest version without needing to perform frequent invalidations.

Origin Shield is an additional layer of caching that further optimizes data delivery and reduces the load on the origin server. Origin Shield acts as a central caching layer between regional edge caches, edge locations, and the origin. When enabled, Origin Shield consolidates multiple requests for the same object into a single request to the origin server, significantly reducing the read load on the origin and increasing cache efficiency.

When an edge location or regional edge cache needs to fetch content that is not already cached, it first checks the Origin Shield cache. If the requested content is found in the Origin Shield cache, it is returned to the requesting location (either the edge location or regional cache), avoiding the need to contact the origin server.
If the content is not in the Origin Shield cache, Origin Shield consolidates all incoming requests for that content into a single request to the origin, reducing the overall number of direct requests made to the origin.

By consolidating requests and centralizing caching, Origin Shield reduces latency and bandwidth costs while also enhancing the cache hit ratio at the edge locations. This feature is enabled during the distribution setup and incurs additional charges, but it provides substantial benefits for high-traffic applications or when the origin server is located far from the edge locations.
