---
pcx_content_type: concept
title: Load Balancing components
weight: 3
---

# Load Balancing components

This page provides a simplified overview of the three main components of the Cloudflare Load Balancing solution and how they relate to one another.

## Load balancers

For a hostname (`blog.example.com`) to resolve, the Domain Name System (DNS) must return an IP address, where the website or application is hosted (origin).

When you set up a load balancer, Cloudflare automatically creates an [LB DNS record](/load-balancing/load-balancers/dns-records/) for the specified hostname. This means that, according to a [priority order](/load-balancing/load-balancers/dns-records/#priority-order), instead of simply returning an IP address, the logic you introduced using the Cloudflare Load Balancing solution will be considered.

{{<render file="_load-balancing-diagram.md">}}

## Pools

Within Cloudflare, pools represent your endpoints and how they are organized. As such, a pool can be a group of several endpoints, or you could also have only one endpoint per pool — it depends on what best suits your use case.

For example, if you are only using Cloudflare to globally distribute traffic across regions ([global traffic steering](/load-balancing/understand-basics/traffic-steering/steering-policies/)), each pool could represent one region and, within each region, you could have one endpoint that represents the entry point to your data center.

Cloudflare [local traffic management (LTM)](/load-balancing/local-traffic-management/) solution and [endpoint steering](/load-balancing/understand-basics/traffic-steering/origin-level-steering/) capabilities enable you to also load balance traffic between your servers within a data center. In this use case, each pool would represent a data center and contain several endpoints that represent your servers.

## Endpoints

{{<glossary-definition term_id="endpoint" prepend="Endpoints refer to ">}}

## Monitors

Finally, monitors are the component you can use to guarantee only [healthy pools](/load-balancing/understand-basics/health-details/) are considered for traffic distribution.

When you configure a monitor and attach it to endpoints, the monitor will issue health monitor requests to your endpoints at regular intervals. This process makes it possible for your load balancer to intelligently handle traffic, considering which endpoints are actually available.

{{<render file="_health-check-diagram.md">}}

{{<Aside type="note">}}

Health monitors associated with load balancers are different from [**Standalone health checks**](/health-checks/).

{{</Aside>}}