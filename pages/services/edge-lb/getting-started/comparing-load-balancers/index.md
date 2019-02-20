---
layout: layout.pug
navigationTitle: Comparing Load Balancers
title: Comparing load balancing services
menuWeight: 15
excerpt: Summarizes the differences between Marathon-LB and Edge-LB load balancing services

enterprise: false
---

Code for Edge-LB config and Marathon-LB config
# Sample configuration example for app with Marathon-LB App - minitwit.json:

```json
{
  "id": "minitwit",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "karlkfi/minitwit",
      "network": "BRIDGE",
      "portMappings": [
        {
          "hostPort": 0,
          "containerPort": 80,
          "servicePort": 10004
        }
      ]
    }
  },
  "instances": 3,
  "cpus": 1,
  "mem": 512,
  "healthChecks": [
    {
      "protocol": "MESOS_HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 2,
      "gracePeriodSeconds": 15,
      "intervalSeconds": 3,
      "maxConsecutiveFailures": 2
    }
  ],
  "labels": {
    "HAPROXY_GROUP": "external",
    "HAPROXY_0_STICKY": "true",
    "HAPROXY_0_VHOST": "<public_agent_public_IP>"
  }
}
```

Configuration example for app with my-app  - my-app-2.json:

```json
{
  "id": "minitwit",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "karlkfi/minitwit",
      "network": "BRIDGE",
      "portMappings": [
        {
          "hostPort": 0,
          "containerPort": 80,
          "protocol": "tcp",
          "name": "web"
        }
      ]
    }
  },
  "instances": 3,
  "cpus": 1,
  "mem": 512,
  "healthChecks": [
    {
      "protocol": "MESOS_HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 2,
      "gracePeriodSeconds": 15,
      "intervalSeconds": 3,
      "maxConsecutiveFailures": 2
    }
  ]
}
```

# Sample configuration for an application
Configuration example for app with my-app  - my-app-2-edgeLBPool.json:
```json
{
  "apiVersion": "V2",
  "name": "minitwit-pool",
  "count": 1,
  "haproxy": {
    "frontends": [
      {
        "bindPort": 80,
        "protocol": "HTTP",
        "linkBackend": {
          "defaultBackend": "minitwit"
        }
      }
    ],
    "backends": [
      {
        "name": "host-httpd",
        "protocol": "HTTP",
        "services": [
          {
            "marathon": {
              "serviceID": "/minitwit"
            },
            "endpoint": {
              "portName": "web"
            }
          }
        ]
      }
    ]
  }
}
```

# How Edge-LB compares to Marathon-LB
The following table provides an overview of the differences between Edge-LB and Marathon-LB load balancing services.

| Edge-LB | Marathon-LB |
|---------|-------------|
| Deployed on any node (Public for ingress) | Generally deployed on DC/OS public agents providing cluster wide ingress Load Balancing |
| Operators deploy individual pool server and load balancers (specific to tenants, groups of applications that share a specific config, or other business rules). | Application developers specify templates or service-specific labels within their app definitions. The labels are picked by the Marathon-LB service from the Marathon Event bus. |
| Designed to scale and provide granular control and differentiated service | Marathon-LB is most often deployed as a single container service with cluster-wide configuration. |
| Applications are explicitly added to the load balancer by administrative users. The load balancer pool is configured for the specific backend service by deploying a new pool configuration. | The service automatically generates load balancer configuration based on the labels and reloads the HAProxy service.


It support all DC/OS applications and services or workloads running on DC/OS cluster. It loadbalances the tasks associated with Mesos frameworks including Data Services. It differs from Marathon-LB in that Marathon-LB was designed only for Marathon-based apps whereas Edge-LB can loadbalance traffic for Marathon as well as mesos based apps.

Marathon-LB is the predecessor of Edge-LB. It has great functionality in terms of loadbalancing traffic from the internet for ingress in the DC/OS cluster. While it works great, it was build for Marathon-based apps. As the ecosystem grew from just Marathon-based apps to SDK based apps like Data services and framework another load balancer was needed with advanced configuration capability and functionality. It reduced the operational complexity when it comes to service upgrade advanced configurations. 

If you want to migrate from existing Marathon-LB configuration, configure an Edge-LB with a new VIP (Virtual IP). Reconfigure the upstream loadbalancer or DNS to point to the Edge-LB VIP instead of Marathon-LB's VIP.

Marathon-LB is the predecessor of Edge-LB. It is based on a single container that manages all processing - config generation, config reload, and load balancing. It listens to the Marathon event bus and therefore is limited to providing load balanced service to Marathon applications only.

Edge-LB solved that problem by working with any service/app running on DC/OS cluster. It listens to mesos event stream. Thus it can load balance all SDK based services like Data services for example Cassandra, Kakfa, etc.

Edge-LB load balancers any service on DC/OS cluster. Its not limited to Marathon-based services. Its critical for load balancing services deployed using DC/OS SDK (i.e. Data services) and load balancing services deployed on K8s cluster running with Mesosphere automation. It provides much more flexibility and control over what operators can do to expose services outside the cluster. 

It provides more granular control over load balance configurations. Its catered to simple to complex configurations management. 

Its more robust solution in terms of resiliency. Marathon-LB could possibly bring down whole cluster if a bad config is provided. 

Edge-LB is built as a DC/OS framework, which can leverage the same DC/OS SDK that all of your production data services are using. This means that you get the same rock solid reliability and platform integration that your mission critical databases and analytics applications are using. With the DC/OS SDK as its foundation, Edge-LB can seamlessly incorporate new features as DC/OS expands.

For example, Marathon-LB’s reliance on app labels made it possible for two different users to specify the same labels, but for different applications. As a result, you could have two completely different applications using the same labels and therefore leveraging the same Marathon-LB configuration, resulting in unintended configuration collision.

Edge-LB removes this potential for misconfiguration. Task names are used instead of labels as the primary mechanism for determining what to load balance. The user is required to define the tasks that they intend to load-balance. This explicit definition ensures uniqueness, since Marathon and other frameworks enforce unique task names.

Marathon-LB can only speak to services running on Marathon. Any services running on Mesos Executor/Task is unknown to Marathon-LB. 

Edge-LB provides a more robust load balancing and proxying solution designed to listen directly to Mesos. Its integrated tightly with DC/OS orchestration, UI, and CLI.

Using Edge-LB instead of Marathon-LB has the following benefits: 
	Support for all applications in DC/OS cluster
	Support for multiple Edge-LB pool for high availability
	Support for 

Labels:
Edge-LB leverages task names instead of labels as its' primary mechanism. It relies on a configuration that requires the user to define the tasks he intends to load-balance, which is much more explicit than allowing users to arbitrarily assign labels.  With labels, in regard to MLB, there is no check in Marathon or frameworks to guarantee the uniqueness of these labels.  This non-uniqueness or label collisions can lead to confusion, especially as your environment scales out. With task names, marathon guarantees uniqueness and since ELB uses task names, the chance of collisions is eliminated.

Second, with tasks, since the user can perform explicit grouping (in marathon), it becomes cleaner to specify rules in Edge-LB.  More importantly, the user does not need to concern themselves with the inner workings of the load balancer.  The operator can take care of the internal details and does not need to expose the load balancer semantics to the user.

Stability of Edge-LB:
Edge-LB does some basic configuration validation before deploying, compared to MLB which only does it at installation time.  The same is true for configuration changes.  With ELB, you can do configuration reloads without any disruptions.  Compare this with MLB, this would mean a complete reinstall which makes the process a lot more error prone and manual.  From a deployment standpoint, you could develop a template which could be validated by ELB and as the platform evolves the level of validation can increase.

Network Compatibility: 
Edge-LB already supports CNI based networks thereby providing you with increased deployment options for your load-balancer strategy.  

Architectural Differences:
ELB's architecture has a clear path towards integrating 3rd party load-balancers (AWS, GCE, Azure) or possibly on-prem load-balancers as well. In order for MLB to accomplish this level of integration, would require a major re-write of Marathon-LB itself, thereby exposing the limitations of the Marathon-LB architecture and why we built Edge-LB.

