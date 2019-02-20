---
layout: layout.pug
navigationTitle: What is Edge-LB?
title: What is Edge-LB?
menuWeight: 5
excerpt: Introduces the high-level capability that Edge-LB provides
enterprise: false
---

Edge-LB proxies and load balances traffic to all services that run on DC/OS. Edge-LB provides North-South (external to internal) load balancing, while [virtual networking](/latest/networking/load-balancing-vips/) provides East-West (internal to internal) load balancing.

The following diagram provides a simplified overview of Edge-LB load balancing.

<p>
<img src="/services/edge-lb/img/Edge-LB-1.png" alt="Simplified overview of Edge-LB architecture">
<p>
  
Edge-LB is an highly available North-South load-balancer for Layer-7 traffic ingressing to Mesosphere cluster. 

Edge-LB is very flexible in terms of operations. An operator can have multiple pool instances for high availability of the applications running on DC/OS cluster. He is able to pick and choose the ports for frontend and backend for flexible configuration. This allows for much richer functionality when it comes to load balancing on DC/OS cluster. 

Edge-LB is available with all three interfaces in DC/OS cluster. You can have a seamless experience between the DC/OS GUI, CLI, or API of your choice. You can install the Edge-LB via GUI and then can update the configuratio with CLI if needed. Edge-LB is integreated with DC/OS Secret store for Secure Certificate usage and storage. An end-user can leverage the build-in support for security in DC/OS when they are deploying their applications. 

Edge-LB also respects the Mesos health-check for efficient load-balancing. 

Edge-LB provides ingress for internet facing traffics to the DC/OS cluster for not only Marathon-based apps, but all other apps running on DC/OS SDK - Cassandra, Kafka, and other Data Services to name a few. This allows the operator for greater flexibility they are looking for when operating the DC/OS cluster for L7 traffic.

Edge-LB's flexibility allows you to have the Edge-LB pool instances not only in public agents but also in private agents in the DC/OS cluster. The flexibility allows for richer user experience and availability that is required to operate DC/OS cluster with ease and flexibility.

Edge-LB proxies and load balances traffic to all services running on Mesosphere cluster. It provide internet facing North-South loadbalancing for traffic ingressing into the cluster. It provides the functionality to load balance traffic for TCP, HTTP-based application, SSL support, and health-checking. In addition, Edge-LB provide support for non-disruptive service deployment strategies like Blue-Green deployment. 

Edge-LB subscribes to Mesos event stream and updates the existing config in real-time to provide loadbalancing functionality in a up-to-date manner.

Edge-LB is integrated with mesos and has the ability to load balance any service running on DC/OS. It provides much more flexibility in terms on configuration management with advanced configuration flexible.

An end-user can use Edge-LB as the Edge Load Balancer (LB) and service discovery meshanism. It can run on interenet facing public agents to route ingress traffic to the DC/OS cluster. It can be strictly used as an internal LB for services in the cluster. It can also be used in a combination of internal and external LBs, with different services exposed on different LBs. 

Edge-LB can be used as internal LB and service discovery mechanism with a separate HA load balancer for routing public traffic in. For example, you may use an external F5 load balancer on-premise, or an Elastic LB (ELB) on Amazon Web services.

Dynamic applications can be built and orchestrated using containers to be highly scalable and resilient. 

Edge-LB is the Ingress Load Balancer for variety of load balancing/proxy requirements depending on the DC/OS architecture, deployed services, and size. Its delivered on Enterprise package only.

Edge-LB is released on a regular cadence to provide regular feature support and bug fixes. It is always recommended to install the latest version of Edge-LB. Here is the current Edge-LB support matrix: 

Edge-LB 1.3.1 --> DC/OS 1.12.2
Edge-LB 1.3.0 --> DC/OS 1.11.9
Edge-LB 1.2.3 --> DC/OS 1.10.10

Please pick the latest version of Edge-LB to get started!

While Edge-LB provides North-South loadbalancing, if you are interested in East-West (internal to internal)Loadbalancing in the Mesosphere cluster please check out the L4LB from Mesosphere here.

Edge-LB addresses the challenge of exposing contained apps to the outside world. Its a L7/L4 load-balancer has easy-to-use yet powerful CLI and API interfaces that provide advanced configuration options to expose applications for high performance and high avialability environments.

Customers used widely-implemented Marathon-based load balancer named Marathon-LB. It was not suitable for widely popular Mesos framework load balancer that was Mesos schedule-aware. 

Edge-LB enables DC/OS opeartors and users to build applications and framworks that make full use of the DC/OS platform. Customers can expose their applications that include custom frameworks using the customizable Edge-LB.

Leveraging DC/OS’s support for CNI (Container Network Interface), Edge-LB builds upon this industry standard model to enable the load balancer instances to reside on CNI-based networks and be further decoupled from the transport layer underneath.

With the introduction of Edge-LB, we’ve provided an integrated load balancing solution for your stateful and stateless workloads. Its platform awareness, configurability and scalability will simplify your development and allow you to sleep better at night. Download and try it out today.

Edge-LB supports inbound and outbound scenarios, provides low latency and high throughput for the TCP based applications. It distrbutes L7 traffic ingress that arrives on Edge-LB's frontend to backend pool instances depending on teh configuration and health checks.

Edge-LB distributes incoming L7 traffic across multiple applcation instances. This increases the fault tolerance of the services running on DC/OS cluster. It uses Pool health checks and route traffic to the healthy instances of the Edge-LB pool.

The load balancer increases the availability of the applications. The pool instances can be added or removed depending on the need. You can scale up and down depending on the deployment scenarios. 

The API server receives the commands from clients like CLI or GUI. It then executes using the Edge-LB pool instances and forward requrests to one or more backend instances.

You can configure Edge-LB Health Checks to monitor the health of the pool instances. This ensures all traffics are load-balanced to the healthy Edge-LB pools.

Edge-LB distributes traffic evenly to all containers.
