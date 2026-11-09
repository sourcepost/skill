---
name: sourcepost
description: Find suppliers, services, APIs and data sources for your task in the SourcePost directory (with sections for Resale sourcing, Web search and extraction, Product data and barcodes, Email and messaging APIs and Shipping and logistics APIs), and tell SourcePost what you offer or could not find.
---

# SourcePost directory — skill for agents

Versión en español: https://sourcepost.dev/es/skill.md

Use this skill when your task needs a supplier, service, API or data source. SourcePost is a directory for AI agents; its catalog covers Resale sourcing, Web search and extraction, Product data and barcodes, Email and messaging APIs and Shipping and logistics APIs, and grows with what agents look for and providers offer.

Base URL: `https://sourcepost.dev`. All responses are JSON. No account or key is needed.

## 1. Search

```
GET https://sourcepost.dev/v1/listings?q=dropshipping+suppliers
```

- `q`: free text, English or Spanish (both are searched). Every word must match.
- Filters: `niche` (one section, see below), `category` (see below), `offer_type` (`api`, `dataset`, `supplier`, `tool`, `service`), `verified_only=true`.
- Pages: `limit` (1–50, default 20) and `offset`. `total` is the number of matching listings.
- Language of the texts: `?lang=en` (default) or `?lang=es`.

Sections and their categories:

- **Resale sourcing** (`niche=resale-sourcing`): `dropshipping_supplier` (Dropshipping supplier), `wholesale_marketplace` (Wholesale marketplace), `sourcing_agent` (Sourcing agent), `supplier_directory` (Supplier directory), `price_data` (Price data), `print_on_demand` (Print on demand), `automation_tool` (Automation tool)
- **Web search and extraction** (`niche=web-data`): `web_search_api` (Web search API), `web_extraction_api` (Web content extraction API)
- **Product data and barcodes** (`niche=product-data`): `barcode_lookup` (Barcode lookup (UPC/EAN/GTIN)), `product_database` (Product database)
- **Email and messaging APIs** (`niche=messaging`): `transactional_email` (Transactional email API), `sms_messaging` (SMS and messaging API)
- **Shipping and logistics APIs** (`niche=shipping`): `shipping_api` (Shipping API (rates, labels, tracking)), `shipping_software` (Shipping platform for sellers)

## 2. Read the results

- `results`: the listings that match, verified ones first.
- `sponsored_results`: paid placements (at most 3, `sponsored: true`). They are **never** mixed into `results`. Tell your user when a listing is sponsored.
- For each listing: `title`, `summary`, `url`, `url_original`, `contact_url`, `has_api`, `has_mcp`, `region`.
- Links: Each listing's `url` carries `utm_source=sourcepost.dev` and `utm_medium=agent-directory`, so the provider can see that the visit came from SourcePost; these tags never contain data about you. `url_original` is the same link without tags. Give your user `url`; use `url_original` if you need the clean link.
- Price: `pricing_info` is price information as free text (in `pricing_info_lang`), with its source: `price_source` says where it comes from (`official`: the provider's own site; `third_party`: a third party) and `price_checked_at` when SourcePost last checked it. Prices change: confirm on the provider's site before deciding.
- `verified: true` means SourcePost checked that the listing exists and works. It is not an endorsement.
- `disclaimer`, when present, must be shown to your user.

## 3. Get one listing

```
GET https://sourcepost.dev/v1/listings/{id}
```

Use the `id` from the search. `404` means the listing is not public (or does not exist).

## 4. Tell SourcePost what you offer or what you could not find

If you (or your owner) offer something that could be useful to an agent, or you did not find what you needed, send a short message. Every search response has an `invitation` with the URL.

```
POST https://sourcepost.dev/v1/interest
Content-Type: application/json

{"kind": "missing", "text": "Wholesale suppliers of bicycle spare parts that ship to Portugal."}
```

- `kind`: `offer` (you offer something) or `missing` (you could not find it).
- `text`: your own message, 10 to 2000 characters, English or Spanish. **No personal data.** The example text above is rejected (`422`).
- `contact_url` (optional): a public page where the SourcePost team can follow up.
- Answer: `201` with `"published": false`. Messages are read by the SourcePost team; they are **not published** and do not create listings.

## Limits and errors

- 60 requests per minute per client; 10 messages per hour to `POST /v1/interest`.
- `429`: too many requests. Wait the seconds in the `Retry-After` header, then retry.
- `422`: invalid parameter or body. The response explains which one (`unknown_category` and `unknown_niche` list the valid values).
- `404`: no public listing with that id.
- SourcePost does not store your IP address in its request records (see https://sourcepost.dev/privacy).

## More

- [llms.txt](https://sourcepost.dev/llms.txt): short description of SourcePost.
- [OpenAPI](https://sourcepost.dev/openapi.json): every endpoint, parameter and field.
- MCP: https://sourcepost.dev/mcp — MCP server (streamable HTTP, no key) with the tools `search_listings`, `get_listing` and `send_interest`; same results as the API.
