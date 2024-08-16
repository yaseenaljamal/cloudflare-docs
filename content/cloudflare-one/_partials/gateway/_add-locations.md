---
_build:
  publishResources: false
  render: never
  list: never
---

{{<glossary-definition term_id="DNS location">}}

The fastest way to start filtering DNS queries from a location is by changing the DNS resolvers at the router.

To add a DNS location to Gateway:

1. In [Zero Trust](https://one.dash.cloudflare.com), go to **Gateway** > **DNS Locations**.

2. Select **Add a location**.

3. Choose a name for your DNS location.

4. Choose at least one [DNS endpoint](/cloudflare-one/connections/connect-devices/agentless/dns/locations/#dns-endpoints) to resolve your organization's DNS queries.

5. (Optional) Toggle the following settings:

   - **Enable EDNS client subnet** sends a user's IP geolocation to authoritative DNS nameservers. {{<glossary-tooltip term_id="EDNS Client Subnet (ECS)" link="/cloudflare-one/glossary/?term=ecs">}}EDNS Client Subnet (ECS){{</glossary-tooltip>}} helps reduce latency by routing the user to the closest origin server. Cloudflare enables EDNS in a privacy preserving way by not sending the user's exact IP address but rather a `/24` range which contains their IP address.

   - **Set as Default DNS Location** sets this location as the default DoH endpoint for DNS queries.

6. Select **Continue**.
7. (Optional) Turn on source IP filtering for your configured endpoints, then add any source IPv4/IPv6 addresses to validate.

   - Endpoint authentication is required for standard IPv4 addresses and optional for dedicated IPv4 addresses.
   - **DoH endpoint filtering & authentication** lets you restrict DNS resolution to only valid identities or user tokens in addition to IPv4/IPv6 addresses.

8. Select **Continue**.
9. Review the settings for your DNS location, then choose **Done**.