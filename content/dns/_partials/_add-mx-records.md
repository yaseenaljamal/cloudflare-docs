---
_build:
  publishResources: false
  render: never
  list: never
---

## Receive email

If you only need to **receive** emails, Cloudflare offers [Email Routing](/email-routing/) for free email forwarding to custom email addresses.

## Send and receive email

To **send and receive** emails from your domain, you need:

- An SMTP provider.
- To create two DNS records within Cloudflare.

To route emails through Cloudflare and to your mail server:

1. Get the IP address and MX record details from your SMTP provider ([vendor-specific guidelines](/dns/manage-dns-records/reference/vendor-specific-records/)).
2. [Add an `A` or `AAAA` record](/dns/manage-dns-records/how-to/create-dns-records/) for your mail subdomain that points to the IP address of your mail server.

     | **Type** | **Name** | **IPv4 address** | **Proxy status** |
     | -------- | -------- | ---------------- | ---------------- |
     | A        | `mail`   | `192.0.2.1`      | DNS only         |

     <details>
      <summary>API example</summary>
      <div>

      ```bash
      ---
      header: Request
      ---
      curl "https://api.cloudflare.com/client/v4/zones/<ZONE_ID>/dns_records" \
      --header "x-auth-email: <EMAIL>" \
      --header "x-auth-key: <API_KEY>" \
      --header "Content-Type: application/json" \
      --data '{
        "type":"A",
        "name":"www.example.com",
        "content":"192.0.2.1",
        "ttl":3600,
        "proxied":false
      }'
      ```

      ```json
      ---
      header: Response
      ---
      {
        "result": {
          "id": "<ID>",
          "zone_id": "<ZONE_ID>",
          "zone_name": "example.com",
          "name": "www.example.com",
          "type": "A",
          "content": "192.0.2.1",
          "proxiable": true,
          "proxied": false,
          "ttl": 1,
          "locked": false,
          "meta": {
            "auto_added": false,
            "managed_by_apps": false,
            "managed_by_argo_tunnel": false,
            "source": "primary"
          },
          "comment": null,
          "tags": [],
          "created_on": "2023-01-17T20:37:05.368097Z",
          "modified_on": "2023-01-17T20:37:05.368097Z"
        },
        "success": true,
        "errors": [],
        "messages": []
      }
      ```

      </div>
      </details>

3. [Add an `MX` record](/dns/manage-dns-records/how-to/create-dns-records/) that points to that subdomain.

      | **Type** | **Name** | **Mail server**    | **TTL** |
      | -------- | -------- | ------------------ | ------- |
      | MX       | `@`      | `mail.example.com` | Auto    |

      <details>
      <summary>API example</summary>
      <div>

      ```bash
      ---
      header: Request
      ---
      curl "https://api.cloudflare.com/client/v4/zones/<ZONE_ID>/dns_records" \
      --header "x-auth-email: <EMAIL>" \
      --header "x-auth-key: <API_KEY>" \
      --header "Content-Type: application/json" \
      --data '{
        "type":"MX",
        "name":"example.com",
        "content":"mail.example.com",
        "ttl":3600
      }'
      ```

      ```json
      ---
      header: Response
      ---
      {
        "result": {
          "id": "<ID>",
          "zone_id": "<ZONE_ID>",
          "zone_name": "example.com",
          "name": "example.com",
          "type": "MX",
          "content": "mail.example.com",
          "priority": 10,
          "proxiable": false,
          "proxied": false,
          "ttl": 3600,
          "locked": false,
          "meta": {
            "auto_added": false,
            "managed_by_apps": false,
            "managed_by_argo_tunnel": false,
            "source": "primary"
          },
          "comment": null,
          "tags": [],
          "created_on": "2023-01-17T20:54:23.660869Z",
          "modified_on": "2023-01-17T20:54:23.660869Z"
        },
        "success": true,
        "errors": [],
        "messages": []
      }
      ```

      </div>
      </details>

{{<Aside type="note">}}

If you encounter issues with your email setup, refer to our [troubleshooting guide](/dns/troubleshooting/email-issues/).

{{</Aside>}}