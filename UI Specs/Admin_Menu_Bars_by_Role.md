# HomeStyle — Admin Menu Bars by Role

```
Document Type:   Menu / IA Specification (input to UI screen design)
Version:         0.1
Status:          Draft
Author:          BA Team
Last Updated:    2026-05-06
Stakeholders:    Product Owner, UX Lead, Frontend Lead, Admin Engineering, Trade Ops, Customer Ops, Content Team
```

## 1. Purpose

Defines the left-hand navigation (menu bar) for each admin role on the HomeStyle Admin Panel.
This is the source of truth used by UX/UI to enumerate the screens that must be designed and built per role. Permissions follow Section 2 of the BA skill and the Module Access Summary in `domain-rules.md`.

Conventions used below:

- **L1** = top-level menu group (collapsible). **L2** = sub-item (a screen).
- Each L2 item maps 1:1 to a screen (or a screen + drill-down detail page).
- Screen IDs follow `SCR-<ROLE>-<NN>` for traceability into the SRS.
- Role short codes: `SA` = Super Admin, `TA` = Admin — Trade, `OA` = Admin — Operations, `CA` = Admin — Content.
- All admin sessions re-authenticate after 30 min idle and every action is audit-logged (per domain-rules §Admin Panel — Shared Rules).

---

## 2. Super Admin — Menu Bar

> **Scope:** Full access to every admin module. Only role that can edit Roles & Permissions and System Settings (BR-011).

| L1 Group | L2 Item | Screen ID | Notes |
|---|---|---|---|
| Dashboard | Overview | SCR-SA-01 | Cross-module KPIs (sales, trade activation, returns, inventory health) |
| Users | All Customers | SCR-SA-02 | B2C + Trade list, search, filter, suspend |
|  | Trade Accounts | SCR-SA-03 | Verified trade accounts (read/write) |
|  | Admin Users | SCR-SA-04 | Create/disable admin accounts |
|  | Roles & Permissions | SCR-SA-05 | RBAC editor — **SA only** (BR-011) |
|  | Audit Log | SCR-SA-06 | Actor / action / timestamp / entity |
| Trade Portal | Applications Queue | SCR-SA-07 | Approve/reject (shared with TA) |
|  | Trade Pricing | SCR-SA-08 | Tier mgmt, bulk import |
|  | Account Manager Assignment | SCR-SA-09 | Map AMs → trade accounts |
|  | B2B Orders | SCR-SA-10 | All trade orders + invoices |
| Catalogue | Products | SCR-SA-11 | CRUD, configurations, SKUs |
|  | Categories | SCR-SA-12 | Tree management |
|  | Collections | SCR-SA-13 | Curated bundles |
|  | Configurator Options | SCR-SA-14 | Material / finish / size / fabric / leg style |
|  | Media Library | SCR-SA-15 | Images, spec PDFs (full-res for trade) |
| Orders | All Orders (B2C + B2B) | SCR-SA-16 | Unified queue |
|  | Order Detail | SCR-SA-17 | Drill-down |
|  | Manual Order | SCR-SA-18 | Phone/back-office order entry |
| Inventory | Stock Levels | SCR-SA-19 | Per SKU / warehouse |
|  | Stock Adjustments | SCR-SA-20 | Manual +/- with reason code |
|  | Restock Schedule | SCR-SA-21 | Lead-time mgmt |
| Shipping | Carriers | SCR-SA-22 | Carrier accounts |
|  | Zones & Rates | SCR-SA-23 | US / UK / EU zones |
|  | Shipments | SCR-SA-24 | Tracking dashboard |
| Returns & Refunds | Returns Queue | SCR-SA-25 | All RMAs |
|  | Refund Approvals | SCR-SA-26 | Tier-based per domain-rules §Returns |
| Promotions | Promo Codes | SCR-SA-27 |  |
|  | Campaigns | SCR-SA-28 | Sitewide/category |
| Content | CMS Pages | SCR-SA-29 |  |
|  | Blog Posts | SCR-SA-30 |  |
|  | Banners & Hero | SCR-SA-31 |  |
|  | Navigation Menus | SCR-SA-32 | Storefront IA |
|  | Email Templates | SCR-SA-33 | Transactional + marketing |
|  | Review Moderation | SCR-SA-34 |  |
| Reports | Sales & AOV | SCR-SA-35 |  |
|  | Conversion | SCR-SA-36 |  |
|  | Trade Activation | SCR-SA-37 |  |
|  | Returns Rate | SCR-SA-38 |  |
|  | Inventory Health | SCR-SA-39 |  |
| Financial Config | Currencies & FX | SCR-SA-40 | USD/GBP/EUR, 24h refresh |
|  | Tax & VAT Rules | SCR-SA-41 | UK 20%, EU per country, US state |
|  | Payment Gateways | SCR-SA-42 |  |
|  | Invoicing Settings | SCR-SA-43 | Trade invoice numbering |
| Compliance | GDPR Requests | SCR-SA-44 | Access / erasure (BR-012) |
|  | CCPA Opt-outs | SCR-SA-45 | "Do Not Sell" log (BR-013) |
|  | Consent Logs | SCR-SA-46 | Cookie / marketing consent |
| System Settings | General | SCR-SA-47 | Brand, locales |
|  | Integrations & API Keys | SCR-SA-48 |  |
|  | Webhooks | SCR-SA-49 |  |
|  | Feature Flags | SCR-SA-50 |  |
|  | Backup & Data Export | SCR-SA-51 |  |
| My Account | Profile | SCR-SA-52 |  |
|  | Sign out | — |  |

---

## 3. Admin — Trade — Menu Bar

> **Scope:** Trade applications, trade pricing, AM assignment, oversight of B2B orders. **No** access to B2C orders, inventory, shipping config, content, or system settings.

| L1 Group | L2 Item | Screen ID | Notes |
|---|---|---|---|
| Dashboard | Trade Overview | SCR-TA-01 | Applications pending, B2B orders today, trade GMV |
| Applications | Pending Queue | SCR-TA-02 | Review docs, VAT number, business type |
|  | Approved | SCR-TA-03 | Read-only history |
|  | Rejected | SCR-TA-04 | Includes 30-day reapply lock state |
|  | Application Detail | SCR-TA-05 | Approve / reject (BR-005); triggers email |
| Trade Accounts | Verified Accounts | SCR-TA-06 | List, search, deactivate |
|  | Account Detail | SCR-TA-07 | Profile, VAT number, AM, project wishlists |
|  | Account Manager Assignment | SCR-TA-08 | Map AM ↔ account |
| Trade Pricing | Price Tiers | SCR-TA-09 | Separate price tier (not % discount, per domain-rules) |
|  | Per-SKU Trade Price | SCR-TA-10 | Override grid |
|  | Bulk Import / Export | SCR-TA-11 | CSV |
| B2B Orders | All B2B Orders | SCR-TA-12 | Trade orders only |
|  | Order Detail | SCR-TA-13 | View only — fulfilment is owned by Ops |
|  | Invoices | SCR-TA-14 | Formal invoice generation |
| Reports | B2B Sales | SCR-TA-15 |  |
|  | Trade Activation Funnel | SCR-TA-16 | Application → approval → first order |
|  | AM Performance | SCR-TA-17 |  |
| My Account | Profile | SCR-TA-18 |  |
|  | Sign out | — |  |

---

## 4. Admin — Operations — Menu Bar

> **Scope:** Orders, inventory, shipping, returns, customer management. **No** financial config, system settings, content, or trade pricing (BR-009).

| L1 Group | L2 Item | Screen ID | Notes |
|---|---|---|---|
| Dashboard | Ops Overview | SCR-OA-01 | Open orders, low-stock alerts, in-flight returns |
| Orders | All Orders | SCR-OA-02 | B2C + B2B fulfilment view |
|  | Order Detail | SCR-OA-03 | Edit shipping, cancel, refund (within Ops scope) |
|  | Manual Order | SCR-OA-04 | Phone-in / customer service order |
|  | Pending Fulfilment | SCR-OA-05 | Pick / pack queue |
| Inventory | Stock Levels | SCR-OA-06 | Per SKU / warehouse |
|  | Stock Adjustments | SCR-OA-07 | Reason-coded |
|  | Restock Alerts | SCR-OA-08 | Low-stock thresholds |
|  | Configurator OOS Status | SCR-OA-09 | Visibility flag for OOS configurations |
| Shipping | Shipments | SCR-OA-10 | Track / re-print labels |
|  | Carrier Accounts (read-only) | SCR-OA-11 | View only — config owned by SA |
|  | Delivery Exceptions | SCR-OA-12 | Failed delivery, reschedule |
| Returns & Refunds | Returns Queue | SCR-OA-13 | RMA inbox |
|  | Return Detail | SCR-OA-14 | Apply refund tier (full / 85% / none) |
|  | Refund Processing | SCR-OA-15 | 5–10 business days SLA |
|  | Custom-Item Exceptions | SCR-OA-16 | Defective configured items |
| Customers | Customer List | SCR-OA-17 | Search, view, suspend |
|  | Customer Detail | SCR-OA-18 | Orders, addresses, support history |
|  | Support Cases | SCR-OA-19 | Tickets / notes |
| Reports | Order Volume | SCR-OA-20 |  |
|  | Fulfilment SLA | SCR-OA-21 |  |
|  | Returns Rate | SCR-OA-22 |  |
|  | Inventory Turnover | SCR-OA-23 |  |
| My Account | Profile | SCR-OA-24 |  |
|  | Sign out | — |  |

---

## 5. Admin — Content — Menu Bar

> **Scope:** Catalogue (product master data, categories, configurator options), CMS, blog, banners, navigation, promotions, email templates, review moderation. **No** orders, customers, financial data (BR-010).

| L1 Group | L2 Item | Screen ID | Notes |
|---|---|---|---|
| Dashboard | Content Overview | SCR-CA-01 | Pending reviews, scheduled campaigns, draft pages |
| Catalogue | Products | SCR-CA-02 | Master data, copy, media |
|  | Product Detail / Editor | SCR-CA-03 | Variants, lead time, descriptions |
|  | Categories | SCR-CA-04 | Tree mgmt |
|  | Collections | SCR-CA-05 | Curated bundles |
|  | Configurator Options | SCR-CA-06 | Material / finish / size / fabric / leg style |
|  | Spec PDFs | SCR-CA-07 | Upload / replace; full-res served only to Trade (BR-008) |
|  | Media Library | SCR-CA-08 | Images, video, alt-text |
| Promotions | Promo Codes | SCR-CA-09 |  |
|  | Campaigns | SCR-CA-10 | Sitewide / category / collection |
|  | Scheduled Banners | SCR-CA-11 | Time-windowed |
| Content | CMS Pages | SCR-CA-12 | About, FAQ, T&Cs |
|  | Blog Posts | SCR-CA-13 |  |
|  | Banners & Hero | SCR-CA-14 |  |
|  | Navigation Menus | SCR-CA-15 | Storefront IA |
|  | Landing Pages | SCR-CA-16 | Campaign LPs |
| Email & Notifications | Transactional Templates | SCR-CA-17 | Order confirmation, shipping, return |
|  | Marketing Templates | SCR-CA-18 |  |
|  | Notification Settings | SCR-CA-19 | Trigger mapping (no PII access) |
| Reviews | Pending Moderation | SCR-CA-20 |  |
|  | Approved | SCR-CA-21 |  |
|  | Rejected / Reported | SCR-CA-22 |  |
| My Account | Profile | SCR-CA-23 |  |
|  | Sign out | — |  |

---

## 6. Cross-Role Menu Access Matrix

Legend: `●` = full access · `◐` = read-only / scoped · `—` = no access

| Menu Group | SA | TA | OA | CA |
|---|:-:|:-:|:-:|:-:|
| Dashboard (role-scoped) | ● | ● | ● | ● |
| Customers — list/detail | ● | — | ● | — |
| Trade Accounts | ● | ● | — | — |
| Admin Users | ● | — | — | — |
| Roles & Permissions | ● | — | — | — |
| Audit Log | ● | — | — | — |
| Trade Applications | ● | ● | — | — |
| Trade Pricing | ● | ● | — | — |
| AM Assignment | ● | ● | — | — |
| Catalogue (Products / Categories / Collections) | ● | — | — | ● |
| Configurator Options | ● | — | ◐ (OOS view) | ● |
| Media Library | ● | — | — | ● |
| Spec PDFs | ● | — | — | ● |
| Orders (B2C) | ● | — | ● | — |
| Orders (B2B) | ● | ◐ (view) | ● | — |
| Manual Order Entry | ● | — | ● | — |
| Inventory | ● | — | ● | — |
| Shipping — Carriers / Zones | ● | — | ◐ (view) | — |
| Shipments / Tracking | ● | — | ● | — |
| Returns & Refunds | ● | — | ● | — |
| Promotions | ● | — | — | ● |
| CMS / Blog / Banners / Nav | ● | — | — | ● |
| Email Templates | ● | — | — | ● |
| Review Moderation | ● | — | — | ● |
| Reports — Sales / Conversion | ● | — | — | — |
| Reports — Trade | ● | ● | — | — |
| Reports — Ops (orders, returns, inventory) | ● | — | ● | — |
| Financial Config (Currencies, Tax, Payments, Invoicing) | ● | — | — | — |
| Compliance — GDPR / CCPA / Consent | ● | — | — | — |
| System Settings (Integrations, Flags, Webhooks) | ● | — | — | — |

---

## 7. UX / IA Notes for Screen Design

1. **Same shell, different menu** — all four roles share the same admin chrome (top bar with environment, search, notifications, user menu). The left rail is what differs by role.
2. **Lazy-load groups** — L1 groups are collapsed by default except the user's most-used group (Dashboard for SA, Applications for TA, Orders for OA, Catalogue for CA).
3. **Deep-link safety** — if a user types a URL outside their role's scope, render the standard 403 admin screen, do not silently redirect.
4. **Hidden ≠ disabled** — items the role cannot access are hidden, not greyed out, to keep the menu clean.
5. **Bulk actions** — every list-style screen needs a confirmation dialog for bulk operations (per domain-rules §Admin Panel).
6. **Audit hooks** — every CRUD screen must emit an audit event; design must reserve UI space for an "Activity" tab on detail pages.

---

## 8. Assumptions

- A1. Customer support tooling lives inside the Ops Admin menu (no separate "Support Admin" role today).
- A2. Trade Admin views B2B orders read-only; fulfilment is owned by Ops Admin.
- A3. Ops Admin can read carrier/zone config but cannot edit it (config = Super Admin scope).
- A4. Content Admin owns spec PDFs upload, but the access rule that limits full-res download to Trade users (BR-008) is enforced at the storefront, not the admin.
- A5. Notifications module sits under Content Admin because it concerns templates/copy, not transmission infrastructure.

---

## 9. Open Questions

- OQ-1. Should there be a dedicated "Customer Support Admin" role separate from Ops, given growing case volume?
- OQ-2. Should Trade Admin be able to **edit** customer records linked to a trade account, or only the trade-account profile?
- OQ-3. Where does **fraud review** live — Ops, a new Risk Admin role, or Super Admin only?
- OQ-4. Does Content Admin need access to a "Preview as Trade" toggle to see trade-only PDP elements?
- OQ-5. Confirm whether **Reports — Sales / Conversion** is SA-only or should also be visible (read-only) to Ops and Content for their respective contexts.
- OQ-6. Confirm checkout reservation timer duration (BR-006) — affects the Inventory dashboard's "soft-reserved" widget under Ops.

---

## 10. Traceability

- Roles defined in: BA Skill §2 (User Roles & Permissions)
- Module access matrix sourced from: `references/domain-rules.md` §Admin Panel — Module Access Summary
- Business Rules referenced: BR-005, BR-008, BR-009, BR-010, BR-011, BR-012, BR-013
