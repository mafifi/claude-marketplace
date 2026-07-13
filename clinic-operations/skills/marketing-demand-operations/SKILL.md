---
name: marketing-demand-operations
description: Inspect Dr Souphi marketing demand, attribution, gift vouchers, and enquiries through Souphi MCP marketing tools. Use when clinic staff ask about campaign performance, voucher sales, enquiry follow-up, conversion stage, or enquiry notes.
mcp_tools:
  - marketing.getAttributionSummary
  - marketing.listGiftVouchers
  - marketing.listEnquiries
  - marketing.setEnquiryStage
  - marketing.addEnquiryLog
---

# Marketing Demand Operations

Use these tools for marketing demand, voucher, attribution, and enquiry operations.

## Default workflow

1. Confirm the tenant and include `tenantId` in every tool call.
2. Use read tools first: `marketing.getAttributionSummary`, `marketing.listGiftVouchers`, or `marketing.listEnquiries`.
3. For enquiry mutations, use the exact `enquiryId` returned by `marketing.listEnquiries`.
4. Use `marketing.setEnquiryStage` only after inspecting the enquiry.
5. Use `marketing.addEnquiryLog` for append-only operator notes.

## Rules

- enquiries are conversion operations; inspect before stage changes
- notes are append-only and should be concrete
- do not infer patient identity from the authenticated operator
- do not treat voucher reads as permission to redeem, refund, or modify vouchers

## Relevant tools

- `marketing.getAttributionSummary`
- `marketing.listGiftVouchers`
- `marketing.listEnquiries`
- `marketing.setEnquiryStage`
- `marketing.addEnquiryLog`
