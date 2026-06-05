# Feature Dependency Matrix
> **Project:** HomeStyle — Premium Design Furniture
> **Owner:** BA / PO
> **Last Updated:** 2026-06-05
> **Version:** 1.1

---

## Table of Contents

1. [How to Use This Document](#how-to-use-this-document)
2. [Dependency Strength Legend](#dependency-strength-legend)
3. [Feature Registry](#feature-registry)
4. [Dependency Matrix](#dependency-matrix)
5. [Feature Detail Cards](#feature-detail-cards)
   - [F-01.5 Token & Session Management](#f-015--token--session-management--critical-foundation)
   - [F-04.2 Payment Processing](#f-042--payment-processing-stripe--critical-path)
   - [F-04.4 Tax & Shipping Calculation](#f-044--tax--shipping-calculation--high-dependency)
   - [F-05.4 Order Status Machine](#f-054--order-status-machine--high-dependency)
   - [F-09.1 Admin Shipping Configuration](#f-091--admin-shipping-configuration--high-dependency)
   - [F-10.1 Trade Account Application Review](#f-101--trade-account-application-review--high-dependency)
   - [F-10.2 Trade Pricing Tiers Management](#f-102--trade-pricing-tiers-management--high-dependency)
6. [Cross-Feature Conflict Watchlist](#cross-feature-conflict-watchlist)
7. [Impact Assessment Quick Reference](#impact-assessment-quick-reference)
8. [Change History](#change-history)

---

## How to Use This Document

- **Before writing any CR impact assessment** — look up the changed feature, check its `Affects` column, review all downstream features
- When a feature changes → find it → check `Affects` → those features need review
- When adding a new feature → add a row AND update `Affects` on anything it depends on
- Dependency strength: **🔴 Strong** = breaks if dependency changes | **🟡 Weak** = partially affected

---

## Dependency Strength Legend

| Symbol | Meaning |
|---|---|
| 🔴 Strong | Breaks or produces wrong output if dependency changes |
| 🟡 Weak | Partially affected but may still function |

---

## Feature Registry

| Feature ID | Feature Name | Domain | Owner | Status |
|---|---|---|---|---|
| **F-01.1** | Customer Registration | Identity & Auth | Auth Team | Active |
| **F-01.2** | Customer Login (Email + Social OAuth) | Identity & Auth | Auth Team | Active |
| **F-01.3** | Password Reset | Identity & Auth | Auth Team | Active |
| **F-01.4** | Admin Login & 2FA | Identity & Auth | Auth / IT | Active |
| **F-01.5** | Token & Session Management | Identity & Auth | Auth Team | Active |
| **F-01.6** | Customer Account Profile Management | Identity & Auth | Auth Team | Active |
| **F-02.1** | Browse by Room & Category | Product Catalogue | Content Team | Active |
| **F-02.2** | Product Search & Filters | Product Catalogue | Backend Team | Active |
| **F-02.3** | Product Detail Page (PDP) | Product Catalogue | Frontend Team | Active |
| **F-02.4** | Delivery Availability Checker | Product Catalogue | Ops Team | Active |
| **F-02.5** | Admin Product & Catalogue Management | Product Catalogue | Content Team | Active |
| **F-03.1** | Shopping Cart | Cart & Inventory | Backend Team | Active |
| **F-03.2** | Guest Cart → Account Cart Merge | Cart & Inventory | Backend Team | Active |
| **F-03.3** | Wishlist Management | Cart & Inventory | Backend Team | Active |
| **F-04.1** | Checkout Flow (Address + Delivery) | Checkout & Payment | Backend Team | Active |
| **F-04.2** | Payment Processing (Stripe) | Checkout & Payment | Finance Team | Active |
| **F-04.3** | Discount Code Application | Checkout & Payment | Finance Team | Active |
| **F-04.4** | Tax & Shipping Calculation | Checkout & Payment | Finance / Ops | Active |
| **F-05.1** | Order Confirmation & History | Order Management | Backend Team | Active |
| **F-05.2** | Order Tracking | Order Management | Ops Team | Active |
| **F-05.3** | Admin Order Fulfillment | Order Management | Ops Team | Active |
| **F-05.4** | Order Status Machine | Order Management | Backend Team | Active |
| **F-06.1** | Return Request | Returns & Refunds | CS Team | Active |
| **F-06.2** | Admin Return Review | Returns & Refunds | Ops Team | Active |
| **F-06.3** | Refund Processing | Returns & Refunds | Finance Team | Active |
| **F-06.4** | Refund Status (Customer View) | Returns & Refunds | Frontend Team | Active |
| **F-07.1** | Review Submission | Reviews | Content Team | Active |
| **F-07.2** | Review Moderation | Reviews | Content Team | Active |
| **F-07.3** | Transactional Email System | Notifications | Backend Team | Active |
| **F-07.4** | Admin Alerts | Notifications | Backend Team | Active |
| **F-08.1** | Cookie Consent | Compliance | Legal / Frontend | Active |
| **F-08.2** | Data Erasure (Right to be Forgotten) | Compliance | Legal / Backend | Active |
| **F-08.3** | Data Retention Jobs | Compliance | Backend Team | Active |
| **F-09.1** | Admin Shipping Configuration | Admin & Config | Ops Team | Active |
| **F-09.2** | Admin Customer Management | Admin & Config | Ops Team | Active |
| **F-09.3** | Admin Inventory & Stock Management | Admin & Config | Ops Team | Active |
| **F-09.4** | Admin Dashboard & KPI Cards | Admin & Config | Ops Team | Active |
| **F-09.5** | Admin Promotions & Discount Codes | Admin & Config | Finance Team | Active |
| **F-10.1** | Trade Account Application Review | Trade Portal | Trade Team | Active |
| **F-10.2** | Trade Pricing Tiers Management | Trade Portal | Trade Team | Active |
| **F-10.3** | Account Manager Assignment | Trade Portal | Trade Team | Active |

---

## Dependency Matrix

| Feature | Depends On | Strength | Affects | Key Business Rules | Data / Tables |
|---|---|---|---|---|---|
| **F-01.1** Customer Registration | — | — | F-01.2, F-01.5, F-01.6, F-08.3 | BR-001, BR-002, BR-003, BR-005, BR-006 | `users`, `email_verifications` |
| **F-01.2** Customer Login | F-01.1, F-01.5 | 🔴 Strong | F-03.2, F-03.3, F-04.1, F-05.1, F-06.1, F-07.1 | BR-007, BR-008, BR-009, BR-011 | `users`, `refresh_tokens` |
| **F-01.3** Password Reset | F-01.1, F-01.5 | 🔴 Strong | F-01.2 | BR-012, BR-013 | `password_reset_tokens`, `refresh_tokens` |
| **F-01.4** Admin Login & 2FA | F-01.5 | 🔴 Strong | All F-09.x, F-10.x | BR-014, BR-015, BR-016, BR-017 | `admin_users`, `totp_secrets`, `refresh_tokens` |
| **F-01.5** Token & Session Management | — | — | ALL authenticated features | BR-007, BR-016, BR-018 | `refresh_tokens`, Redis session store |
| **F-01.6** Account Profile Management | F-01.2, F-01.5 | 🔴 Strong | F-04.1 | BR-019, BR-020 | `users`, `addresses` |
| **F-02.1** Browse by Room & Category | F-02.5 | 🔴 Strong | F-02.3, F-03.1 | BR-021, BR-022, BR-023, BR-024 | `collections`, `products`, `skus` |
| **F-02.2** Product Search | F-02.5 | 🔴 Strong | F-02.3, F-03.1 | BR-023, BR-027 | `search_index` (Elasticsearch/Algolia) |
| **F-02.3** Product Detail Page | F-02.5, F-10.2 | 🔴 Strong | F-03.1, F-03.3 | BR-024, BR-025, BR-026 | `products`, `skus`, `configurations` |
| **F-02.4** Delivery Availability Checker | F-09.1 | 🔴 Strong | F-04.1 | BR-028 | `serviceable_areas`, `shipping_zones` |
| **F-02.5** Admin Product Management | — | — | F-02.1, F-02.2, F-02.3, F-09.3 | BR-021, BR-023, BR-027 | `products`, `skus`, `collections`, `configurations` |
| **F-03.1** Shopping Cart | F-01.5, F-02.3 | 🔴 Strong | F-03.2, F-04.1 | BR-029, BR-030, BR-031 | `carts`, `cart_items`, Redis (guest cart) |
| **F-03.2** Guest Cart Merge | F-01.2, F-03.1 | 🔴 Strong | F-04.1 | BR-029 | `carts`, `cart_items`, Redis |
| **F-03.3** Wishlist Management | F-01.2, F-02.3 | 🔴 Strong | F-07.3 | BR-032, BR-033 | `wishlists`, `wishlist_items` |
| **F-04.1** Checkout Flow | F-01.5, F-03.1, F-04.4, F-09.1 | 🔴 Strong | F-04.2, F-04.3, F-05.1 | BR-034, BR-035, BR-036 | `checkout_sessions`, Redis (inventory holds) |
| **F-04.2** Payment Processing | F-04.1 | 🔴 Strong | F-05.1, F-05.4 | BR-037, BR-038 | `payment_intents` (Stripe-managed) |
| **F-04.3** Discount Code Application | F-04.1, F-09.5 | 🔴 Strong | F-04.2 | BR-039, BR-040, BR-041 | `discount_codes`, `code_redemptions` |
| **F-04.4** Tax & Shipping Calculation | F-09.1, F-10.2 | 🔴 Strong | F-04.1, F-04.2 | BR-042, BR-043, BR-044, BR-045 | `shipping_rates`, `tax_calculations` |
| **F-05.1** Order Confirmation & History | F-04.2, F-07.3 | 🔴 Strong | F-05.2, F-05.4, F-06.1 | BR-046, BR-047 | `orders`, `order_items` |
| **F-05.2** Order Tracking | F-05.1 | 🟡 Weak | — | BR-048 | `order_tracking_events` |
| **F-05.3** Admin Order Fulfillment | F-05.1, F-05.4 | 🔴 Strong | F-05.2, F-07.3 | — | `orders`, `fulfillment_records` |
| **F-05.4** Order Status Machine | F-05.1 | 🔴 Strong | F-05.2, F-05.3, F-06.1 | BR-046 | `orders` (status field) |
| **F-06.1** Return Request | F-05.1, F-05.4 | 🔴 Strong | F-06.2 | BR-049, BR-050 | `return_requests`, `return_items` |
| **F-06.2** Admin Return Review | F-06.1 | 🔴 Strong | F-06.3, F-07.3 | BR-051 | `return_requests` |
| **F-06.3** Refund Processing | F-06.2, F-04.2 | 🔴 Strong | F-05.4, F-06.4, F-07.3 | BR-052, BR-053, BR-054 | `refunds` (Stripe), `orders` |
| **F-06.4** Refund Status (Customer View) | F-06.3, F-05.1 | 🟡 Weak | — | — | `refunds`, `orders` |
| **F-07.1** Review Submission | F-05.1, F-01.2 | 🔴 Strong | F-07.2 | BR-055, BR-056, BR-058 | `reviews` |
| **F-07.2** Review Moderation | F-07.1 | 🔴 Strong | F-02.3 | BR-057 | `reviews` |
| **F-07.3** Transactional Email System | — | — | F-05.1, F-05.2, F-06.3, F-10.1 | BR-059 | `email_templates`, `email_queue` (BullMQ) |
| **F-07.4** Admin Alerts | F-07.3 | 🟡 Weak | — | — | `admin_alerts` |
| **F-08.1** Cookie Consent | — | — | Analytics & marketing scripts | BR-060, BR-061 | `cookie_consents` |
| **F-08.2** Data Erasure | F-01.5, F-01.6 | 🔴 Strong | F-08.3, F-09.2 | BR-062, BR-063 | `users`, all PII tables |
| **F-08.3** Data Retention Jobs | F-08.2 | 🟡 Weak | — | BR-006, BR-063, BR-064, BR-065 | All tables (retention policies) |
| **F-09.1** Admin Shipping Configuration | — | — | F-02.4, F-04.4, F-05.6 | BR-045, BR-066, BR-067, BR-077–083 | `shipping_zones`, `shipping_rates`, `whiteglove_partners`, `carrier_profiles`, `green_delivery_zones` |
| **F-09.2** Admin Customer Management | F-01.5 | 🔴 Strong | F-08.2 | BR-068 | `users` |
| **F-09.3** Admin Inventory Management | F-02.5, **Velocity Job** | 🔴 Strong | F-02.3, F-04.1, **F-09.4** | BR-031, **BR-084–093** | `inventory_records`, `sku_stock`, **`inventory_threshold_config`**, **`sales_velocity_daily`** |
| **F-09.4** Admin Dashboard & KPIs | F-05.1, F-10.1, **F-09.1**, **F-09.3** | 🟡 Weak | — | — | Aggregated from orders, returns, trade, **green_delivery_zones**, **inventory_threshold_config** |
| **F-09.5** Admin Promotions & Discounts | — | — | F-04.3 | BR-069, BR-070 | `discount_codes`, `code_redemptions` |
| **F-10.1** Trade Account Review | F-01.5, F-10.2, F-10.3, F-07.3 | 🔴 Strong | F-01.2, F-03.3, F-04.4 | BR-074, BR-075 | `trade_accounts`, `trade_documents` |
| **F-10.2** Trade Pricing Tiers | F-10.1 | 🔴 Strong | F-02.3, F-04.1, F-04.4 | BR-071, BR-072, BR-073 | `trade_tiers`, `trade_price_overrides` |
| **F-10.3** Account Manager Assignment | F-10.1 | 🟡 Weak | — | BR-076 | `account_managers`, `am_assignments` |

---

## Feature Detail Cards

---

### F-01.5 | Token & Session Management ⚠️ CRITICAL FOUNDATION

| Field | Detail |
|---|---|
| **Domain** | Identity & Auth |
| **Owner** | Auth Team |
| **Status** | Active |
| **Description** | Issues, rotates, and revokes JWT access and refresh tokens for all customer and admin sessions |

**Depends On** — *(none — foundational)*

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| ALL authenticated features | Customer/Admin | 🔴 Strong | Every authenticated API call requires a valid token from this feature |

**Business Rules**
- `BR-007` — Customer token lifetimes (access 15min, refresh 30d)
- `BR-016` — Admin token lifetimes (access 15min, refresh 8h)
- `BR-018` — Refresh token reuse detection → full family revocation

**Data / Tables Involved**
- `refresh_tokens` — stores hashed refresh tokens
- Redis — session cache, lockout counters

**Teams to Notify on Change**
- [ ] Auth Team — core token logic change
- [ ] Security Team — any TTL or rotation changes
- [ ] All feature teams — any API auth contract change

**CR Impact Checklist**
- [ ] Do downstream authenticated features still work with new token structure?
- [ ] Are cookie attributes (httpOnly, SameSite, Secure) preserved?
- [ ] Is token rotation still atomic?
- [ ] Are lockout counters and TTLs still correct in Redis?
- [ ] Does the security event log still write on reuse detection?

---

### F-04.2 | Payment Processing (Stripe) ⚠️ CRITICAL PATH

| Field | Detail |
|---|---|
| **Domain** | Checkout & Payment |
| **Owner** | Finance / Backend Team |
| **Status** | Active |
| **Description** | Handles Stripe PaymentIntent creation, webhook processing, and order creation trigger |

**Depends On**

| Feature ID | Feature Name | Strength | Why |
|---|---|---|---|
| F-04.1 | Checkout Flow | 🔴 Strong | PaymentIntent requires locked checkout session data (price, inventory hold, address) |

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| F-05.1 | Order Confirmation | 🔴 Strong | Order record is only created on `payment_intent.succeeded` webhook |
| F-05.4 | Order Status Machine | 🔴 Strong | Initial status set by payment webhook |
| F-06.3 | Refund Processing | 🔴 Strong | Refunds use the original PaymentIntent ID |

**Business Rules**
- `BR-037` — Order created on webhook only
- `BR-038` — No payment data stored
- `BR-039` — Discount code re-validated at PaymentIntent creation

**Data / Tables Involved**
- Stripe (PaymentIntents, Refunds — external)
- `checkout_sessions` — checkout data snapshot
- `orders` — created on webhook

**Teams to Notify on Change**
- [ ] Finance Team — any payment method or webhook change
- [ ] Ops Team — order creation logic
- [ ] Security Team — PCI DSS compliance implications

**CR Impact Checklist**
- [ ] Is the idempotency key logic still correct?
- [ ] Does `payment_intent.succeeded` webhook still trigger order creation?
- [ ] Are available payment methods per market (US/EU/UK) still correct?
- [ ] Do refunds still reference the correct PaymentIntent?

---

### F-04.4 | Tax & Shipping Calculation ⚠️ HIGH DEPENDENCY

| Field | Detail |
|---|---|
| **Domain** | Checkout & Payment |
| **Owner** | Finance / Ops Team |
| **Status** | Active |
| **Description** | Calculates tax (Stripe Tax / TaxJar) and shipping rates per market, applying EU VAT rules and trade VAT exemption |

**Depends On**

| Feature ID | Feature Name | Strength | Why |
|---|---|---|---|
| F-09.1 | Shipping Configuration | 🔴 Strong | All rate data sourced from admin-configured shipping zones and rates |
| F-10.2 | Trade Pricing Tiers | 🔴 Strong | VAT exemption flag and trade status required for EU trade checkout |

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| F-04.1 | Checkout Flow | 🔴 Strong | Tax and shipping displayed in delivery step and order summary |
| F-04.2 | Payment Processing | 🔴 Strong | Authoritative tax calculation feeds PaymentIntent amount |

**Business Rules**
- `BR-042` — Tax is authoritative at PaymentIntent
- `BR-043` — EU VAT included in price; US tax added at checkout
- `BR-044` — Trade VAT exemption + Reverse Charge
- `BR-045` — Shipping rate formula

**Data / Tables Involved**
- `shipping_rates`, `shipping_zones` — rate lookup
- `tax_calculations` — snapshot for audit
- `trade_accounts` — VAT exemption flag

**Teams to Notify on Change**
- [ ] Finance Team — any tax rate or VAT rule change
- [ ] Trade Team — changes to trade VAT exemption logic
- [ ] Ops Team — shipping rate formula changes

**CR Impact Checklist**
- [ ] Does EU VAT display still show inclusive pricing?
- [ ] Does US tax still appear as a separate checkout line item?
- [ ] Is trade VAT exemption still correctly applied (only EU, only verified)?
- [ ] Does white-glove surcharge still apply correctly?
- [ ] Are rate changes non-retroactive (locked sessions unaffected)?

---

### F-05.4 | Order Status Machine ⚠️ HIGH DEPENDENCY

| Field | Detail |
|---|---|
| **Domain** | Order Management |
| **Owner** | Backend Team |
| **Status** | Active |
| **Description** | Enforces valid state transitions for all orders, from creation through fulfillment, returns, and refunds |

**Depends On**

| Feature ID | Feature Name | Strength | Why |
|---|---|---|---|
| F-05.1 | Order Confirmation | 🔴 Strong | Initial state set at order creation |

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| F-05.2 | Order Tracking | 🔴 Strong | Tracking display depends on current order state |
| F-05.3 | Admin Order Fulfillment | 🔴 Strong | Fulfillment actions trigger state transitions |
| F-06.1 | Return Request | 🔴 Strong | Return eligibility depends on status being 'delivered' |
| F-06.3 | Refund Processing | 🔴 Strong | Status set to 'refunded' on refund webhook |

**Business Rules** — State transitions are the governing logic for all order, return, and refund flows

**Data / Tables Involved**
- `orders` (status field + transition audit log)

**Teams to Notify on Change**
- [ ] Ops Team — fulfillment workflow
- [ ] CS Team — return eligibility check
- [ ] Finance Team — refund status update

**CR Impact Checklist**
- [ ] Do all downstream features that read order status still get the expected values?
- [ ] Is the return eligibility check (status = 'delivered') still correct?
- [ ] Does refund webhook correctly set status to 'refunded'?

---

### F-09.1 | Admin Shipping Configuration ⚠️ HIGH DEPENDENCY

| Field | Detail |
|---|---|
| **Domain** | Admin & Config |
| **Owner** | Ops Team |
| **Status** | Active |
| **Spec Version** | v3.0 (updated 2026-06-05) |
| **Description** | Admin interface to configure shipping zones, rates, free-shipping thresholds, white-glove delivery partners, **carrier profiles (weight/dim limits, green certification)**, and **EU Green Delivery zones** |

**Depends On** — *(none — configuration source)*

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| F-02.4 | Delivery Availability Checker | 🔴 Strong | Zone coverage data determines delivery availability and estimate |
| F-04.4 | Tax & Shipping Calculation | 🔴 Strong | All rate calculations read from shipping zones/rates tables |
| **F-05.6** | **Warehouse Pick, Pack & Ship** | 🔴 Strong | **Carrier auto-assignment engine reads `carrier_profiles` at label generation; CARRIER_UNRESOLVED blocks dispatch** |
| **F-09.4** | **Admin Dashboard & KPIs** | 🟡 Weak | **Green Delivery Ratio KPI card per zone sourced from green_delivery_zones** |

**Business Rules**
- `BR-045` — Shipping rate formula
- `BR-066` — Rate changes non-retroactive
- `BR-067` — Soft-delete only
- `BR-028` — White-glove eligibility
- `BR-077` — Carrier weight/dimension eligibility check *(new)*
- `BR-078` — DPD EU parcel weight ceiling (≤ 31.5 kg) *(new)*
- `BR-079` — LTL/white-glove routing for heavy furniture *(new)*
- `BR-080` — Carrier auto-assignment with manual override *(new)*
- `BR-081` — CARRIER_UNRESOLVED blocks label generation *(new)*
- `BR-082` — Green Delivery zone preferential routing *(new)*
- `BR-083` — Green Delivery KPI target & monitoring *(new)*

**Data / Tables Involved**
- `shipping_zones`, `shipping_rates`, `whiteglove_partners`
- `carrier_profiles` *(new)* — weight/dim limits, green certification per carrier
- `green_delivery_zones` *(new)* — EU urban postal prefixes + KPI targets
- `carrier_assignment_log` *(new)* — audit log of auto-selected vs. operator-chosen carriers

**Teams to Notify on Change**
- [ ] Finance Team — any rate change affects checkout cost; green KPI affects EU tax rebate revenue
- [ ] Ops Team — white-glove partner coverage changes; carrier profile changes
- [ ] Logistics Director — green delivery zone config and KPI target changes
- [ ] Warehouse Team — carrier auto-assignment logic changes at pack step (F-05.6)

**CR Impact Checklist**
- [ ] Are rate changes non-retroactive (locked checkouts and confirmed orders unaffected)?
- [ ] Does the free-shipping threshold still display correctly at checkout?
- [ ] Does white-glove eligibility logic still use the correct weight threshold and zone coverage?
- [ ] Does carrier auto-assignment correctly enforce weight/dim limits at F-05.6 pack-verify?
- [ ] Does CARRIER_UNRESOLVED state correctly block label generation and trigger F-07.4 alert?
- [ ] Does green delivery preferential routing activate correctly when zone ratio < target?
- [ ] Is `green_cert_reference` still required for all `green_certified = true` carriers?

---

### F-10.1 | Trade Account Application Review ⚠️ HIGH DEPENDENCY

| Field | Detail |
|---|---|
| **Domain** | Trade Portal |
| **Owner** | Trade Team |
| **Status** | Active |
| **Description** | Admin workflow to approve, reject, or suspend trade account applications, triggering downstream role + pricing + AM provisioning |

**Depends On**

| Feature ID | Feature Name | Strength | Why |
|---|---|---|---|
| F-10.2 | Trade Pricing Tiers | 🔴 Strong | Default tier assigned at approval |
| F-10.3 | Account Manager Assignment | 🔴 Strong | AM assigned at approval |
| F-07.3 | Transactional Email | 🔴 Strong | Welcome email and decision emails sent via email system |

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| F-01.2 | Customer Login | 🔴 Strong | Approval grants Verified Trade role; suspension revokes all sessions |
| F-03.3 | Wishlist Management | 🟡 Weak | Approval unlocks multi-project wishlist capability (max 20) |
| F-04.4 | Tax & Shipping Calculation | 🔴 Strong | Approval sets EU VAT-exempt flag used at checkout |

**Business Rules**
- `BR-074` — 5 business day review SLA
- `BR-075` — Approval triggers chain (role, VAT, tier, AM, email)

**Data / Tables Involved**
- `trade_accounts`, `trade_documents`, `users` (role field)

**Teams to Notify on Change**
- [ ] Finance Team — VAT exemption changes
- [ ] Trade Team — approval workflow changes
- [ ] Auth Team — role provisioning logic

**CR Impact Checklist**
- [ ] Does approval still trigger all 5 downstream actions (role, VAT, tier, AM, email)?
- [ ] Does suspension still immediately revoke all sessions?
- [ ] Is the 5 business day SLA still tracked and surfaced in F-09.4?

---

### F-10.2 | Trade Pricing Tiers Management ⚠️ HIGH DEPENDENCY

| Field | Detail |
|---|---|
| **Domain** | Trade Portal |
| **Owner** | Trade Team |
| **Status** | Active |
| **Description** | Admin configuration of trade discount tiers and per-customer/product overrides; server-resolves trade price on every read |

**Depends On**

| Feature ID | Feature Name | Strength | Why |
|---|---|---|---|
| F-10.1 | Trade Account Review | 🔴 Strong | Trade accounts must be approved before pricing applies |

**Affects**

| Feature ID | Feature Name | Strength | How it's affected |
|---|---|---|---|
| F-02.3 | Product Detail Page | 🔴 Strong | Trade price displayed on PDP for verified trade accounts |
| F-04.1 | Checkout Flow | 🔴 Strong | Trade price used in checkout calculations |
| F-04.4 | Tax & Shipping Calculation | 🔴 Strong | Minimum cart value for discount eligibility uses post-trade-discount subtotal |

**Business Rules**
- `BR-071` — Server-side only
- `BR-072` — Resolution order
- `BR-073` — Price formula
- `BR-044` — VAT exemption is separate from trade discount

**Data / Tables Involved**
- `trade_tiers`, `trade_price_overrides`, `sku_trade_prices`

**Teams to Notify on Change**
- [ ] Finance Team — any discount level change
- [ ] Trade Team — customer-specific overrides
- [ ] Frontend Team — cache invalidation

**CR Impact Checklist**
- [ ] Are pricing changes non-retroactive (locked checkouts unaffected)?
- [ ] Is cache invalidation triggered for affected SKUs / customers?
- [ ] Does the resolution priority order still work (customer-product → customer-category → tier-category → tier baseline)?

---

## Cross-Feature Conflict Watchlist

> Review this section whenever a CR touches any of these features.

| Feature A | Feature B | Conflict Risk | Description | Last Reviewed |
|---|---|---|---|---|
| F-04.3 Discount Codes | F-10.2 Trade Pricing | High | Minimum cart value evaluated after trade discount. Stacking rules (OD-04) still pending. Changes to either can produce unintended combined discounts. | 2026-06-02 |
| F-04.4 Tax Calculation | F-10.1 Trade Approval | High | EU VAT exemption flag set at trade approval. If VAT validation or approval logic changes, tax calculation for EU trade orders will be wrong. | 2026-06-02 |
| F-05.4 Order Status Machine | F-06.1 Return Request | High | Return eligibility depends on order status = 'delivered'. Any change to valid status values or naming must be reflected in return eligibility check. | 2026-06-02 |
| F-08.2 Data Erasure | F-05.1 Order History | High | Erasure anonymises PII but retains financial records. If the order history display reads anonymised fields without graceful handling, customer-facing UI may break. | 2026-06-02 |
| F-09.1 Shipping Config | F-04.4 Tax & Shipping | High | Shipping rates non-retroactive, but locked checkout sessions must snapshot the rate at lock time. Any rate structure change (new fields, formula change) must be backward-compatible with existing snapshots. | 2026-06-02 |
| F-01.5 Token Management | F-09.2 Customer Management | Medium | Admin deactivating a customer must trigger immediate token revocation. If token revocation logic in F-01.5 changes, F-09.2 deactivation may silently fail to log out the customer. | 2026-06-02 |
| F-07.3 Email System | F-10.1 Trade Approval | Medium | Trade approval sends welcome email via F-07.3. Template changes or email system downtime will affect the approval completion experience. | 2026-06-02 |
| **F-09.1 Shipping Config** | **F-05.6 Pick, Pack & Ship** | High | **Carrier auto-assignment at pack-verify reads `carrier_profiles` from F-09.1. Changes to carrier weight/dim limits, green certification status, or zone coverage immediately affect which carriers are eligible at label generation. A carrier deactivation mid-wave can cause CARRIER_UNRESOLVED for in-progress shipments.** | 2026-06-05 |
| **F-09.1 Green Delivery Zones** | **F-09.4 Admin Dashboard** | Medium | **Green Delivery Ratio KPI card in F-09.4 reads rolling ratio from green_delivery_zones. Changes to zone postal prefixes or KPI target percentage will alter the dashboard KPI values immediately and may trigger or suppress Logistics Director alerts.** | 2026-06-05 |
| **F-09.3 Inventory Threshold Config** | **F-09.3 Background Velocity Job** | High | **Risk level (Critical/Medium/Low) classification depends on nightly velocity and monthly lead-time recomputation jobs. If either job fails silently, risk scores become stale — warehouse teams act on incorrect data. Job failure must surface in F-07.4 admin alerts.** | 2026-06-05 |
| **F-09.3 Threshold Governance** | **F-09.4 Admin Dashboard** | Medium | **F-09.4 must surface pending approval request count as a KPI card. If F-09.3 approval workflow changes (approval bands, approver roles), the F-09.4 dashboard task count and routing logic must be updated together.** | 2026-06-05 |

---

## Impact Assessment Quick Reference

> When a CR comes in, run these 5 steps.

**Step 1 — Find the feature being changed**
Look up the feature ID in the Dependency Matrix above.

**Step 2 — Check `Affects` column**
All listed features need impact review. Start with 🔴 Strong dependencies.

**Step 3 — Check `Business Rules` column**
All listed BRs may need updating in the Business Rule Catalog.

**Step 4 — Check Cross-Feature Conflict Watchlist**
Does this CR touch any known conflict pair?

**Step 5 — Notify teams**
Use the `Teams to Notify on Change` list from the feature's detail card.

---

## Change History

| Date | Feature ID | Change | Changed By | Reason |
|---|---|---|---|---|
| 2026-06-02 | All features | Initial matrix created | BA | Project documentation |
| 2026-06-05 | F-09.1 | Added F-05.6 as affected feature (🔴 Strong); added carrier_profiles and green_delivery_zones to data tables; added BR-077–083; updated detail card and CR checklist | BA | Carrier validation + Green Delivery routing extension |
| 2026-06-05 | F-09.3 | Added Velocity Job dependency; added F-09.4 as affected feature; added BR-084–093; added inventory_threshold_config and sales_velocity_daily tables | BA | Dynamic safety stock + risk-based replenishment extension |
| 2026-06-05 | F-09.4 | Added F-09.1 and F-09.3 as data sources for new KPI cards (Green Delivery Ratio, Risk Summary, Pending Approvals) | BA | New operational KPIs from logistics and inventory intelligence extensions |
| 2026-06-05 | Cross-Feature Watchlist | Added 4 new conflict pairs: F-09.1↔F-05.6, F-09.1 Green Zones↔F-09.4, F-09.3 Threshold Config↔Velocity Job, F-09.3 Governance↔F-09.4 | BA | New dependencies introduced by F-09.1 and F-09.3 extensions |
