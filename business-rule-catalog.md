# Business Rule Catalog
> **Project:** HomeStyle — Premium Design Furniture
> **Owner:** BA / PO
> **Last Updated:** 2026-06-05
> **Version:** 1.1

---

## Table of Contents

1. [How to Use This Document](#how-to-use-this-document)
2. [Status Legend](#status-legend)
3. [Rule Index](#rule-index)
4. [Rules by Domain](#rules-by-domain)
   - [🔐 Identity & Auth](#-domain-identity--auth) — BR-001 to BR-020
   - [🏷️ Product & Catalogue](#️-domain-product--catalogue) — BR-021 to BR-028
   - [🛒 Cart & Inventory](#-domain-cart--inventory) — BR-029 to BR-033
   - [💳 Checkout & Payment](#-domain-checkout--payment) — BR-034 to BR-045
   - [📦 Order & Fulfillment](#-domain-order--fulfillment) — BR-046 to BR-048
   - [🔄 Returns & Refunds](#-domain-returns--refunds) — BR-049 to BR-054
   - [⭐ Reviews](#-domain-reviews) — BR-055 to BR-058
   - [📧 Notifications](#-domain-notifications) — BR-059
   - [🔒 Compliance](#-domain-compliance) — BR-060 to BR-065
   - [⚙️ Admin & Config](#️-domain-admin--config) — BR-066 to BR-070
   - [🤝 Trade Portal](#-domain-trade-portal) — BR-071 to BR-076
   - [🚚 Logistics & Carrier](#-domain-logistics--carrier) — BR-077 to BR-083
   - [📊 Inventory Intelligence](#-domain-inventory-intelligence) — BR-084 to BR-093
5. [Change History](#change-history)

---

## How to Use This Document

- **Never delete** old rules — mark as `DEPRECATED` and create a new version
- Each rule has a unique ID in format `BR-XXX`
- When a CR affects a rule → update `Status` and `Last Modified`
- Link related rules in the `Related Rules` field

## Status Legend

| Status | Meaning |
|---|---|
| `ACTIVE` | Currently enforced |
| `PENDING` | Approved but not yet implemented |
| `DEPRECATED` | No longer active — kept for audit trail |
| `UNDER REVIEW` | Being discussed or changed |

---

## Rule Index

| Rule ID | Name | Domain | Status |
|---|---|---|---|
| BR-001 | Email Uniqueness | Identity & Auth | ACTIVE |
| BR-002 | Password Strength & Storage | Identity & Auth | ACTIVE |
| BR-003 | Account Verification Required for Login | Identity & Auth | ACTIVE |
| BR-004 | Verification Email Token Rules | Identity & Auth | ACTIVE |
| BR-005 | T&C Acceptance Mandatory | Identity & Auth | ACTIVE |
| BR-006 | Unverified Account Auto-Cleanup | Identity & Auth | ACTIVE |
| BR-007 | Customer Session Token Lifetimes | Identity & Auth | ACTIVE |
| BR-008 | Customer Login Lockout | Identity & Auth | ACTIVE |
| BR-009 | Login Error Non-Disclosure | Identity & Auth | ACTIVE |
| BR-010 | OAuth State Token (CSRF Protection) | Identity & Auth | ACTIVE |
| BR-011 | EU Social Login Hold | Identity & Auth | PENDING |
| BR-012 | Password Reset Token Rules | Identity & Auth | ACTIVE |
| BR-013 | Password Reset Invalidates All Sessions | Identity & Auth | ACTIVE |
| BR-014 | Admin 2FA Mandatory | Identity & Auth | ACTIVE |
| BR-015 | Admin Login Lockout | Identity & Auth | ACTIVE |
| BR-016 | Admin Session Token Lifetimes | Identity & Auth | ACTIVE |
| BR-017 | Admin Inactivity Timeout | Identity & Auth | ACTIVE |
| BR-018 | Refresh Token Reuse Detection | Identity & Auth | ACTIVE |
| BR-019 | Email Change Re-Verification | Identity & Auth | ACTIVE |
| BR-020 | Max Saved Addresses Per Customer | Identity & Auth | ACTIVE |
| BR-021 | Collection Hierarchy Max 3 Levels | Product & Catalogue | ACTIVE |
| BR-022 | Empty Collections Hidden from Nav | Product & Catalogue | ACTIVE |
| BR-023 | Archived Products Excluded Everywhere | Product & Catalogue | ACTIVE |
| BR-024 | Currency & VAT Display Rules | Product & Catalogue | ACTIVE |
| BR-025 | Spec PDF Presigned URL & Access Tiers | Product & Catalogue | ACTIVE |
| BR-026 | Lead Time Per Configuration | Product & Catalogue | ACTIVE |
| BR-027 | Search Index Freshness | Product & Catalogue | ACTIVE |
| BR-028 | White-Glove Delivery Eligibility | Product & Catalogue | ACTIVE |
| BR-029 | Guest Cart TTL | Cart & Inventory | ACTIVE |
| BR-030 | Cart Quantity Limit | Cart & Inventory | ACTIVE |
| BR-031 | Inventory Not Decremented on Add-to-Cart | Cart & Inventory | ACTIVE |
| BR-032 | Trade vs. Consumer Wishlist Limits | Cart & Inventory | ACTIVE |
| BR-033 | Back-in-Stock Notification SLA | Cart & Inventory | ACTIVE |
| BR-034 | Inventory Hold on Checkout Start | Checkout & Payment | ACTIVE |
| BR-035 | Checkout Session Max Duration | Checkout & Payment | ACTIVE |
| BR-036 | Lead Time Locked at Checkout | Checkout & Payment | ACTIVE |
| BR-037 | Order Created on Webhook Only | Checkout & Payment | ACTIVE |
| BR-038 | No Payment Data Stored | Checkout & Payment | ACTIVE |
| BR-039 | Discount Code Double Validation | Checkout & Payment | ACTIVE |
| BR-040 | One Discount Code Per Order | Checkout & Payment | ACTIVE |
| BR-041 | Trade-Only Code Enforcement | Checkout & Payment | ACTIVE |
| BR-042 | Tax Calculation Authoritative Timing | Checkout & Payment | ACTIVE |
| BR-043 | EU VAT Included; US Tax Added at Checkout | Checkout & Payment | ACTIVE |
| BR-044 | Trade VAT Exemption & Reverse Charge | Checkout & Payment | ACTIVE |
| BR-045 | Shipping Rate Formula | Checkout & Payment | ACTIVE |
| BR-046 | Order Reference Format | Order & Fulfillment | ACTIVE |
| BR-047 | Confirmation Email Dispatch SLA | Order & Fulfillment | ACTIVE |
| BR-048 | Dispatch Email SLA | Order & Fulfillment | ACTIVE |
| BR-049 | Return Window Default & Configurability | Returns & Refunds | ACTIVE |
| BR-050 | Made-to-Order Return Restriction | Returns & Refunds | ACTIVE |
| BR-051 | Return Review SLA (Admin) | Returns & Refunds | ACTIVE |
| BR-052 | Refund Processing SLA | Returns & Refunds | ACTIVE |
| BR-053 | Total Returns Resolution Target | Returns & Refunds | ACTIVE |
| BR-054 | Refund via Original PaymentIntent | Returns & Refunds | ACTIVE |
| BR-055 | Verified Purchase Required for Review | Reviews | ACTIVE |
| BR-056 | One Review Per Customer Per Product | Reviews | ACTIVE |
| BR-057 | Reviews Require Admin Approval | Reviews | ACTIVE |
| BR-058 | Review Submission Window | Reviews | ACTIVE |
| BR-059 | Email Retry Policy | Notifications | ACTIVE |
| BR-060 | No Non-Essential Cookies Before Consent | Compliance | ACTIVE |
| BR-061 | CCPA Do Not Sell Link (California) | Compliance | ACTIVE |
| BR-062 | GDPR Erasure 30-Day Deadline | Compliance | ACTIVE |
| BR-063 | Financial Records 7-Year Retention | Compliance | ACTIVE |
| BR-064 | Server Log Purge 90 Days | Compliance | ACTIVE |
| BR-065 | Audit Log Archive 3 Years | Compliance | ACTIVE |
| BR-066 | Shipping Rate Non-Retroactive | Admin & Config | ACTIVE |
| BR-067 | Soft-Delete Only for Shipping Config | Admin & Config | ACTIVE |
| BR-068 | Customer Hard-Delete Forbidden | Admin & Config | ACTIVE |
| BR-069 | Discount Code Immutable After Redemption | Admin & Config | ACTIVE |
| BR-070 | Discount Minimum Cart Value Basis | Admin & Config | ACTIVE |
| BR-071 | Trade Pricing Server-Side Only | Trade Portal | ACTIVE |
| BR-072 | Trade Price Resolution Order | Trade Portal | ACTIVE |
| BR-073 | Trade Price Formula | Trade Portal | ACTIVE |
| BR-074 | Trade Application Review SLA | Trade Portal | ACTIVE |
| BR-075 | Trade Approval Triggers | Trade Portal | ACTIVE |
| BR-076 | Every Active Trade Account Needs an AM | Trade Portal | ACTIVE |
| BR-077 | Carrier Weight/Dimension Eligibility Check | Logistics & Carrier | ACTIVE |
| BR-078 | DPD EU Parcel Weight Ceiling | Logistics & Carrier | ACTIVE |
| BR-079 | LTL/White-Glove Routing for Heavy Furniture | Logistics & Carrier | ACTIVE |
| BR-080 | Carrier Auto-Assignment with Manual Override | Logistics & Carrier | ACTIVE |
| BR-081 | CARRIER_UNRESOLVED Blocks Label Generation | Logistics & Carrier | ACTIVE |
| BR-082 | Green Delivery Zone Preferential Routing | Logistics & Carrier | ACTIVE |
| BR-083 | Green Delivery KPI Target & Monitoring | Logistics & Carrier | ACTIVE |
| BR-084 | Dynamic Safety Stock Configuration Scope | Inventory Intelligence | ACTIVE |
| BR-085 | Risk Level Derivation Formula | Inventory Intelligence | ACTIVE |
| BR-086 | Sales Velocity Computation | Inventory Intelligence | ACTIVE |
| BR-087 | Lead Time Computation | Inventory Intelligence | ACTIVE |
| BR-088 | Threshold Change Governance — Auto-Approval | Inventory Intelligence | ACTIVE |
| BR-089 | Threshold Change Governance — Manager Approval | Inventory Intelligence | ACTIVE |
| BR-090 | Threshold Change Governance — Finance Approval | Inventory Intelligence | ACTIVE |
| BR-091 | Threshold Change Audit & Versioning | Inventory Intelligence | ACTIVE |
| BR-092 | Seasonal Config Expiration & Reversion | Inventory Intelligence | ACTIVE |
| BR-093 | Historical Reports Use Active Config at Event Time | Inventory Intelligence | ACTIVE |

---

## Rules by Domain

---

### 🔐 Domain: Identity & Auth

---

#### BR-001 | Email Uniqueness

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Auth Team |
| **Source** | Feature Spec F-01.1 |

**Rule Statement**
> Every email address must be unique across all user accounts, checked case-insensitively.

**Detail**
`john@x.com` and `JOHN@X.COM` are treated as the same email. Uniqueness is enforced at submission server-side, not just on the frontend blur event.

**Applies To** — All user registration endpoints (customer + social OAuth)

**Exceptions** — *(None)*

**Related Rules** — `BR-003`, `BR-019`

**Related Features** — F-01.1, F-01.2, F-01.6

---

#### BR-002 | Password Strength & Storage

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Auth Team |
| **Source** | Feature Spec F-01.1 |

**Rule Statement**
> Passwords must be 8–128 characters with ≥1 uppercase, lowercase, and number; stored as bcrypt hash (cost 12); never stored in plaintext.

**Applies To** — Registration, password reset, password change

**Exceptions** — *(None)*

**Related Rules** — `BR-012`, `BR-013`

**Related Features** — F-01.1, F-01.3, F-01.6

---

#### BR-003 | Account Verification Required for Login

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.1, F-01.2 |

**Rule Statement**
> Accounts with `status='unverified'` cannot log in. Login is only permitted after the email verification link is clicked.

**Applies To** — All customer login attempts

**Exceptions** — Admin accounts managed separately via F-01.4

**Related Rules** — `BR-004`, `BR-006`

**Related Features** — F-01.1, F-01.2

---

#### BR-004 | Verification Email Token Rules

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.1 (TC-06) |

**Rule Statement**
> Verification link is a signed JWT, 24-hour expiry, single-use. Resend rate-limited to 3 per hour per email (Redis counter). Excess requests silently accepted to prevent enumeration.

**Formula / Logic**
```
Token TTL: 24 hours
Max resends: 3 / hour / email
Token invalidated in DB on first use
```

**Applies To** — Customer registration email flow

**Exceptions** — *(None)*

**Related Rules** — `BR-003`, `BR-012`

**Related Features** — F-01.1

---

#### BR-005 | T&C Acceptance Mandatory

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal / Auth Team |
| **Source** | Feature Spec F-01.1 |

**Rule Statement**
> T&C checkbox is mandatory at registration. Acceptance stored with `accepted_at` timestamp and `terms_version` for GDPR audit trail.

**Applies To** — Customer registration

**Related Features** — F-01.1, F-08.2

---

#### BR-006 | Unverified Account Auto-Cleanup

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Data Team |
| **Source** | Feature Spec F-01.1, F-08.3 (TC-31) |

**Rule Statement**
> Unverified accounts inactive for more than 30 days are soft-deleted by the nightly data retention job.

**Applies To** — `users` table where `status='unverified'`

**Related Rules** — `BR-003`, `BR-063`

**Related Features** — F-01.1, F-08.3

---

#### BR-007 | Customer Session Token Lifetimes

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.2, F-01.5 (TC-01, TC-02) |

**Rule Statement**
> Customer access tokens expire in 15 minutes; refresh tokens expire in 30 days with rotation on every use. Both stored in httpOnly cookies only — never localStorage.

**Formula / Logic**
```
Access token TTL: 15 minutes (TC-01)
Refresh token TTL: 30 days (TC-02), rotating
Storage: httpOnly, Secure, SameSite=Strict cookies
```

**Applies To** — All authenticated customer/trade sessions

**Exceptions** — Admin sessions: see `BR-016`

**Related Rules** — `BR-016`, `BR-018`

**Related Features** — F-01.2, F-01.5

---

#### BR-008 | Customer Login Lockout

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.2 (TC-10) |

**Rule Statement**
> After 5 consecutive failed login attempts, the account is locked for 15 minutes. Counter stored in Redis; resets on successful login.

**Formula / Logic**
```
Threshold: 5 attempts
Lockout duration: 15 minutes
Counter: Redis with TTL
```

**Applies To** — Customer and trade login

**Exceptions** — Admin lockout: stricter — see `BR-015`

**Related Rules** — `BR-009`, `BR-015`

**Related Features** — F-01.2

---

#### BR-009 | Login Error Non-Disclosure

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth / Security Team |
| **Source** | Feature Spec F-01.2 |

**Rule Statement**
> Failed login error messages must never indicate whether the email or the password was wrong. Generic message only: "Incorrect email or password."

**Applies To** — All login endpoints

**Related Features** — F-01.2, F-01.4

---

#### BR-010 | OAuth State Token (CSRF Protection)

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.2 (TC-08) |

**Rule Statement**
> OAuth state token is a random UUID per login attempt, stored in Redis with 10-minute TTL. State mismatch on callback rejects the request (CSRF protection).

**Applies To** — Google and Facebook OAuth flows

**Related Features** — F-01.2

---

#### BR-011 | EU Social Login Hold

| Field | Detail |
|---|---|
| **Status** | `PENDING` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal |
| **Source** | Feature Spec F-01.2 (OD-06) |

**Rule Statement**
> Google/Facebook social login must remain disabled on EU-facing routes until Legal confirms GDPR legal basis for data transfer.

**Applies To** — EU customer-facing OAuth buttons

**Notes / Open Questions**
- [ ] OD-06: Legal sign-off on GDPR legal basis for social login in EU still pending

**Related Features** — F-01.2

---

#### BR-012 | Password Reset Token Rules

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.3 (TC-07) |

**Rule Statement**
> Password reset link is a signed JWT with 1-hour expiry and single-use. A new request immediately invalidates any previous active token. Rate-limited to 3 requests per hour per email.

**Formula / Logic**
```
Token TTL: 1 hour
Max requests: 3 / hour / email (silent — same response shown regardless)
New request invalidates previous token immediately
```

**Applies To** — Password reset flow

**Related Rules** — `BR-002`, `BR-013`

**Related Features** — F-01.3

---

#### BR-013 | Password Reset Invalidates All Sessions

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth / Security Team |
| **Source** | Feature Spec F-01.3, F-01.5 |

**Rule Statement**
> Successful password reset immediately invalidates ALL active refresh tokens for the user across all devices, forcing re-authentication everywhere.

**Applies To** — Password reset, password change via account settings

**Related Rules** — `BR-007`, `BR-018`

**Related Features** — F-01.3, F-01.5, F-01.6

---

#### BR-014 | Admin 2FA Mandatory

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Security / IT |
| **Source** | Feature Spec F-01.4 |

**Rule Statement**
> 2FA (TOTP) is mandatory for all admin accounts. New admins cannot log in until 2FA is enrolled. Each TOTP code is single-use per 30-second window.

**Applies To** — All admin roles (Super Admin, Ops, Content, Trade)

**Related Rules** — `BR-015`, `BR-016`

**Related Features** — F-01.4

---

#### BR-015 | Admin Login Lockout

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Security / IT |
| **Source** | Feature Spec F-01.4 (TC-11) |

**Rule Statement**
> After 3 consecutive failed attempts (password or TOTP), admin account locked for 30 minutes. Super Admin notified by email. Super Admin can manually unlock.

**Formula / Logic**
```
Threshold: 3 attempts (stricter than customer BR-008)
Lockout duration: 30 minutes
Notification: Super Admin email alert on every lockout
```

**Applies To** — All admin login attempts

**Related Rules** — `BR-008`, `BR-014`

**Related Features** — F-01.4

---

#### BR-016 | Admin Session Token Lifetimes

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.4 (TC-03, TC-04, TC-05) |

**Rule Statement**
> Admin access token: 15 minutes. Admin refresh token: 8 hours (shorter than customer's 30 days due to elevated privileges). Admin inactivity timeout: 30 minutes with 5-minute warning modal.

**Formula / Logic**
```
Admin access token TTL: 15 minutes (TC-03)
Admin refresh token TTL: 8 hours (TC-04)
Inactivity timeout: 30 minutes (TC-05)
Warning modal: at 5 minutes remaining
```

**Applies To** — All admin sessions

**Exceptions** — Customer sessions: see `BR-007`

**Related Rules** — `BR-007`, `BR-014`

**Related Features** — F-01.4, F-01.5

---

#### BR-017 | Admin Auto-Save Drafts

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Frontend / Admin Team |
| **Source** | Feature Spec F-01.4 |

**Rule Statement**
> Admin panel must auto-save all open form drafts to localStorage every 60 seconds to prevent data loss on session expiry.

**Applies To** — All admin panel forms

**Related Rules** — `BR-016`

**Related Features** — F-01.4

---

#### BR-018 | Refresh Token Reuse Detection

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth / Security Team |
| **Source** | Feature Spec F-01.5 |

**Rule Statement**
> If a refresh token is used after already being rotated (reuse detected), the entire refresh token family for that user is revoked immediately across all devices. Security event written to audit log.

**Applies To** — All authenticated user types

**Related Rules** — `BR-007`, `BR-013`

**Related Features** — F-01.5

---

#### BR-019 | Email Change Re-Verification

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Auth Team |
| **Source** | Feature Spec F-01.6 |

**Rule Statement**
> Changing a customer's email requires re-verification of the new email before it replaces the old one. Old email remains active until new one is verified.

**Applies To** — Customer and trade account email change

**Related Rules** — `BR-001`, `BR-003`

**Related Features** — F-01.6

---

#### BR-020 | Max Saved Addresses Per Customer

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend Team |
| **Source** | Feature Spec F-01.6 |

**Rule Statement**
> Maximum 10 saved delivery addresses per customer. Exactly one address flagged as `isDefault=true` at a time; setting a new default clears the previous flag atomically.

**Applies To** — Customer and trade account address management

**Related Features** — F-01.6, F-04.1

---

### 🏷️ Domain: Product & Catalogue

---

#### BR-021 | Collection Hierarchy Max 3 Levels

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Content Team |
| **Source** | Feature Spec F-02.1 |

**Rule Statement**
> Collection hierarchy is capped at 3 levels (Room/Theme → Subcategory → Designer Collection). Products can belong to multiple collections simultaneously.

**Applies To** — Admin product catalogue management, storefront navigation

**Related Features** — F-02.1, F-02.5

---

#### BR-022 | Empty Collections Hidden from Navigation

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Content / Frontend Team |
| **Source** | Feature Spec F-02.1 |

**Rule Statement**
> Collections with zero active products are automatically hidden from all navigation menus and listing pages — no admin action required.

**Applies To** — All collection listing pages and nav menus

**Related Rules** — `BR-023`

**Related Features** — F-02.1, F-02.5

---

#### BR-023 | Archived Products Excluded Everywhere

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Content / Backend Team |
| **Source** | Feature Spec F-02.1, F-02.2 |

**Rule Statement**
> Products with `is_archived=true` are excluded from all listing pages, collection pages, and the search index.

**Applies To** — Storefront product listing, search, and catalogue APIs

**Related Rules** — `BR-022`, `BR-027`

**Related Features** — F-02.1, F-02.2, F-02.3, F-02.5

---

#### BR-024 | Currency & VAT Display Rules

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance / Frontend Team |
| **Source** | Feature Spec F-02.1, F-04.4 |

**Rule Statement**
> EU prices are displayed VAT-inclusive. US prices show base price with tax added at checkout. Trade accounts see trade pricing after login.

**Formula / Logic**
```
EU: displayed_price = base_price_incl_VAT
US: displayed_price = base_price (+ tax at checkout)
Trade: displayed_price = trade_price (server-resolved, see BR-073)
```

**Applies To** — All product listing and detail pages, cart, checkout

**Related Rules** — `BR-042`, `BR-043`, `BR-044`, `BR-071`

**Related Features** — F-02.1, F-02.3, F-04.4

---

#### BR-025 | Spec PDF Presigned URL & Access Tiers

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Trade Team |
| **Source** | Feature Spec F-02.3 (TC-09) |

**Rule Statement**
> Specification PDFs are served via S3 presigned URLs with 15-minute TTL. Full-resolution version served to Verified Trade Accounts only; standard resolution to all others.

**Applies To** — Product Detail Page PDF download

**Related Rules** — `BR-071`

**Related Features** — F-02.3, F-10.2

---

#### BR-026 | Lead Time Per Configuration

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / Backend Team |
| **Source** | Feature Spec F-02.3 |

**Rule Statement**
> Lead time is displayed and stored per selected configuration (material + finish + size), not per product. The same sofa may show different lead times for different fabric options.

**Applies To** — PDP, cart line items, checkout, order confirmation

**Related Rules** — `BR-036`

**Related Features** — F-02.3, F-03.1, F-04.1, F-05.1

---

#### BR-027 | Search Index Freshness

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Content Team |
| **Source** | Feature Spec F-02.2 |

**Rule Statement**
> Search index must be updated within 60 seconds of any product create, update, or archive event. Archived products must be removed from the index entirely.

**Applies To** — Search engine (Elasticsearch / Algolia)

**Related Rules** — `BR-023`

**Related Features** — F-02.2, F-02.5

---

#### BR-028 | White-Glove Delivery Eligibility

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops Team |
| **Source** | Feature Spec F-02.4, F-04.1, F-09.1 |

**Rule Statement**
> White-glove delivery is offered only when BOTH conditions are met: (a) at least one line item has `requires_whiteglove=true` (weight exceeds admin-configured threshold) AND (b) destination ZIP/postal code is in an active white-glove service zone.

**Formula / Logic**
```
white_glove_eligible = (any line item requires_whiteglove=true)
                    AND (destination_zip IN white_glove_coverage)
If (a) true but (b) false → offer "threshold delivery" or "Contact us"
```

**Applies To** — Delivery availability checker, checkout delivery step, shipping rate calculation

**Related Rules** — `BR-045`

**Related Features** — F-02.4, F-04.1, F-04.4, F-09.1

---

### 🛒 Domain: Cart & Inventory

---

#### BR-029 | Guest Cart TTL

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend Team |
| **Source** | Feature Spec F-03.1 (TC-12) |

**Rule Statement**
> Guest cart stored in Redis with 30-day TTL from last activity. Each cart update refreshes the TTL.

**Applies To** — Unauthenticated cart sessions

**Related Features** — F-03.1, F-03.2

---

#### BR-030 | Cart Quantity Limit

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend Team |
| **Source** | Feature Spec F-03.1 |

**Rule Statement**
> Maximum 10 units per configuration per cart.

**Applies To** — Cart (guest and authenticated)

**Related Features** — F-03.1

---

#### BR-031 | Inventory Not Decremented on Add-to-Cart

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Ops Team |
| **Source** | Feature Spec F-03.1 |

**Rule Statement**
> Inventory is NOT decremented or soft-reserved when a customer adds to cart. Inventory is only soft-held when checkout begins (Redis SETNX hold, 15-minute TTL per BR-034).

**Applies To** — Cart add/update operations

**Related Rules** — `BR-034`

**Related Features** — F-03.1, F-04.1

---

#### BR-032 | Trade vs. Consumer Wishlist Limits

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Trade Team |
| **Source** | Feature Spec F-03.3 |

**Rule Statement**
> Consumer accounts have 1 default wishlist. Verified Trade Accounts can have up to 20 named project wishlists. No guest wishlist.

**Applies To** — Wishlist creation and management

**Related Rules** — `BR-071`

**Related Features** — F-03.3, F-10.1

---

#### BR-033 | Back-in-Stock Notification SLA

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Ops Team |
| **Source** | Feature Spec F-03.3 (TC-28) |

**Rule Statement**
> Back-in-stock email must be sent within 1 hour of a restock event, triggered by a BullMQ hourly job.

**Applies To** — Wishlist items with out-of-stock products

**Related Rules** — `BR-059`

**Related Features** — F-03.3, F-07.3

---

### 💳 Domain: Checkout & Payment

---

#### BR-034 | Inventory Hold on Checkout Start

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Ops Team |
| **Source** | Feature Spec F-04.1 (TC-13) |

**Rule Statement**
> When checkout begins, inventory is soft-held via Redis SETNX (atomic). Hold TTL: 15 minutes. Other customers checking out the same configuration will see reduced available quantity.

**Formula / Logic**
```
Redis key: inventory_hold:{sku_id}:{checkout_session_id}
TTL: 15 minutes
Operation: SETNX (atomic — prevents race conditions)
```

**Applies To** — Checkout flow initiation

**Related Rules** — `BR-031`, `BR-035`

**Related Features** — F-04.1

---

#### BR-035 | Checkout Session Max Duration

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend Team |
| **Source** | Feature Spec F-04.1 (TC-14) |

**Rule Statement**
> Checkout session maximum duration is 30 minutes, enforced server-side. Prevents stale prices, lead times, and tax rates on long-idle checkouts.

**Applies To** — Active checkout sessions

**Related Rules** — `BR-034`, `BR-036`

**Related Features** — F-04.1, F-04.2, F-04.4

---

#### BR-036 | Lead Time Locked at Checkout

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / Backend Team |
| **Source** | Feature Spec F-04.1 |

**Rule Statement**
> Lead time per configured item is locked at checkout session creation and displayed in the delivery step and order summary. Cannot change during the session.

**Applies To** — Checkout session, order confirmation

**Related Rules** — `BR-026`, `BR-035`

**Related Features** — F-04.1, F-05.1

---

#### BR-037 | Order Created on Webhook Only

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Finance Team |
| **Source** | Feature Spec F-04.2 |

**Rule Statement**
> An order record is created ONLY when the `payment_intent.succeeded` webhook is received from Stripe — not on frontend payment confirmation alone.

**Applies To** — All order creation flows

**Related Rules** — `BR-054`

**Related Features** — F-04.2, F-05.1

---

#### BR-038 | No Payment Data Stored

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance / Security Team |
| **Source** | Feature Spec F-04.2 |

**Rule Statement**
> HomeStyle stores no payment card data. PCI DSS compliance is delegated entirely to Stripe. Stripe API timeout is 30 seconds — errors shown to user, no auto-retry.

**Applies To** — Payment processing

**Related Features** — F-04.2, F-06.3

---

#### BR-039 | Discount Code Double Validation

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend Team |
| **Source** | Feature Spec F-04.3 |

**Rule Statement**
> Discount codes are validated twice: (1) on entry for UX feedback; (2) authoritatively at PaymentIntent creation server-side. The second validation is the binding check.

**Applies To** — Checkout discount code field and PaymentIntent creation

**Related Rules** — `BR-040`, `BR-041`

**Related Features** — F-04.2, F-04.3, F-09.5

---

#### BR-040 | One Discount Code Per Order

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance Team |
| **Source** | Feature Spec F-04.3 (OD-04) |

**Rule Statement**
> Default: one discount code per order. Stacking rules are pending client decision (OD-04). Single-use codes are consumed only when `payment_intent.succeeded` webhook is received.

**Applies To** — Checkout discount code application

**Notes / Open Questions**
- [ ] OD-04: Client decision on discount code stacking rules still pending

**Related Rules** — `BR-039`, `BR-041`

**Related Features** — F-04.3, F-09.5

---

#### BR-041 | Trade-Only Code Enforcement

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Trade Team |
| **Source** | Feature Spec F-04.3, F-09.5 |

**Rule Statement**
> Trade-only discount codes are validated against the account's `is_trade_verified` flag. Consumer and guest accounts cannot apply trade codes. Trade codes are not returned by customer-facing endpoints.

**Applies To** — Discount code validation, code listing APIs

**Related Rules** — `BR-039`, `BR-071`

**Related Features** — F-04.3, F-09.5

---

#### BR-042 | Tax Calculation Authoritative Timing

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance / Backend Team |
| **Source** | Feature Spec F-04.4 |

**Rule Statement**
> Tax calculation is authoritative at PaymentIntent creation (Stripe Tax / TaxJar). The pre-confirmation tax shown to the customer is an estimate only.

**Applies To** — Checkout tax line items

**Related Rules** — `BR-043`, `BR-044`

**Related Features** — F-04.2, F-04.4

---

#### BR-043 | EU VAT Included; US Tax Added at Checkout

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance Team |
| **Source** | Feature Spec F-04.4 |

**Rule Statement**
> EU prices include VAT — displayed as "Includes X% VAT: €[amount]" at checkout. US prices show tax as an additional line item at checkout. Nexus states determined per CR-10.

**Formula / Logic**
```
EU: total = displayed_price (VAT already included)
US: total = subtotal + sales_tax_by_state + shipping
```

**Applies To** — All checkout tax display and calculation

**Related Rules** — `BR-024`, `BR-042`, `BR-044`

**Related Features** — F-04.4

---

#### BR-044 | Trade VAT Exemption & Reverse Charge

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance / Trade Team |
| **Source** | Feature Spec F-04.4 (OD-06) |

**Rule Statement**
> EU Trade Accounts with verified VAT registration receive VAT-exempt checkout. Invoice generated with customer VAT number and 'Reverse Charge' notation. Trade price discount and VAT exemption are separate — trade price is always net-of-list-discount; VAT exemption applied on top at checkout.

**Applies To** — EU Verified Trade Account checkout

**Notes / Open Questions**
- [ ] OD-06: EU VAT registration verification process to be confirmed with Legal

**Related Rules** — `BR-043`, `BR-071`, `BR-072`, `BR-073`

**Related Features** — F-04.4, F-10.1, F-10.2

---

#### BR-045 | Shipping Rate Formula

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops Team |
| **Source** | Feature Spec F-04.4, F-09.1 |

**Rule Statement**
> Shipping rate is calculated as: `flat_rate + (total_weight_kg × per_kg_rate)` from the `shipping_rates` table by zone, plus a white-glove surcharge (added once if any line item qualifies). Rate changes are non-retroactive.

**Formula / Logic**
```
shipping_cost = flat_rate + (sum(line_item_weight_kg) × per_kg_rate)
              + white_glove_surcharge (if applicable, once per order)
Free shipping threshold: per zone+method (admin-configurable)
```

**Applies To** — Checkout shipping calculation

**Related Rules** — `BR-028`, `BR-066`

**Related Features** — F-04.4, F-09.1

---

### 📦 Domain: Order & Fulfillment

---

#### BR-046 | Order Reference Format

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / Backend Team |
| **Source** | Feature Spec F-05.1 |

**Rule Statement**
> Order references follow the format `HS-[YEAR]-[6-digit sequential ID]` (e.g. `HS-2026-004821`).

**Applies To** — All order records, customer-facing and admin

**Related Features** — F-05.1

---

#### BR-047 | Confirmation Email Dispatch SLA

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / Backend Team |
| **Source** | Feature Spec F-05.1 (TC-17, TC-29) |

**Rule Statement**
> Order confirmation email is dispatched as an immediate BullMQ job. Retry policy: 3× at 1-minute intervals, then 1 final attempt at 15 minutes. Failures after 4 attempts go to dead-letter queue with admin alert.

**Applies To** — Post-order confirmation email

**Related Rules** — `BR-059`

**Related Features** — F-05.1, F-07.3

---

#### BR-048 | Dispatch Email SLA

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops Team |
| **Source** | Feature Spec F-05.2 (TC-18) |

**Rule Statement**
> Dispatch notification email must be sent within 48 hours of order confirmation.

**Applies To** — Order dispatch flow

**Related Rules** — `BR-059`

**Related Features** — F-05.2, F-07.3

---

### 🔄 Domain: Returns & Refunds

---

#### BR-049 | Return Window Default & Configurability

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / CS Team |
| **Source** | Feature Spec F-06.1 (TC-19) |

**Rule Statement**
> Return window is admin-configurable with a default of 30 days from delivery date. Eligibility is validated server-side at both UI render and form submission.

**Applies To** — Return request submission

**Related Rules** — `BR-050`

**Related Features** — F-06.1

---

#### BR-050 | Made-to-Order Return Restriction

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / CS Team |
| **Source** | Feature Spec F-06.1 (OD-08) |

**Rule Statement**
> Made-to-order and custom configuration items are eligible for exchange or store credit only — not cash refund — unless the item is faulty or does not match the specification.

**Applies To** — Return requests for MTO/custom items

**Notes / Open Questions**
- [ ] OD-08: Client to confirm final MTO return policy before launch

**Related Rules** — `BR-049`, `BR-052`

**Related Features** — F-06.1, F-06.2

---

#### BR-051 | Return Review SLA (Admin)

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / CS Team |
| **Source** | Feature Spec F-06.1, F-06.2 (TC-21) |

**Rule Statement**
> Admin must review a return request within 2 business days. SLA breach flags the item in the admin panel and alerts the Super Admin.

**Applies To** — Admin return review queue

**Related Rules** — `BR-052`, `BR-053`

**Related Features** — F-06.1, F-06.2

---

#### BR-052 | Refund Processing SLA

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance / Ops Team |
| **Source** | Feature Spec F-06.3 (TC-22) |

**Rule Statement**
> Refund must be processed within 1 business day of admin marking the return item as 'Received'. Partial refunds (subset of returned items) are permitted.

**Applies To** — Refund initiation step

**Related Rules** — `BR-051`, `BR-053`, `BR-054`

**Related Features** — F-06.2, F-06.3

---

#### BR-053 | Total Returns Resolution Target

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / Finance Team |
| **Source** | Feature Spec F-06.3 (TC-23) |

**Rule Statement**
> Total end-to-end refund resolution must be completed within ≤12 business days (2 days review + 1 day initiation + up to 10 days Stripe processing).

**Formula / Logic**
```
12 business days = 2 (admin review SLA)
                 + 1 (refund initiation SLA)
                 + up to 10 (Stripe bank processing)
```

**Applies To** — Full returns-to-refund lifecycle

**Related Rules** — `BR-051`, `BR-052`

**Related Features** — F-06.2, F-06.3

---

#### BR-054 | Refund via Original PaymentIntent

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance / Backend Team |
| **Source** | Feature Spec F-06.3 |

**Rule Statement**
> All refunds are initiated via Stripe Refunds API using the original `PaymentIntent ID`. Order status is set to 'refunded' only on `refund.succeeded` webhook. If webhook not received within 24 hours, an admin alert is triggered.

**Applies To** — Refund processing

**Related Rules** — `BR-037`, `BR-038`

**Related Features** — F-04.2, F-06.3

---

### ⭐ Domain: Reviews

---

#### BR-055 | Verified Purchase Required for Review

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Content Team |
| **Source** | Feature Spec F-07.1 (TC-24) |

**Rule Statement**
> Only customers who have a delivered order containing the specific product may submit a review. Purchase verification is performed server-side.

**Applies To** — Review submission endpoint

**Related Rules** — `BR-056`, `BR-058`

**Related Features** — F-07.1

---

#### BR-056 | One Review Per Customer Per Product

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Content Team |
| **Source** | Feature Spec F-07.1 |

**Rule Statement**
> One review per customer per product. A duplicate submission returns 422 with a link to edit the existing review.

**Applies To** — Review submission

**Related Rules** — `BR-055`

**Related Features** — F-07.1

---

#### BR-057 | Reviews Require Admin Approval

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Content Team |
| **Source** | Feature Spec F-07.1, F-07.2 |

**Rule Statement**
> Reviews must be approved by admin moderation (F-07.2) before appearing on any product page.

**Applies To** — Review display on PDP

**Related Features** — F-07.1, F-07.2

---

#### BR-058 | Review Submission Window

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Content Team |
| **Source** | Feature Spec F-07.1 (TC-25) |

**Rule Statement**
> The 'Write a Review' option is available for 180 days from delivery date. Submissions outside this window are rejected.

**Applies To** — Review submission UI and API

**Related Rules** — `BR-055`

**Related Features** — F-07.1

---

### 📧 Domain: Notifications

---

#### BR-059 | Email Retry Policy

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Ops Team |
| **Source** | Feature Spec F-07.3 (TC-29) |

**Rule Statement**
> All transactional emails use BullMQ with retry: 3× at 1-minute intervals, then 1 final attempt at 15 minutes. After 4 failures → dead-letter queue → admin alerted.

**Formula / Logic**
```
Attempt 1: immediate
Attempts 2–4: retry at 1 min each
Attempt 5: retry at 15 min
After 5 failures: dead-letter queue + admin alert
```

**Applies To** — All transactional emails (order confirm, dispatch, refund, back-in-stock, etc.)

**Related Rules** — `BR-047`, `BR-048`, `BR-033`

**Related Features** — F-07.3, F-05.1, F-05.2, F-06.3

---

### 🔒 Domain: Compliance

---

#### BR-060 | No Non-Essential Cookies Before Consent

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal / Frontend Team |
| **Source** | Feature Spec F-08.1 (TC-36) |

**Rule Statement**
> No analytics or marketing cookies are set before the user records cookie consent. Consent stored with timestamp and policy version in a first-party cookie and, if authenticated, synced to the user record.

**Applies To** — All storefront pages, EU and US

**Related Rules** — `BR-061`

**Related Features** — F-08.1

---

#### BR-061 | CCPA Do Not Sell Link (California)

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal / Frontend Team |
| **Source** | Feature Spec F-08.1 (TC-37) |

**Rule Statement**
> "Do Not Sell My Personal Information" link must be shown to users identified as being in California. Opt-out must take effect immediately (same page load).

**Applies To** — California-detected users (GeoIP)

**Related Rules** — `BR-060`

**Related Features** — F-08.1

---

#### BR-062 | GDPR Erasure 30-Day Deadline

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal / Backend Team |
| **Source** | Feature Spec F-08.2 (TC-34) |

**Rule Statement**
> Data erasure (Right to be Forgotten) must be completed within 30 days of a verified erasure request. Erasure covers: name, email, phone, addresses, wishlists, reviews (anonymised), session tokens.

**Applies To** — GDPR erasure requests

**Related Rules** — `BR-063`, `BR-068`

**Related Features** — F-08.2

---

#### BR-063 | Financial Records 7-Year Retention

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal / Finance Team |
| **Source** | Feature Spec F-08.2 (TC-32) |

**Rule Statement**
> Financial records (orders, invoices, tax records) are retained for 7 years per legal requirement. During erasure, PII fields are anonymised but financial totals and line items are preserved.

**Formula / Logic**
```
Erasure + financial record: PII fields → anonymised
Financial totals, config details, line items → retained
Retention period: 7 years from order date
```

**Applies To** — GDPR erasure, data retention jobs

**Related Rules** — `BR-062`, `BR-006`

**Related Features** — F-08.2, F-08.3

---

#### BR-064 | Server Log Purge 90 Days

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Infra Team |
| **Source** | Feature Spec F-08.3 (TC-33) |

**Rule Statement**
> Server and application logs older than 90 days are auto-purged by the nightly retention job.

**Related Features** — F-08.3

---

#### BR-065 | Audit Log Archive 3 Years

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Backend / Security Team |
| **Source** | Feature Spec F-08.3 (TC-35) |

**Rule Statement**
> Audit logs older than 3 years are archived to cold storage (S3 Glacier).

**Related Features** — F-08.3

---

### ⚙️ Domain: Admin & Config

---

#### BR-066 | Shipping Rate Non-Retroactive

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops Team |
| **Source** | Feature Spec F-09.1 |

**Rule Statement**
> Shipping rate changes are non-retroactive. Locked checkout sessions (TC-14) and confirmed orders always use the rate captured at the time the session was locked.

**Applies To** — Admin shipping rate updates

**Related Rules** — `BR-045`, `BR-035`

**Related Features** — F-09.1, F-04.4

---

#### BR-067 | Soft-Delete Only for Shipping Config

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Ops / Backend Team |
| **Source** | Feature Spec F-09.1 |

**Rule Statement**
> Zones, rates, and delivery partners are soft-deleted only. Hard-delete is forbidden to preserve historical order traceability.

**Applies To** — Admin shipping configuration

**Related Features** — F-09.1

---

#### BR-068 | Customer Hard-Delete Forbidden

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Legal / Ops Team |
| **Source** | Feature Spec F-09.2 |

**Rule Statement**
> Customer records are never hard-deleted from the admin customer management interface. All erasure must go through the GDPR erasure flow (F-08.2).

**Applies To** — Admin customer management panel

**Related Rules** — `BR-062`

**Related Features** — F-09.2, F-08.2

---

#### BR-069 | Discount Code Immutable After Redemption

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance Team |
| **Source** | Feature Spec F-09.5 |

**Rule Statement**
> Discount code type and value are immutable once any redemption exists. Admin must retire the code and create a new one to make changes. Retired status is terminal — no reactivation.

**Formula / Logic**
```
Status flow: draft → scheduled → active → expired (auto)
                                        → retired (manual, any state)
Retired is terminal — cannot be reactivated
```

**Applies To** — Admin discount code management

**Related Rules** — `BR-039`, `BR-040`

**Related Features** — F-09.5, F-04.3

---

#### BR-070 | Discount Minimum Cart Value Basis

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Finance Team |
| **Source** | Feature Spec F-09.5 |

**Rule Statement**
> Minimum cart value for discount code eligibility is evaluated against the subtotal after trade discount (if applicable) and before tax and shipping.

**Formula / Logic**
```
eligibility_basis = subtotal − trade_discount
                  (before tax, before shipping)
```

**Applies To** — Discount code validation

**Related Rules** — `BR-039`, `BR-044`

**Related Features** — F-04.3, F-09.5

---

### 🤝 Domain: Trade Portal

---

#### BR-071 | Trade Pricing Server-Side Only

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Trade / Backend Team |
| **Source** | Feature Spec F-10.2 |

**Rule Statement**
> Trade pricing is resolved server-side on every read. It is never computed client-side and never included in API responses sent to non-trade roles.

**Applies To** — All product, cart, and checkout pricing endpoints

**Related Rules** — `BR-072`, `BR-073`

**Related Features** — F-10.2, F-02.3, F-03.1, F-04.1

---

#### BR-072 | Trade Price Resolution Order

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Trade Team |
| **Source** | Feature Spec F-10.2 |

**Rule Statement**
> Trade price is resolved using the following precedence (highest first): (1) per-customer per-product override, (2) per-customer per-category override, (3) tier per-category override, (4) tier baseline.

**Formula / Logic**
```
Priority: customer+product > customer+category > tier+category > tier baseline
```

**Applies To** — Trade price resolution for all product reads

**Related Rules** — `BR-071`, `BR-073`

**Related Features** — F-10.2

---

#### BR-073 | Trade Price Formula

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Trade / Finance Team |
| **Source** | Feature Spec F-10.2 |

**Rule Statement**
> Final trade price = `list_price × (1 − resolved_discount_percent)`, rounded per currency rules. Pricing changes are non-retroactive. EU VAT exemption (BR-044) is applied at checkout — not to the trade price line.

**Formula / Logic**
```
trade_price = list_price × (1 − resolved_percent)
            → rounded per currency rule
VAT exemption: applied separately at F-04.4 (not here)
Non-retroactive: locked checkout sessions and confirmed orders use price at lock time
```

**Applies To** — All trade price calculations

**Related Rules** — `BR-071`, `BR-072`, `BR-044`

**Related Features** — F-10.2, F-04.4

---

#### BR-074 | Trade Application Review SLA

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Trade Team |
| **Source** | Feature Spec F-10.1 |

**Rule Statement**
> Trade account applications must reach a terminal decision (approved or rejected) within 5 business days of submission. Breach surfaces on admin dashboard and daily digest alert.

**Applies To** — Trade account application queue

**Related Rules** — `BR-075`

**Related Features** — F-10.1, F-09.4

---

#### BR-075 | Trade Approval Triggers

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Trade Team |
| **Source** | Feature Spec F-10.1 |

**Rule Statement**
> Approving a trade account triggers (in order): role provisioning to Verified Trade, VAT-exempt flag (if EU VAT validated), assignment to default pricing tier, Account Manager assignment, and welcome email.

**Applies To** — Trade account approval workflow

**Related Rules** — `BR-044`, `BR-072`, `BR-076`

**Related Features** — F-10.1, F-10.2, F-10.3, F-07.3

---

#### BR-076 | Every Active Trade Account Needs an AM

| Field | Detail |
|---|---|
| **Status** | `ACTIVE` |
| **Version** | 1.0 |
| **Created** | 2026-06-02 |
| **Last Modified** | 2026-06-02 |
| **Owner** | Trade Team |
| **Source** | Feature Spec F-10.3 |

**Rule Statement**
> Every Verified Trade Account in `active` or `suspended` state must have an assigned Account Manager. Departed AMs are non-deletable — records retained for audit. Bulk assignment hard-blocks at the AM's `max_book_size` capacity.

**Applies To** — Trade account management

**Related Rules** — `BR-074`, `BR-075`

**Related Features** — F-10.1, F-10.3

---

---

### 🚚 Domain: Logistics & Carrier

*Rules governing carrier eligibility validation, auto-assignment, and EU Green Delivery routing. Implemented at the F-05.6 Pack-Verify step and configured via F-09.1 Admin Shipping Configuration.*

---

#### BR-077 | Carrier Weight/Dimension Eligibility Check

| Field | Value |
|---|---|
| **Rule ID** | BR-077 |
| **Name** | Carrier Weight/Dimension Eligibility Check |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-09.1 (config), F-05.6 (enforcement) |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Before generating a shipping label, the system must validate that the selected carrier's `max_weight_kg`, `max_length_cm`, and `max_girth_cm` constraints are not exceeded by the packed consignment. Any carrier that fails this check is excluded from the eligible carrier list.

**Rationale**

Prevents carrier refusals at collection (e.g. DPD EU refusing a 120 kg sofa assigned by a warehouse operator who did not check weight limits), which cause 2+ day delivery delays and negative customer reviews.

**Enforcement Point**

At pack-verify step in F-05.6, before label API call.

**Related Rules** — `BR-078`, `BR-079`, `BR-080`, `BR-081`

---

#### BR-078 | DPD EU Parcel Weight Ceiling

| Field | Value |
|---|---|
| **Rule ID** | BR-078 |
| **Name** | DPD EU Parcel Weight Ceiling |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-09.1, F-05.6 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

DPD EU parcel service (`carrier_code: DPD-EU`) is constrained to consignments of ≤ 31.5 kg. Any consignment exceeding this limit must not be assigned to DPD-EU, regardless of zone eligibility.

**Rationale**

DPD EU's contractual and physical limit is 31.5 kg per consignment. Assignment of heavier items causes guaranteed refusal at collection.

**Related Rules** — `BR-077`, `BR-079`

---

#### BR-079 | LTL/White-Glove Routing for Heavy Furniture

| Field | Value |
|---|---|
| **Rule ID** | BR-079 |
| **Name** | LTL/White-Glove Routing for Heavy Furniture |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-09.1, F-05.6 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Furniture SKUs with `requires_ltl = true` OR packed weight > 31.5 kg must be routed exclusively to carriers with `service_type = ltl` or `service_type = white_glove`. Assignment to a `service_type = parcel` carrier is blocked by the eligibility engine.

**Related Rules** — `BR-077`, `BR-078`, `BR-080`

---

#### BR-080 | Carrier Auto-Assignment with Manual Override

| Field | Value |
|---|---|
| **Rule ID** | BR-080 |
| **Name** | Carrier Auto-Assignment with Manual Override |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-05.6, F-09.1 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

The system auto-selects the highest-ranked eligible carrier and presents it to the warehouse operator for confirmation. The operator may override the auto-selected carrier, but must provide a mandatory reason code. All overrides — including the auto-selected carrier, the chosen carrier, the operator ID, and the reason — are audit-logged.

**Override Safeguard**

If the operator selects an ineligible carrier (fails BR-077 constraints), the system presents a warning. The operator may still confirm with a reason code, but the override is flagged for monthly audit review.

**Related Rules** — `BR-077`, `BR-081`, `BR-082`

---

#### BR-081 | CARRIER_UNRESOLVED Blocks Label Generation

| Field | Value |
|---|---|
| **Rule ID** | BR-081 |
| **Name** | CARRIER_UNRESOLVED Blocks Label Generation |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-05.6, F-07.4 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

If no eligible carrier is found after running the eligibility engine, the shipment enters `CARRIER_UNRESOLVED` status. Label generation is blocked. An F-07.4 admin alert is raised within 60 seconds. An Ops Admin must resolve the status manually before dispatch can proceed. TC-17/TC-18 SLA clocks continue running.

**Related Rules** — `BR-077`, `BR-080`

---

#### BR-082 | Green Delivery Zone Preferential Routing

| Field | Value |
|---|---|
| **Rule ID** | BR-082 |
| **Name** | Green Delivery Zone Preferential Routing |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-09.1, F-05.6 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

For shipments destined for a configured `green_delivery_zone` (e.g. Berlin, Paris, Amsterdam urban postal prefixes), the carrier eligibility engine must preferentially rank carriers with `green_certified = true` first, **when** the rolling 30-day green-delivery ratio for that zone is below `green_kpi_target_pct` (default 30%). When the ratio is at or above target, standard cost-optimised ranking applies.

**Rationale**

EU urban tax rebate policy requires ≥ 30% of intra-urban deliveries to use certified green carriers (EV fleet or carbon-neutral certified). Non-compliance forfeits EU tax rebate revenue.

**Related Rules** — `BR-083`, `BR-080`

---

#### BR-083 | Green Delivery KPI Target & Monitoring

| Field | Value |
|---|---|
| **Rule ID** | BR-083 |
| **Name** | Green Delivery KPI Target & Monitoring |
| **Domain** | Logistics & Carrier |
| **Status** | ACTIVE |
| **Feature** | F-09.1, F-09.4 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

- Green delivery ratio is computed per `green_delivery_zone` on a rolling 30-day basis, recalculated nightly.
- Target: ≥ 30% of shipments into each zone must use `green_certified = true` carriers.
- Zones below target for two consecutive months trigger a Logistics Director alert.
- `green_cert_reference` is mandatory for all carriers with `green_certified = true`; missing reference blocks carrier activation.
- Monthly CSV export of green shipments is generated for EU tax rebate submission.

**Related Rules** — `BR-082`

---

### 📊 Domain: Inventory Intelligence

*Rules governing dynamic safety stock configuration, risk-based alert prioritisation, and governance of threshold changes. Replaces the deprecated hardcoded ATP ≤ 5 universal threshold. Implemented in F-09.3 Admin Inventory Management.*

---

#### BR-084 | Dynamic Safety Stock Configuration Scope

| Field | Value |
|---|---|
| **Rule ID** | BR-084 |
| **Name** | Dynamic Safety Stock Configuration Scope |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Safety stock is configured per scope in `inventory_threshold_config`. Resolution priority (highest wins): SKU + Warehouse > SKU (all warehouses) > Category + Warehouse > Category (all warehouses) > System Default. System defaults: parcel accessory = 2; white-glove furniture = 5; made-to-order = 0.

**Deprecates**

The previous hardcoded universal threshold of ATP ≤ 5 for all SKUs is deprecated and must not be used.

**Related Rules** — `BR-085`, `BR-088`, `BR-089`, `BR-090`

---

#### BR-085 | Risk Level Derivation Formula

| Field | Value |
|---|---|
| **Rule ID** | BR-085 |
| **Name** | Risk Level Derivation Formula |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Alert risk level is derived dynamically, not from a static threshold:

```
days_of_stock = ATP / sales_velocity (units/day)
lead_time     = category_avg_lead_time_days (or SKU-level override)

CRITICAL  : days_of_stock < lead_time
MEDIUM    : days_of_stock < (lead_time × 1.5)
LOW       : days_of_stock ≥ (lead_time × 1.5)  [silent — no alert]
```

| Risk Level | Alert Behaviour |
|---|---|
| Critical | Real-time alert + dashboard Critical badge |
| Medium | Dashboard amber warning |
| Low | Silent monitoring only |

**Examples**

- Sofa: ATP=4, velocity=3/day → days=1.3, lead time=28 → **Critical**
- Lamp: ATP=4, velocity=0.1/day → days=40, lead time=2 → **Low**

**Related Rules** — `BR-084`, `BR-086`, `BR-087`

---

#### BR-086 | Sales Velocity Computation

| Field | Value |
|---|---|
| **Rule ID** | BR-086 |
| **Name** | Sales Velocity Computation |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Sales velocity per SKU is the 30-day rolling average of `quantity_sold_per_day`, computed by a nightly background job from `sales_velocity_daily`. Admin may set a `sales_velocity_override` at SKU level to handle new products with no history or manual corrections. New products with no history default to category average velocity.

**Related Rules** — `BR-085`, `BR-087`

---

#### BR-087 | Lead Time Computation

| Field | Value |
|---|---|
| **Rule ID** | BR-087 |
| **Name** | Lead Time Computation |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Lead time per product category is the monthly-recomputed average of `DATEDIFF(day, po_created_date, received_date)` from `purchase_order_receipts`. Admin may set a `lead_time_days_override` at SKU level. This value feeds the BR-085 risk formula.

**Related Rules** — `BR-085`, `BR-086`

---

#### BR-088 | Threshold Change Governance — Auto-Approval

| Field | Value |
|---|---|
| **Rule ID** | BR-088 |
| **Name** | Threshold Change Governance — Auto-Approval |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Safety stock threshold changes of **less than 10%** relative to the current active config are auto-approved and take effect from `effective_date` with no human approval step. An audit record is still created.

**Related Rules** — `BR-089`, `BR-090`, `BR-091`

---

#### BR-089 | Threshold Change Governance — Manager Approval

| Field | Value |
|---|---|
| **Rule ID** | BR-089 |
| **Name** | Threshold Change Governance — Manager Approval |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Safety stock threshold changes of **10%–30%** require Inventory Manager approval before taking effect. A pending approval request is created and surfaced in the approver's dashboard. Requests expire after 5 business days (auto-rejected; requester and approver notified).

**Related Rules** — `BR-088`, `BR-090`, `BR-091`

---

#### BR-090 | Threshold Change Governance — Finance Approval

| Field | Value |
|---|---|
| **Rule ID** | BR-090 |
| **Name** | Threshold Change Governance — Finance Approval |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Safety stock threshold changes of **more than 30%** require Finance Manager approval. Inventory Manager approval alone is insufficient for changes in this band. Rationale: large threshold increases materially affect working capital and safety stock cost.

**Related Rules** — `BR-088`, `BR-089`, `BR-091`

---

#### BR-091 | Threshold Change Audit & Versioning

| Field | Value |
|---|---|
| **Rule ID** | BR-091 |
| **Name** | Threshold Change Audit & Versioning |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Every threshold config change — auto-approved or manually approved — produces an immutable audit record containing: `old_value`, `new_value`, `changed_by`, `approved_by` (null if auto-approved), `change_reason`, `timestamp`, and `version`. Config records are versioned; no record is deleted.

**Related Rules** — `BR-088`, `BR-089`, `BR-090`, `BR-093`

---

#### BR-092 | Seasonal Config Expiration & Reversion

| Field | Value |
|---|---|
| **Rule ID** | BR-092 |
| **Name** | Seasonal Config Expiration & Reversion |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Threshold configs may include an `expiration_date`. At expiration, the system automatically reverts to the next-priority active config in the resolution chain (BR-084). No manual action is required. Admins are notified 3 days before expiration.

**Use Case**

Christmas collection: temporary safety stock increase effective Dec 1 → expires Jan 7. System auto-reverts to standard category config on Jan 7.

**Related Rules** — `BR-084`, `BR-091`

---

#### BR-093 | Historical Reports Use Active Config at Event Time

| Field | Value |
|---|---|
| **Rule ID** | BR-093 |
| **Name** | Historical Reports Use Active Config at Event Time |
| **Domain** | Inventory Intelligence |
| **Status** | ACTIVE |
| **Feature** | F-09.3 |
| **Last Modified** | 2026-06-05 |

**Rule Statement**

Inventory reports for past periods must use the threshold config version whose `effective_date` ≤ event date < `expiration_date` (or no expiration). Reports must not apply the current active config retroactively, as this would misrepresent historical alert conditions and distort trend analysis.

**Related Rules** — `BR-091`, `BR-092`

---

## Change History

| Date | Rule ID | Change | Changed By | Reason |
|---|---|---|---|---|
| 2026-06-02 | BR-001 to BR-076 | Initial catalog created | BA | Project documentation |
| 2026-06-05 | BR-077 to BR-083 | Added 🚚 Logistics & Carrier domain — carrier eligibility validation and Green Delivery routing rules | BA | EU logistics expansion: Poland warehouse → DE/FR/NL markets; carrier mis-assignment incidents; EU Green Delivery KPI |
| 2026-06-05 | BR-084 to BR-093 | Added 📊 Inventory Intelligence domain — dynamic safety stock, risk-based alerting, governance workflow | BA | Replaced hardcoded ATP ≤ 5 threshold; 20K+ SKU differentiated replenishment; Finance governance concern |
