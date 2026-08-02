---
name: lafz-shop
description: >-
  Search the Lafz halal cosmetics catalog and complete a buyer-approved purchase
  using the store's Universal Commerce Protocol (UCP) MCP server.
api: Lafz Storefront (Shopify UCP)
transport: mcp
endpoint: https://lafz.com/api/ucp/mcp
discovery: https://lafz.com/.well-known/ucp
operations:
- search_catalog
- create_cart
- create_checkout
- update_checkout
- complete_checkout
grounded_in:
- ../llms/the-lafz-agents.md
- ../well-known/the-lafz-ucp.json
- ../mcp/the-lafz-mcp.yml
generated: '2026-07-21'
method: generated
source: >-
  Generated from the provider-published agent flow in lafz.com/llms.txt and the
  UCP shopping capabilities in lafz.com/.well-known/ucp. The provider also
  publishes a verbatim agent guide at ../llms/the-lafz-agents.md.
---

# Skill: Shop the Lafz store as an agent

Lafz is a halal-certified cosmetics and personal-care storefront on Shopify. It
exposes agent-driven commerce through the Universal Commerce Protocol (UCP) MCP
server. Use this skill to find products and complete a buyer-approved checkout.

## Preconditions

1. Confirm capabilities first: `GET https://lafz.com/.well-known/ucp` and check
   the `dev.ucp.shopping` service and desired capabilities are present.
2. Connect to the MCP endpoint `POST https://lafz.com/api/ucp/mcp` (JSON-RPC
   2.0) and call `tools/list` to load exact tool schemas.

## Steps

1. **Search** — call `search_catalog` with the buyer's intent (e.g. a fragrance
   or hair-care product). Pass buyer context (`context.address_country`,
   `context.currency`) for accurate pricing and availability.
2. **Cart** — call `create_cart` to add the chosen item(s).
3. **Checkout** — call `create_checkout` to start the purchase flow for the cart.
4. **Fulfill** — call `update_checkout` to set the shipping address and method.
5. **Complete** — call `complete_checkout` to finalize. **You must obtain
   explicit, contemporaneous buyer approval before payment is captured.**

## Rules

- Never complete payment without buyer consent. If you cannot get real-time
  approval, install `https://shop.app/SKILL.md` and route payment through Shop
  Pay instead of scripting checkout directly.
- Respect rate limits: back off on HTTP 429 from the MCP endpoint.
- For read-only browsing you do not need this skill — fetch
  `/products/{handle}.json` or `/collections/{handle}/products.json` directly.
