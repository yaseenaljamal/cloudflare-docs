---
pcx_content_type: reference
title: Types of analytics
weight: 2
---

# Types of analytics

Cloudflare Analytics is a comprehensive product that encompasses all metadata generated by the Cloudflare network. You can access these insights through the Cloudflare dashboard. Depending on where in the dashboard you are, it will show you different aspects from the collected metadata.

## Root-level analytics

### Account Analytics (beta)

Available under **Analytics & Logs** in your Cloudflare dashboard when you log in, Account Analytics (beta) shows you an [overview of traffic for all sites](/analytics/account-and-zone-analytics/account-analytics/) on your Cloudflare account, such as requests and bandwidth by country, information related to security, cache, and errors, among others. To access Account Analytics, [log in to the Cloudflare dashboard](https://dash.cloudflare.com/login), select the appropriate account, and go to **Analytics & Logs** > **Account Analytics**.

### Network Analytics

Network Analytics provides [visibility into network and transport-layer traffic patterns, and DDoS attacks](/analytics/network-analytics/).

The Network Analytics dashboard is only available for customers on an Enterprise plan who use [Spectrum](/spectrum/), [Magic Transit](/magic-transit/), or [Bring Your Own IP (BYOIP)](/byoip/).

### Web Analytics

Web Analytics (formerly known as Browser Insights) [provides free, privacy-first analytics for your website](/analytics/web-analytics/). Web Analytics does not collect your visitor's personal data, and allows you to have a detailed view into the performance of web pages as experienced by your visitors.


### Carbon Impact Report

Carbon Impact Report gives you a [report on carbon savings](https://blog.cloudflare.com/understand-and-reduce-your-carbon-impact-with-cloudflare/) from using Cloudflare services versus Internet averages for your usage volume.

Cloudflare is committed to use 100% renewable energy sources, but also to [remove all greenhouse gases emitted](https://blog.cloudflare.com/cloudflare-committed-to-building-a-greener-internet/) as a result of powering our network since 2010.

## Analytics related to specific properties

Access aggregated traffic, security, and performance metrics for each domain proxied through Cloudflare. To access these analytics, [log in to the Cloudflare dashboard](https://dash.cloudflare.com/login), select your account and domain, and go to the **Analytics** application.

Data available on the **Analytics** application includes:

* **Traffic** - Requests, Data transfer, Page views, Visits, and [API requests](/api-shield/security/api-discovery/#api-requests).
* **Security** - Total Threats, Top Crawlers/Bots, Rate Limiting, Total Threats Stopped.
* **Performance** - Origin Performance, Bandwidth Saved.
* **Edge Reachability** - [Last mile insights](/network-error-logging/) for Enterprise customers.
* **DNS** - DNS Queries by Response Code, Record Type, and Cloudflare Data Center. [Available metrics](/analytics/account-and-zone-analytics/zone-analytics/#dns) vary according to the zone plan.
* **Workers** - [Detailed information](/workers/observability/metrics-and-analytics/) related to your Workers per zone, and Workers KV per account.
* **Logs** - [Detailed logs](/logs/) of the metadata generated by Cloudflare products for Enterprise customers.
* **Instant logs** - [Live stream of traffic](/logs/instant-logs/) for your domain. Enterprise customers can access this live stream from the Cloudflare dashboard or from a command-line interface (CLI).

## Product analytics

Beyond the analytics provided for your properties, you can also access analytics related to specific products:

* [Bot Analytics](/bots/bot-analytics/) - Shows which requests are associated with known bots, likely automated traffic, likely human traffic, and more.
* [Cache Analytics](/cache/performance-review/cache-analytics/) - Insights to that help determine if resources are missing from cache, expired, or ineligible for caching.
* [Security Events](/waf/analytics/security-events/) - Highlights attack and mitigation metrics detected by the Cloudflare WAF and HTTP DDoS protection systems.
* [Security Analytics](/waf/analytics/security-analytics/) - Displays information about all incoming HTTP requests, including those not affected by security measures (for example, from the WAF and DDoS protection systems).
* [Load Balancing Analytics](/load-balancing/reference/load-balancing-analytics/) - Features metrics to help gain insights into traffic load balancer steering decisions.


## GraphQL APIs

If you would like to have more control over how you visualize the analytic and log information available on the Cloudflare dashboard, use the [GraphQL Analytics API](/analytics/graphql-api/) to build customized views. This API replaces and expands on the previous [Zone Analytics API](/api/operations/zone-analytics-(-deprecated)-get-dashboard).