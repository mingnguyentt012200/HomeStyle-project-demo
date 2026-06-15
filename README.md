# 🛋️ HomeStyle — Enterprise Furniture Commerce & Operations Platform

> **BA Portfolio Project** · Business Analysis & Product Design
> **Market:** United States + Europe (B2C + B2B Trade)
> **Platform type:** Custom-built web application (Next.js storefront + admin panel + REST API)

---

## 📌 About This Project

HomeStyle is a **BA portfolio demonstration** simulating a real-world enterprise documentation workflow for a premium design furniture e-commerce platform. The platform supports multi-channel commerce, inventory management, order fulfillment, customer support, and administrative workflows across US and EU markets.

The full system spans ~40 features across 10 domains. **This repository contains the complete system-level BRD and feature specs.** My direct BA contribution covers the **Admin Operations domain** — inventory management, logistics automation, and operational reporting. Other domain specs (Auth, Checkout, Trade Portal, etc.) are included for system context and dependency traceability.

---

## 🎯 My BA Scope — What I Owned

I was responsible for the following features end-to-end (problem → requirements → business rules → API contracts → acceptance criteria):

### 🔑 Feature 1: Dynamic Safety Stock & Risk-Based Replenishment
**`/feature_specs/F_09.3_Admin_Inventory_Management.md`**

| | |
|---|---|
| **Business Problem** | A hardcoded ATP ≤ 5 threshold applied to all 20,000+ SKUs. A sofa with a 28-day lead time and a lamp with a 3-day lead time received the same alert. Warehouse teams began ignoring the dashboard entirely. |
| **Root Cause** | Not all low-stock situations carry the same business risk. Operational urgency depends on how fast a SKU sells *relative* to how long it takes to replenish. |
| **Solution** | Replaced the static threshold with a dynamic risk formula: `days_of_stock = ATP / sales_velocity`. Risk classified as Critical / Medium / Low against lead time. Safety stock configurable per category, SKU, and warehouse with governance controls. |
| **BA Techniques** | SQL analysis on purchase order receipts (lead time) and sales velocity data · Stakeholder interviews · Root cause analysis · RACI governance model · Business rule modeling · API contract design |

**Key design decisions:**
- Category-level defaults with SKU-level overrides and warehouse-specific configs
- Approval governance: < 10% change → auto-approved · 10–30% → Inventory Manager · > 30% → Finance Manager
- Immutable audit versioning on every threshold change
- Seasonal config support (effective date / expiration date)

---

### 🔑 Feature 2: Carrier Validation & Green Delivery Routing
**`/feature_specs/F_09.1_Admin_Shipping_Configuration.md`** *(Extension section)*

| | |
|---|---|
| **Business Problem** | Warehouse staff in Poland manually assigned carriers to EU orders without constraint enforcement. A 120 kg oak wardrobe was assigned to DPD EU (contractual max: 31.5 kg). DPD refused collection. 2-day delays. Trustpilot 1-star reviews. Additionally, the CEO introduced a KPI: 30% of EU urban deliveries (Berlin, Paris, Amsterdam) must use certified green carriers to qualify for EU tax rebates. |
| **Root Cause** | No system-enforced carrier eligibility check at dispatch. Green routing left entirely to individual warehouse judgment. |
| **Solution** | Carrier profile system with weight/dimension constraints drives automatic eligibility at pack-verify. Green-certified carriers ranked first for EU urban zones when rolling 30-day green ratio < 30% target. |
| **BA Techniques** | Stakeholder pain mapping (Logistics Director) · Business rule modeling · Data model design · API contract definition · KPI framework definition |

**Key design decisions:**
- `carrier_profiles` table with per-carrier weight, dimension, girth, and green certification data
- Auto-assignment engine at F-05.6 pack-verify step — blocks label generation if no eligible carrier found (`CARRIER_UNRESOLVED`)
- Manual operator override allowed but audit-logged with mandatory reason code
- Monthly CSV export for EU tax rebate submission

---

## 📁 Repository Structure

```
HomeStyle-project-demo/
│
├── HomeStyle_BRD_v4.md              ← Full system BRD (v4.8)
│                                       My contributions: §12.13 (BC-88–93),
│                                       §16.6 (TC-INV-03, TC-CAR-01, TC-GREEN-01),
│                                       §17.9 (BR-INV-11–16, BR-FUL-11–13),
│                                       §18.2 (4 new operational KPIs)
│
├── feature_specs/
│   ├── F_09.1_Admin_Shipping_Configuration.md   ← My work (Extension section v3.0)
│   ├── F_09.3_Admin_Inventory_Management.md     ← My work (Extension section v4.0)
│   ├── F_05.3_Admin_Order_Fulfilment.md         ← Context: validated scenarios
│   └── [Other specs — system context, not my authorship]
│
└── BPMN/
    └── [Process diagrams for system-wide flows]
```

**Start here if you're a reviewer:**
1. Read the two feature spec extension sections above (F-09.1 carrier validation, F-09.3 dynamic safety stock)
2. See the corresponding BRD sections: §17.9 BR-INV-11–16 and BR-FUL-11–13
3. The full BRD is available for system-level context

---

## 🛠️ BA Techniques Demonstrated

| Technique | Where Applied |
|---|---|
| **SQL Data Analysis** | Lead time analysis (`AVG(DATEDIFF)` on PO receipts) and sales velocity analysis to validate the risk formula before writing specs. Queries embedded in F-09.3 spec for dev/QA reference. |
| **Root Cause Analysis** | Both features started with a structured AS-IS → pain point → root cause → solution design flow |
| **Business Rule Modeling** | BR-INV-11–16, BR-FUL-11–13 — rules with rationale, enforcement point, and related-rule cross-references |
| **RACI Governance Design** | Threshold change approval matrix (warehouse vs. inventory manager vs. finance) |
| **API Contract Design** | Endpoint definitions, request/response shapes, validation rules, error codes, and permission matrices in every feature spec |
| **Acceptance Criteria** | Structured AC tables (criteria, test type, status) per feature |
| **Stakeholder Management** | Translated Logistics Director pain points into system requirements; resolved Finance Manager governance concern |
| **KPI Framework** | Defined Green Delivery Ratio, Inventory Risk Alert Accuracy, CARRIER_UNRESOLVED Rate, and Threshold Approval Cycle Time |
| **Impact Analysis** | Feature dependency matrix showing how F-09.1 changes propagate to F-05.6, F-04.4, and F-09.4 |

---

## 📐 Design Artefacts

| Artefact | Link |
|---|---|
| Figma Wireframes (v1) | [View on Figma](https://www.figma.com/design/AaDMTC6h17acKOp4Jv5Vem/Homestyle?node-id=0-1&t=8Q9n6dpgJ2iTMOhJ-1) |
| Figma Wireframes (v2 — in progress) | [View on Figma](https://www.figma.com/design/AaDMTC6h17acKOp4Jv5Vem/Homestyle?node-id=11-2&t=8Q9n6dpgJ2iTMOhJ-1) — 🚧 currently on it. Ops Admin and Content Admin wireframes completed; remaining screens in progress. |
| Customer Purchase Journey BPMN | [View on Lucidchart](https://lucid.app/lucidchart/239a29ac-57a5-4e17-b75f-f809a7116fd3/edit?viewport_loc=-6007%2C-270%2C10583%2C6141%2C0_0&invitationId=inv_9f726783-163b-403b-8ebf-25f613a55f35) |
| Return & Refund Journey BPMN | [View on Lucidchart](https://lucid.app/lucidchart/32687af9-be2e-456d-9b5e-a9d0f94b8be4/edit?viewport_loc=-5295%2C-993%2C5127%2C2713%2C0_0&invitationId=inv_31dd6994-3243-41d4-a51a-7ea42e341f09) |

---

## 🏗️ System Overview (Context)

The full HomeStyle platform covers 10 domains. I contributed to **Admin Operations (F-09.x)**. The other domains exist in this repo for completeness and dependency traceability — they reflect the broader system architecture I worked within, not my individual authorship.

| Domain | Features | My Involvement |
|---|---|---|
| 🔐 Identity & Auth | Registration, Login, 2FA, Sessions | Context only |
| 🏷️ Product & Catalogue | Browse, Search, PDP, Admin Catalogue | Context only |
| 🛒 Cart & Inventory | Cart, Guest Merge, Wishlist | Context only |
| 💳 Checkout & Payment | Checkout Flow, Stripe, Tax & Shipping Calc | Dependency: shipping config feeds F-04.4 |
| 📦 Order Management | Confirmation, Tracking, Fulfilment, Status Machine | Validated operational scenarios for F-05.3 |
| 🔄 Returns & Refunds | Return Request, Admin Review, Refund | Context only |
| ⭐ Reviews | Submission, Moderation | Context only |
| 🔒 Compliance (GDPR) | Cookie Consent, Data Erasure, Retention | Context only |
| ⚙️ **Admin Operations** | **Shipping Config, Inventory, Dashboard, Promotions** | **Full ownership — F-09.1 extension, F-09.3 extension** |
| 🤝 Trade Portal | Trade Account Review, Pricing Tiers, AM Assignment | Context only |

---

## 📋 Project Notes

- **No backend implementation** — this is a BA documentation portfolio, not a working application
- **No real payment data** — Stripe integration is specified at requirements level only
- The BRD, feature specs, and business rule catalog represent the type of artefacts I produce in a professional BA role
- All business scenarios, stakeholder pain points, and case studies are based on realistic EU logistics and e-commerce operational contexts

---

## 👤 Author

**Minh** — Business Analyst
Specialisation: Operations domain · Inventory intelligence · Logistics automation · EU market requirements
