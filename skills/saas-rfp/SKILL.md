---
name: saas-rfp
description: Use this skill to post software RFPs for a company, to find cheaper replacements for a paid SaaS tool, or to bid as a seller on SaaS RFP (https://saasrfp.com). Use it when the person lists tools they pay for, asks what companies pay for a tool, or wants to bid on an RFP.
---

# SaaS RFP

SaaS RFP is a public marketplace. A buyer posts an RFP to replace one software vendor. A seller bids with a product and a per-feature coverage claim. Everything is public.

## Connect

Use the MCP server if the client has it. The server URL is https://saasrfp.com/mcp. It needs sign-in through OAuth. The read-only URL is https://saasrfp.com/mcp/public.

If there is no MCP, use the REST API at https://saasrfp.com/api/v1. The spec is https://saasrfp.com/openapi.json. Send "Authorization: Bearer <token>". The person makes a token at https://saasrfp.com/settings. Read calls need no token.

The full manual is at https://saasrfp.com/llms-full.txt.

## Post RFPs for a company

1. List the tools the company pays for. Get the annual cost of each in USD.
2. For each tool, list the features the company really uses.
3. Optional: call get_vendor (or GET /api/v1/vendors/{slug}) to read the vendor's feature names. Use those names.
4. Show the person the entries. Wait for a yes.
5. Call post_rfps with the entries. Or POST /api/v1/rfps. Or use the page https://saasrfp.com/rfps/bulk.
6. Report each result. An outcome is posted, already_posted or error.

Entry format. Post at most 60 at once.

```json
[
  {
    "vendor": "Notion",
    "annualSpend": 9600,
    "seats": 40,
    "renewalDate": "2027-03-01",
    "features": ["Docs", { "name": "Databases", "importance": "must", "evidence": "212 active databases" }]
  }
]
```

- vendor, annualSpend and features are required.
- Optional: seats, renewalDate (YYYY-MM-DD), plan, title, description, anonymous.
- A vendor with an open RFP from the same account is skipped. Posting twice is safe.

## Find a cheaper replacement

1. Call search with the vendor name. Or call list_rfps with the vendor slug.
2. Call get_rfp for the RFP. Read the bids, the prices and the match percent.
3. Report the url of each record you cite.

## Bid as a seller

1. Call create_product once for each product. Keep the product slug.
2. Call get_rfp. Copy the feature ids.
3. Call submit_bid with the productSlug, the annualPrice in dollars and one coverage entry for each feature id.
4. Coverage levels are full, partial, integration and none. A feature you leave out counts as none.
5. Be honest. The buyer reads each claim.

## Rules

- Everything posted is public.
- Report figures and feature usage only. Do not post contract text, invoices or confidential terms.
- Post only what the person may disclose.
- Ask the person before any write call.
- Money is whole US dollars per year.
