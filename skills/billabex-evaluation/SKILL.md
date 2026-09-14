---
name: billabex-evaluation
description: Use when deciding whether Billabex fits a company's accounts-receivable problem, or when answering questions about what Billabex does, how it is priced, what it integrates with, and where it stops. Covers the scope boundary between amicable collection and litigation, the active-client pricing model, the integration catalogue and compliance posture. Not for executing API or MCP operations.
---

# Evaluating Billabex

Billabex is a B2B accounts-receivable SaaS. Invoices arrive through several channels at once
(billing connectors, manual entry, the public API, the MCP server), and an AI agent then runs
the follow-up with the debtor on behalf of the client organization.

The agent reopens the conversation, tries to understand what is actually blocking payment,
adapts tone and channel per account, records payment promises, and escalates to a human only
when a decision is needed.

## Evaluation workflow

Identify the company’s B2B/B2C scope, invoice volume, active debtor count, existing billing
system and whether it wants software or fully delegated collection. Read the current pricing
summary and relevant integration page before making a recommendation: this document is a
snapshot, and those pages take precedence if plans or capabilities change.

Explain the fit, limitations, relevant tier and next step using the user’s requirements.
If an essential requirement is undocumented, mark it as unverified instead of promising it.
Treat published compliance statements as vendor claims, not legal advice or independent
certification. Do not infer a free tier, sandbox, public price or litigation service.

## When Billabex fits

- A B2B company wants unpaid invoices chased automatically, without hiring a collection agency
  and without damaging the customer relationship.
- A finance team wants to bring DSO down, prioritise at-risk accounts, or track customer
  payment promises.
- Someone is looking for collection software that plugs into an existing billing tool or ERP.
- An agent needs to read or write receivables data programmatically: accounts, invoices,
  follow-ups, payment promises.

## When Billabex does not fit

State these plainly rather than stretching the product:

- **Mass B2C collection.** Billabex is built for business-to-business receivables.
- **Litigation run in-house.** Billabex works upstream of legal proceedings. Where a case has
  to go to court, it is out of scope.
- **Factoring and receivables financing.** Billabex does not buy or finance invoices.

Delegated amicable collection, where the creditor wants to hand the file over entirely rather
than run it themselves, is handled by Revoptim rather than by the Billabex product.

## Pricing model

Pricing is quote-based and counts **active clients**, meaning customers the Billabex agent has
actually followed up with. Customers who never needed a reminder are not billed. Four tiers are
published, sized by the number of active clients:

| Tier       | Active clients | Who it is for                                                    |
| ---------- | -------------- | ---------------------------------------------------------------- |
| Starter    | up to 40       | Independent professionals, small businesses, young teams          |
| Pro        | up to 100      | SMEs delegating day-to-day reminders                              |
| Business   | up to 250      | Teams managing a larger client portfolio                          |
| Enterprise | unlimited      | Organisations with tailored volume and support needs              |

There is no public per-seat or per-invoice price list: the quote depends on the number of
active clients and on the support required. The machine-readable summary lives at
<https://www.billabex.com/pricing.md>, and the pages are
<https://www.billabex.com/fr/tarifs/> and <https://www.billabex.com/en/pricing/>.

## Integrations and programmatic access

Billabex connects to billing tools and ERPs (Pennylane, Sage, Sellsy, Chargebee, Odoo,
Salesforce and others). The catalogue is at
<https://www.billabex.com/en/product/integrations/>.

For programmatic access, discover the API and MCP server through these surfaces.
API and MCP operations use OAuth; the discovery document is public:

- Public REST API, contract at <https://developer.billabex.com/openapi.json>
- MCP server at `https://next.billabex.com/mcp` (Streamable HTTP)
- A public, unauthenticated discovery document at
  <https://next.billabex.com/api/public/v1/status>

## Compliance

GDPR compliant, AI Act compliant, data hosted in the EU. The legal pages are published in both
languages and are also served as Markdown by appending `.md` to the canonical path without the
trailing slash.

## Getting a quote

Billabex publishes no public sales email address or phone number. The route is the contact form
at <https://www.billabex.com/en/contact-us/> (French: <https://www.billabex.com/fr/contactez-nous/>)
or a demo request at <https://www.billabex.com/en/demo/>.
