# REST API

REST stands for representational state transfer and API stand for application programming interface. REST is known for its simplicity, scalability, and cacheability.
𝗥𝗘𝗦𝗧  is great for web services, microservices, CRUD applications, and mobile apps. Its stateless, scalable nature makes it a strong choice for many applications.
RESTful API is a type of API that uses the REST architectural style, an API is uses REST if it has this characteristics:

## 𝗞𝗲𝘆 𝗕𝗲𝗻𝗲𝗳𝗶𝘁𝘀: 
🔷 `𝐒𝐜𝐚𝐥𝐚𝐛𝐢𝐥𝐢𝐭𝐲`: Because it’s stateless, REST can handle many requests and scale horizontally with ease.
🔷 `𝐒𝐢𝐦𝐩𝐥𝐢𝐜𝐢𝐭𝐲 𝐚𝐧𝐝 𝐅𝐥𝐞𝐱𝐢𝐛𝐢𝐥𝐢𝐭𝐲`:  It uses standard HTTP methods, making it easy to understand and implement.
🔷 `𝐂𝐚𝐜𝐡𝐞𝐚𝐛𝐢𝐥𝐢𝐭𝐲`: Client-side caching can boost performance and lower server load.

## 𝗖𝗵𝗮𝗹𝗹𝗲𝗻𝗴𝗲𝘀: 
🔶 `𝐃𝐚𝐭𝐚 𝐑𝐞𝐝𝐮𝐧𝐝𝐚𝐧𝐜𝐲 𝐚𝐧𝐝 𝐆𝐚𝐩𝐬`: REST APIs might return too much data or need multiple calls for complex queries.
🔶 `𝐑𝐢𝐠𝐢𝐝 𝐒𝐭𝐫𝐮𝐜𝐭𝐮𝐫𝐞`: Sticking to RESTful principles can sometimes make API design inflexible.

`Client-Server` – The communication between Client and Server.
`Stateless` – After the server completed a http request, no session information is retained on the server.
`Cacheable` – Response from the server can be cacheable or non cacheable.
`Uniform Interface` – This are the constraints that simplify things. The constraints are Identification of the resources, resource manipulation through representations, self-descriptive messages, hypermedia as the engine of application state.
`Layered System` – These are intermediaries between client and server like proxy servers, cache servers, etc.
`Code on Demand`(Optional) – This is optional, the ability of the server to send executable codes to the client like java applets or JavaScript code, etc.