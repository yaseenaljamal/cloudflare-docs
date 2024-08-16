---
pcx_content_type: concept
source: https://support.cloudflare.com/hc/en-us/articles/200170016-What-is-Email-Address-Obfuscation-
title: Email Address Obfuscation
weight: 1
---

# Email Address Obfuscation

By enabling Cloudflare Email Address Obfuscation, email addresses on your web page will be hidden from bots, while keeping them visible to humans. In fact, there are no visible changes to your website for visitors.

## Background

Email harvesters and other bots roam the Internet looking for email addresses to add to lists that target recipients for spam. This trend results in an increasing amount of unwanted email.

Web administrators have come up with clever ways to protect against this by writing out email addresses, such as `help [at] cloudflare [dot] com` or by using embedded images of the email address. However, you lose the convenience of clicking on the email address to automatically send an email. By enabling Cloudflare Email Address Obfuscation, email addresses on your web page will be obfuscated (hidden) from bots, while keeping them visible to humans. In fact, there are no visible changes to your website for visitors.

## Change Email Address Obfuscation setting

Cloudflare enables email address obfuscation automatically when you sign up.

{{<tabs labels="Dashboard | API">}}
{{<tab label="dashboard" no-code="true">}}

To disable **Email Address Obfuscation** in the dashboard:

1.  Log into the [Cloudflare dashboard](https://dash.cloudflare.com/login).
2.  Select your account and website.
3.  Go to **Scrape Shield**.
4.  For **Email Address Obfuscation**, switch the toggle to **Off**.

{{</tab>}}
{{<tab label="api" no-code="true">}}

To disable **Email Address Obfuscation** with the API, send a [`PATCH`](/api/operations/zone-settings-edit-single-setting) request with `email_obfuscation` as the setting name in the URI path, and the `value` parameter set to `"off"`.

{{</tab>}}
{{</tabs>}}

{{<render file="_configuration-rule-promotion.md" productFolder="rules">}}

## Prevent Cloudflare from obfuscating email

To prevent Cloudflare from obfuscating specific email addresses, you can:

-   Add the following comment in the page HTML code: 

    ```
    <!--email_off-->contact@example.com<!--/email_off-->
    ```

-   Return email addresses in JSON format for AJAX calls, making sure your web server returns a content type of `application/json`.
-   Disable the Email Obfuscation feature by creating a [Configuration Rule](/rules/configuration-rules/) to be applied on a specific endpoint.

---

## Troubleshoot email obfuscation

To prevent unexpected website behavior, email addresses are not obfuscated when they appear in:

-   Any HTML tag attribute, except for the _href_ attribute of the _a_ tag.
-   Other HTML tags:
    -   _script_ tags: `<script></script>`
    -   _noscript_ tags: `<noscript></noscript>`
    -   _textarea_ tags: `<textarea></textarea>`
    -   _xmp_ tags: `<xmp></xmp>`
    -   _head_ tags: `<head></head>`
-   Any page that does not have a MIME type of `text/html` or `application/xhtml+xml`.

{{<Aside type="warning" header="Warnings">}}

Email Obfuscation will not apply in the following cases:

- You are using the `Cache-Control: no-transform` header.
- The HTML/JS code is specifically added by a [Worker](/workers/).

{{</Aside>}}