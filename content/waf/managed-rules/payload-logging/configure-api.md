---
title: Configure payload logging via API
pcx_content_type: how-to
weight: 4
meta:
  title: Configure payload logging for a managed ruleset via API
---

# Configure payload logging for a managed ruleset via API

Use the [Rulesets API](/ruleset-engine/rulesets-api/) to configure payload logging for a managed ruleset via API.

## Configure and enable payload logging

1. Use the [Get a zone entry point ruleset](/api/operations/getZoneEntrypointRuleset) operation to obtain the following IDs:

    - The ID of the [entry point ruleset](/ruleset-engine/about/rulesets/#entry-point-ruleset) of the `http_request_firewall_managed` [phase](/ruleset-engine/about/phases/).
    - The ID of the rule deploying the WAF managed ruleset (an `execute` rule) for which you want to configure payload logging.

2.  Use the [Update a zone ruleset rule](/api/operations/updateZoneRulesetRule) operation to update the rule you identified in the previous step.

    Include a `matched_data` object in the rule's `action_parameters` object to configure payload logging. The `matched_data` object has the following structure:

    ```json
    ---
    highlight: 3-5
    ---
    "action_parameters": {
      // ...
      "matched_data": {
        "public_key": "<PUBLIC_KEY_VALUE>"
      }
    }
    ```

    Replace `<PUBLIC_KEY_VALUE>` with the public key you want to use for payload logging. You can generate a public key [in the command line](/waf/managed-rules/payload-logging/command-line/generate-key-pair/) or [in the Cloudflare dashboard](/waf/managed-rules/payload-logging/configure/).

{{<Aside type="note" header="Account-level configuration">}}

To configure payload logging for a managed ruleset deployed at the account level (only available in Enterprise plans with a paid add-on), use the following API operations instead:

- In step 1: [Get an account entry point ruleset](/api/operations/getAccountEntrypointRuleset)
- In step 2: [Update an account ruleset rule](/api/operations/updateAccountRulesetRule)

{{</Aside>}}

### Example

This example configures payload logging for the [Cloudflare Managed Ruleset](/waf/managed-rules/reference/cloudflare-managed-ruleset/), which is already deployed for a zone with ID `{zone_id}`.

1. Invoke the [Get a zone entry point ruleset](/api/operations/getZoneEntrypointRuleset) operation (a `GET` request) to obtain the rules currently configured in the entry point ruleset of the `http_request_firewall_managed` phase.

    ```bash
    ---
    header: Request
    ---
    curl https://api.cloudflare.com/client/v4/zones/{zone_id}/rulesets/phases/http_request_firewall_managed/entrypoint \
    --header "Authorization: Bearer <API_TOKEN>"
    ```

    ```json
    ---
    header: Example response
    highlight: 3,12,20
    ---
    {
      "result": {
        "id": "060013b1eeb14c93b0dcd896537e0d2c", // entry point ruleset ID
        "name": "default",
        "description": "",
        "source": "firewall_managed",
        "kind": "zone",
        "version": "3",
        "rules": [
          // (...)
          {
            "id": "1bdb49371c1f46958fc8b985efcb79e7", // `execute` rule ID
            "version": "1",
            "action": "execute",
            "expression": "true",
            "last_updated": "2024-01-20T14:21:28.643979Z",
            "ref": "1bdb49371c1f46958fc8b985efcb79e7",
            "enabled": true,
            "action_parameters": {
              "id": "efb7b8c949ac4650a09736fc376e9aee", // "Cloudflare Managed Ruleset" ID
              "version": "latest"
            }
          },
          // (...)
        ],
        "last_updated": "2024-01-20T14:29:00.190643Z",
        "phase": "http_request_firewall_managed"
      },
      "success": true,
      "errors": [],
      "messages": []
    }
    ```

2. Save the following IDs for the next step:

    - The ID of the entry point ruleset: {{<rule-id>}}060013b1eeb14c93b0dcd896537e0d2c{{</rule-id>}}
    - The ID of the `execute` rule deploying the Cloudflare Managed Ruleset: {{<rule-id>}}1bdb49371c1f46958fc8b985efcb79e7{{</rule-id>}}

    To find the correct rule in the `rules` array, search for an `execute` rule containing the ID of the Cloudflare Managed Ruleset ({{<rule-id>}}efb7b8c949ac4650a09736fc376e9aee{{</rule-id>}}) in `action_parameters` > `id`.

    {{<Aside type="note">}}
To get the IDs of existing WAF managed rulesets, refer to [WAF Managed Rules](/waf/managed-rules/#managed-rulesets) or use the [List account rulesets](/api/operations/listAccountRulesets) operation.
    {{</Aside>}}

3. Invoke the [Update a zone ruleset rule](/api/operations/updateZoneRulesetRule) operation (a `PATCH` request) to update the configuration of the rule you identified. The rule will now include the payload logging configuration (`matched_data` object).

    ```bash
    ---
    header: Request
    highlight: 9-11
    ---
    curl --request PATCH \
    "https://api.cloudflare.com/client/v4/zones/{zone_id}/rulesets/060013b1eeb14c93b0dcd896537e0d2c/rules/1bdb49371c1f46958fc8b985efcb79e7" \
    --header "Authorization: Bearer <API_TOKEN>" \
    --header "Content-Type: application/json" \
    --data '{
      "action": "execute",
      "action_parameters": {
        "id": "efb7b8c949ac4650a09736fc376e9aee",
        "matched_data": {
          "public_key": "Ycig/Zr/pZmklmFUN99nr+taURlYItL91g+NcHGYpB8="
        }
      },
      "expression": "true"
    }'
    ```

    The response will include the complete ruleset after updating the rule.

For more information on deploying managed rulesets via API, refer to [Deploy a managed ruleset](/ruleset-engine/managed-rulesets/deploy-managed-ruleset/) in the Ruleset Engine documentation.

---

## Disable payload logging

To disable payload logging for a managed ruleset:

1. Use the [Update a zone ruleset rule](/api/operations/updateZoneRulesetRule) operation (a `PATCH` request) to update the rule deploying the managed ruleset (an `execute` rule).

2. Modify the rule definition so that there is no `matched_data` object in `action_parameters`.

For example, the following `PATCH` request updates rule with ID `{rule_id}` deploying the [Cloudflare Managed Ruleset](/waf/managed-rules/reference/cloudflare-managed-ruleset/) so that payload logging is disabled:

```bash
curl --request PATCH \
"https://api.cloudflare.com/client/v4/zones/{zone_id}/rulesets/{entrypoint_ruleset_id}/rules/{rule_id}" \
--header "Authorization: Bearer <API_TOKEN>" \
--header "Content-Type: application/json" \
--data '{
  "action": "execute",
  "action_parameters": {
    "id": "efb7b8c949ac4650a09736fc376e9aee"
  },
  "expression": "true"
}'
```

For details on obtaining the entry point ruleset ID and the ID of the rule to update, refer to [Configure and enable payload logging](/waf/managed-rules/payload-logging/configure-api/#configure-and-enable-payload-logging).