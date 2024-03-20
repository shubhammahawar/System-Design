
**Build a system to distribute 6 million free burgers in ~10 minutes.**

**Functional Requirements :**

a. only for users with account
   Each user = 1 account = 1 free burger

b. 30M active user on app, however 20M will take part in this campaign or try and claim free burger.

c. Any user above 6M will get "SORRY" message.

d. All user information  already existing in user table/service.

e. Banner/Button popup on screen.

f. Assignment/Distribution of burger - FIFO.

g. Geographically Confined : If  its a global level - then issue of trcking tracking data at multiple data centre along with multiple languages.

**Non-Functional Requirements**

a. Infrastructure already exists in terms of distributed counter - lets discuss it more.

b. supports for clients - mobile and web.

c. High Availability

d. Low Latency

e. System should be scalable and efficient

f. Slightly under acceptable [ For those who do not accept ? assign exactly 6M]


**Estimation :**

Traffic:
20M request per 10 minute -> i.e 20*10^6 /(60*10) -> 35K rps

**Bandwidth:**
35K request coming each second and each request is of size 1Kb (1000 bytes) i.e
35000*1000bytes = 3.5MB/second


**Storage:**
20M request coming every 10 minutes and each request is of 1Kb 

20M * 10* 1000byts = 200GB

**Cache :**
No Requirement


**API design :**



**Discussion :**


Its a write heavy system compare to read heavy as most of the request are for a new user

Multiple write replica requires Strong consistency because if a request comes it should update the same in all DB's

If multiple concurrent request came from the same user- even though SQL database that has acid properties should not allow the second request to finish and return like an error response that "only 1 free burger is allowed to claim".


**Schema Design :**

**User Table**

userID    timeStamp     statusClaim -> This will tell if the user is already claimed the burger or not, if its status is pending still it will response as fail


**Possible issues and tackle :**

1. Starting the Database - Hashing the userID and route it to specific instances

2. we want to know it should not exceed 6M burger and we will be running into concurrency issues - Suppose 5.99M has already distributed.Now 2 users claiming the spot of 1 left burger which may ran into concurrency issue

**Points :**

a. Burger Give away service is a cluster of service

b. Distributed counters are like in memory counter like redis or something 

c. If these counters are stored in each instance the problem is if instance goes down we will loose the count.

d. If we have a incremental counter then we will need a cordination between distributed counter, aggregator & total count.

e. Instead we will be having a 6 decremented counter with initially 1M each - and once the counter is finisded we will go for another counter and check if it has some capacity

f. We actually might not need Cache here, because we don't know if there is any specific user will try to raise request again or not. 

g. We can easily store 6M record in main DB

h. We will be having a round robin Load Balancer between Burger service and API Service 

**Monitoring Service :**

a. It will check the throughput to the database to the main database and check if the database is goes down what we have to do next

b. Check Response time

c. Check Memory Utilization


**Question ?**

How will you determine from Burger service to counter,which specific counter request will go?

We will use something like Zookeeper to distribute it to specific counter

Can we like use ahsing on userID and then distribute the request based on hashvalue to a counter?

The issue here if request goes to a counter which has a zero capacity and others counter still have capacity left so that will again v=create to and fro of request

How the request will going to server 200/OK ?

In the user Table we will search if the userId is still exist with status clamied/pending we will return a fail request.If it doesn't exist we will check the capacity count of a counter and if its available we will write it and decrement the counter and write succedded in database and return a success response.

So Total 2 checks in the database and one for the counter

What will be the rateLimiting in terms of API Gateway?

So this API Gateway sits in front of multiple service not just Burger give away service. We can have a rate limit to 1 for a specific end point configuration.In our case its a burger give away service - if a particular user is requesting more than once free burger. But it also might not need because we are handling it at database level based on userId and status check.


**WORKFLOW**

1. Clients initiate a request to claim a free burger by sending an HTTP request to the API Gateway.

2. The API Gateway receives the request and sends its to Lod Balancer which routes it to the appropriate 
   endpoint within the system.

3. The App Server receives the request and route the particular request to the Burger Giveaway Service .

4. The Burger Giveaway Service queries the decremented distributed counter (e.g., ZooKeeper) to check the availability of free burgers.

5. If free burgers are available, the request is placed onto a messaging queue for asynchronous processing.
   AppServers (servers) consume messages from the queue.

6. App Server will process the request, It interacts with the database to verify user eligibility and update user records as necessary.

7. Now the App Server updates the database to reflect the claimed free burger and any associated changes to user records.

8. Response - The result of the request processing is sent back through the system, ultimately reaching the client through the API Gateway.




**Architecture Diagram**


![Distribute_Burger](https://github.com/shubhammahawar/System-Design/assets/22192051/07df8372-9369-44a7-ba64-60b16c65cb30)















