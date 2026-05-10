---
company: Zendesk
role: Senior Software Engineer → Technical Lead
location: Copenhagen, Denmark
start: 2023-03
end: 2025-04
type: full-time
website: https://www.zendesk.com
featured: true
cv: true
order: 3
links:
  - label: Advanced Customer Data Privacy
    url: https://www.zendesk.com/service/ticketing-system/customer-data-privacy/
---

Co-built bring-your-own-key (BYOK) encryption from the ground up on a six-engineer team — a merger of the Advanced Encryption and Data Protection groups. Customers hold their own key encryption keys; data is encrypted at-rest and in-transit, with fine-grained control over who gets to decrypt what.

The hard part was retrofitting field-level encryption across dozens of backend services — including a large Rails monolith — without compromising the rich features customers depend on. Every search, filter, and analytics path had to keep working. A business unlock for regulated and enterprise customers; a long road of decision records, EnvoyFilter work at the Istio layer, key-rotation design, and gRPC integrations to get there.

Stepped up as Technical Lead for the merged Data Protection team in the final months. Also acted as Security Champion and Reliability Champion: SLAs, observability, CI pipelines (GitHub Actions, Jenkins), and the Spinnaker deployment automation.
