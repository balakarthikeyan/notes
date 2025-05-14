𝟭. 𝗙𝗲𝗮𝘁𝘂𝗿𝗲𝘀 

Start with the main features of the system. For example, if asked to design Twitter, list its key features. This helps ensure you're aligned with the interviewer.

𝟮. 𝗨𝘀𝗲𝗿𝘀

Consider the types of users, usage patterns, and growth rates. Identify peak times and regions.

𝟯. 𝗗𝗮𝘁𝗮 𝗠𝗼𝗱𝗲𝗹

Decide between relational and NoSQL databases based on your use case. Plan your tables, indexing, and replication strategies.

𝟰. 𝗚𝗲𝗼𝗴𝗿𝗮𝗽𝗵𝘆 & 𝗟𝗮𝘁𝗲𝗻𝗰𝘆

Reduce latency with servers in different regions. Use CDNs to serve content faster.

𝟱. 𝗦𝗲𝗿𝘃𝗲𝗿 𝗖𝗮𝗽𝗮𝗰𝗶𝘁𝘆

Determine CPU, RAM, and storage needs. Plan for vertical and horizontal scaling.

𝟲. 𝗔𝗣𝗜𝘀 & 𝗦𝗲𝗰𝘂𝗿𝗶𝘁𝘆

Choose between REST, GraphQL, or gRPC for your APIs. Address security concerns and implement rate limiting.

𝟳. 𝗔𝘃𝗮𝗶𝗹𝗮𝗯𝗶𝗹𝗶𝘁𝘆 / 𝗠𝗶𝗰𝗿𝗼𝘀𝗲𝗿𝘃𝗶𝗰𝗲𝘀

Ensure high availability with redundancies and fault tolerance. Use tools like Kubernetes if needed.

𝟴. 𝗖𝗮𝗰𝗵𝗶𝗻𝗴

Speed up reads with caching at various layers. Use appropriate eviction policies.

𝟵. 𝗣𝗿𝗼𝘅𝗶𝗲𝘀

Use load balancers and reverse proxies for better availability and security.

𝟭𝟬. 𝗠𝗲𝘀𝘀𝗮𝗴𝗶𝗻𝗴

Choose the right messaging protocols (TCP/UDP) and tools (Kafka, RabbitMQ) based on your needs.

- `System Design Components & Concepts` that help you create fast, scalable and robust systems:

- `DNS (Domain Name System):` Helps to map a domain name into an IP address. So we only need to remember easy domain names and not complicated IP addresses.

- `Horizontal Scaling:` Increase the throughput of a system by adding more nodes (machines/computers) rather than increasing the individual capacity of a particular node. Leads to distributed systems.

- `Load Balancing:` In distributed systems, a load balancer decides how to route the traffic between multiple nodes. Consider that as a traffic policeman who manages the traffic.

- `Cache:` A faster memory lane, available on the client side as well as the server side. Very simple data type, and mostly uses primary storage to remain swift. Redis, Memcache, and Varnish are common examples.

- `Queues (or schedulers):` Most tasks need time! and the system cannot put other tasks on hold during this time. Queues help in efficiently scheduling tasks, and inform the system once the task is completed. Exactly how YouTube processes video asynchronously.

- `Sharding:` Large databases are generally the culprits for slow services. Sharding is used to break the database into smaller parts, making the queries super efficient.

- `Continuous deployment:` The practice of automatically deploying every build to production after it passes automatic tests

- `Continuous integration:` The practice of automatically and frequently building and unit testing an entire application, ideally on every source code check-in.

- `End-to-End (E2E) testing:` Tests performed to exercise the full flow of an application in the same way an end user

- `Integration:` Tests performed as application pieces are brought together

- `Security testing:` Tests performed to identify flaws in code and runtime to prevent compromises and data leaks in production
