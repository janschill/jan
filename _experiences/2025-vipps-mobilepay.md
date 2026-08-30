---
company: Vipps MobilePay
role: Senior Software Engineer
location: Oslo, Norway
start: 2025-08
end: 2026-05
type: full-time
website: https://vippsmobilepay.com
featured: true
cv: true
order: 2
links:
  - label: Sales API docs
    url: https://developer.vippsmobilepay.com/docs/APIs/sales-api/
---

A tiger team pulled from four teams across the merchant lifecycle domain, tasked with something the company had wanted for years but never shipped: the [Sales API](https://developer.vippsmobilepay.com/docs/APIs/sales-api/), giving accounting partners the real sales data they need to do real accounting for tens of thousands of merchants.

The team thinned out fast, with two engineers pulled onto other initiatives and a third out for a stretch, so the project needed someone to drive it and I took that on. I wrote the RFC and the decision records, kept stakeholders current, and shipped the first API and internal consumer in two months, in a language (C#) and a platform I'd never touched. We spent the next month hardening it, then integrated it with the mPOS product when my teammate returned.

Today it ingests around 3 million events a week and serves roughly 220k requests at a 22.6 ms mean for a growing set of accounting partners. No incidents since launch.
