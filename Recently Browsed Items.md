System Design: Recently Browsed
Items
Conn
Description
An e-commerce company called HackerWear has blown up recently. Their backend is made in NodeS and is getting 5X the traffic as compared to last week
The company uses a microservices-based architecture and has scaled up to handle the load. To capitalize on the flow, the company has decided to Implement a component that shows the user their last 5 browsed products.
They belleve that by bullding this, the user will spend more time on their site, and hence the chances of them ordering sorething will increase.
Design the system of the upcoming component such that the existing functionality of the site remains as it is. Provide a solution with a design optimized to integrate seamlessly with the existing design.
Components:
• Cloud Infrastructure - Cloud like AW/S OpenConnect which act as a content delivery network
• Backend- NodelS with Mongo
• Client- Any device which can browse the internet
• Services Running = User Service Product Service, Order Servica, Payment Service



**Functional Requirements:**

**User Service Functionality:**
  Track user browsing history.
  Store browsing history in the database along with timestamps.

**Product Service Functionality:**
  Provide endpoints to fetch the last 5 browsed products for a user.
  Implement caching mechanism to store and retrieve user's browsing history efficiently.

**Client-side Component Functionality:**
  Fetch last 5 browsed products for the current user.
  Display the retrieved products in the user interface.

**Non-functional Requirements:**

**Performance:**
  Ensure low latency in retrieving the last 5 browsed products.
  Implement efficient caching to minimize database queries.

**Scalability:**
  Design the system to handle increased traffic without compromising performance.
  Utilize cloud infrastructure for horizontal scaling.

**Reliability:**
  Ensure high availability of the system.
  Implement fault-tolerant mechanisms to handle failures gracefully.

**Security:**
  Authenticate users before accessing their browsing history.
  Encrypt sensitive data such as browsing history to protect user privacy.

**Estimation :**

**Traffic :**
Suppose 200 million users are registered on the app and only 10 % are active on a given day. This will gave us the 20 million users/ day.


Reads/Write per month :
As we assume its 100:1 read write ratio 

For Reads per day = 100 * 20 million = 2 billion/day
For Write per day =  1*20 million = 20 million/day

**Request per second =**

For write request
20 *1000000 /(24*3600) =  230 request per second

For read request
2 billion / (24*3600) = 2.3 MB/sec

**Bandwidth :**
We expect around 230 request generating every second and each request is of size 500 bytes then total incoming data =
230 * 500 bytes = 115KB/second


**Storage :**
For storage, we will assume we store each record in our database for 10 years. 
Since we expect around 20M new requests every day, the total number of records we will need to store would be:

20M* 10 years* 12 months * 24 hrs = 57 billion

Also each record is of size 500 bytes = 57 billion * 500 bytes = 28 TB


**Caching :**

Cache Implementation:
   Use an in-memory cache such as Redis to store the last 5 browsed products for each user.

Cache Invalidation:
   Invalidate cache entries when a user browses a new product to reflect the updated browsing history.
   Implement a time-based or event-based invalidation mechanism.


**API Design:**

**User Service API:**
   Endpoint: /track-browsing
   Method: POST
   Parameters: User ID, Product ID
   Description: Tracks user browsing history and stores the product ID along with the timestamp.

**
Product Service API:**
   Endpoint: /last-browsed-products/:userId
   Method: GET
   Parameters: User ID
   Description: Retrieves the last 5 browsed products for the specified user.


**Component :**

Cloud Infrastructure: This represents the underlying cloud services such as AWS or OpenConnect that host the microservices and provide scalability and reliability.
Load Balancer: Distributes incoming traffic across multiple instances of the User Service and Product Service for load balancing and fault tolerance.
User Service: Handles user-related functionalities including tracking browsing history. It communicates with the RedisIn Cache to retrieve user last products data.
Product Service: Manages products and implements caching for efficient retrieval of browsing history. It communicates with both the MongoDB database and the Redis cache.
MongoDB Database: Stores user information and browsing history data. It is accessed by both the User Service and the Product Service.
Redis Cache: In-memory cache used by the Product Service to store the last 5 browsed products for each user. It helps reduce latency and improve performance by caching frequently accessed data.


**Let’s go over how the system will work**

1. User will use their favourite browser to view the products page
2. Product details will be fetched from the product service
3. The Product service will publish an event on the ActiveMQ. The event will have following structure

{
"timestamp": 123456767,
"userId": 123,
"itemId": 456
}
4. The writer side of the recently viewed items page service will listen to events
5. The event will be written to PostgreSQL for long term storage. This data can later be used for product recommendations and other Machine Learning activities.
6. Writer will write the event to Redis. Here we will make use of Redis sorted sets to keep latest 100 events produced in last 6 months. 
   We will have one sorted set per user.
7. User makes a request through UserService to view the recently viewed items. The reque
8. The request will go to any of the reader based on their current CPU utilisation
9. The reader will make a call to the Redis sorted set and return the response


![Recently Browsed Items IMG](https://github.com/shubhammahawar/System-Design/assets/22192051/1543ee62-4749-4538-9123-ff72fbae0972)







