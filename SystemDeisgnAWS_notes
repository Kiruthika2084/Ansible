
Content : https://www.youtube.com/watch?v=h8NpIop9Lho

System design on AWS
---------------------

Monolith - Single tiered application in which user interface and data access code are combined into a single proglem
on a single platform

Pros:
Simple
No overengineering
single code base
Resource efficient at small 

Cons:
Modularity is hard to enforce as app grows
scaling is a challenge
All or nothing deployment
Slow to react to customer demand

Can you use API with Monolith ? Yes

Issue of scaling
--------------------
store-> get |
            |
store-> post| API Gateway/Load balancer --> Monolith(entry fn to check url/path and execute fn -> Database
            |
store-> del |


If traffic for store-get increases, threshold of CPU reach the limit
if monolith runs on EC2 and uses autoscaling, you would endup scaling the entire application

With microservices, each codebase for get/post/del would run in a different EC2 machine, in different instant types

can be written in different programming languages (this feature is called polyglot)
each service can scale independently

Independent
-scaling 
-governance
-deployment
-testing
-functionality
Not required to follow every characteristic

Microservices can run in EC2/Lambda(serverless)/EKS/ECS... can front it with ELB or API gateway
--------

In case of EKS

Ingress ALB -> store(get/post/del) -> SERVICES for pods(A/B/C) -> 3 different pods

Can mix and match
store->get ----> EKS
store->post ---> EC2
store->del ----> Lambda

Which one to choose?-> based on the requirement

Load balancer
=====================

jar file in EC2 -> how will you invoke this application

through IP?, what if application scales and has many IPS, how to discover?

Load balancer automatically distribes across application
each ELB will have DNS name
it tracks the health of EC2

what if app to be exposed to a website
use ALB with route53


www.store.com -> Route53(alias mapping) -> ALB DNS -> Application

Types of Load balancer
ALB -> path based routing to particular target groups, Layer7, validates and terminates SSL
NLB -> based on protocol and port of incoming traffic, Layer4, SSL passthrough(TLS termination is possible)

NLB
-handles spiky traffic
-exposes static IP address(ALB needs global accelerator)
-influenced by choices
   -> API Gateway REST API integration with NLB with privatelink
   -> NLB supports EC2 and IP address as backend target group
   -> ALB supports EC2,IP address and Lambda.
   
   
 ===================\
 
 API what and why?
 ------------------------
 
 In hotel waiter acts as API, he takes order from you and gets the food from the kitchen
 
 When applications are invoked by millions of users, lot of factors involved
 like Traffic management, Load balancing , specific input/output needs, Auth(n/z) and many more
 
 All these need not be implemented as program logic which would result in overhead
 thats why we have this layer API gateway
 
 So what is the difference between API and Application Load balancer??
 
 API Gateway
 -------------
 Can implement rate limiting, bursting for APIs
 integrate with WAF
 Not able to get static IP for endpoint
 Accepts HTTPS traffic
 Able to do request validation, request/response mapping
 Able to handle spiky traffic
 Able to integrate with lambda in diff region and diff account
 Pay per use
 No health check 
 Able to cache responces
 
 
 Scaling - Vertical Vs Horizontal
 ----------------------------------
 Vertical - Scaling up/down takes longer
 chances of missing txns during cutover
 Limited scaling
 Expensive
 
 Horizontal
 - Legacy code needs to be refactored for scaling
 
 
 Lambda
==
 for each traffic, a new lambda gets spun up
 rach lambda instance will have same memory allocated
 
 Container-EKS
 ==
 HPA- Horizontal pod autoscaler
 Cluster autoscaler (nodes)
 
 How can you make your appln scalable for a big traffic day???
 pre-warm load balancer
 use scheduled scaling
 lightweight AMI
 use database proxy
 Run IEM to ensure it can handle high traffic
 
 Three-tier architecture
 =====================================
 Frontend/Backend/Database
 
 Avoid single point of failure
 HA,scalable
 Network security(NACL,SG, WAF with LB)
 
 Database
 use aws native databases
 Multi-AZ
 Global database
 Optimize(read replica, caching, query tuning)
 
 
   





           
