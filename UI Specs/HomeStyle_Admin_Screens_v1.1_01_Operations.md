# HomeStyle — Admin Panel Screen Descriptions
## File 1 of 5 · Operations role
**v1.1 Add-on · May 2026 · Companion to v1.0 · April 2026**

> **About this file pack:** v1.1 was originally a single addendum. This pack splits the addendum by admin role for easier hand-off:
>
> 1. **`HomeStyle_Admin_Screens_v1.1_01_Operations.md`** *(this file)* — SCR-A-07..A-12
> 2. `HomeStyle_Admin_Screens_v1.1_02_Content.md` — SCR-B-05..B-11
> 3. `HomeStyle_Admin_Screens_v1.1_03_Trade.md` — SCR-TR-02..TR-06
> 4. `HomeStyle_Admin_Screens_v1.1_04_SuperAdmin.md` — SCR-S-06..S-10
> 5. `HomeStyle_Admin_Screens_v1.1_05_Shared_Patterns_and_Traceability.md` — PAT-01..05, traceability, open Qs, assumptions
>
> **Read order:** v1.0 §1 (shell, sidebar, topbar, TC-05 timeout, auto-save banner, global search) → file 5 (shared patterns) → this file. The shell defined in v1.0 §1 applies to every screen below without repetition.

**Source map:**
- TC-XX → `HomeStyle_BRD_v4.md` §17 (Time Constraints)
- OD-XX → `HomeStyle_BRD_v4.md` §9 (Open Decisions)
- E-XX → `HomeStyle_BRD_v4.md` §14 (Email Triggers)
- F-XX.X → `feature_specs/F_XX.X_*.md`

**Role colour key:** Operations = teal · Content = purple · Trade = amber · Super Admin = red

---

## Scope of this file

This file describes the **Admin — Operations** role's add-on screens introduced in v1.1. Operations covers the day-to-day fulfilment side of the business: returns review, refunds, customer service, inventory adjustments, shipping configuration, and operational analytics.

| ID | Screen | Source spec |
|---|---|---|
| SCR-A-07 | Return Detail (full review workflow) | F-06.2 |
| SCR-A-08 | Refund Processing modal | F-06.3 |
| SCR-A-09 | Customer Detail | F-01.6, F-08.2 |
| SCR-A-10 | Inventory / Stock view | F-02.5 |
| SCR-A-11 | Shipping Configuration | F-04.4 |
| SCR-A-12 | Operations Analytics | KPIs §19 |

**Module access (BRD §7 + `domain-rules.md`):** Operations + Super Admin only. Trade has read access to a B2B-filtered slice of SCR-A-12 only. Content has no access to any screen in this file.

**Critical role boundaries to preserve in design:**
- **BR-009:** Ops cannot edit price, name, images, descriptions, or SEO of products. SCR-A-10 enforces this — stock-only adjustments.
- **BR-009:** Ops cannot touch financial config (tax model, FX, free-shipping thresholds). SCR-A-11 covers operational shipping config (zones, rates, white-glove partners, cut-offs); financial-only fields are in SCR-S-08 (Super Admin).
- **GDPR:** Ops sees the customer profile in SCR-A-09 but never the GDPR sub-section (export / erasure / CCPA toggles) — that's Super Admin only.
- **Analytics blame:** SLA per-admin drill-down on SCR-A-12 is Super Admin only. Ops sees aggregate.

---

## 1. Role: Admin — Operations (additions)

### SCR-A-07 · Return Detail

**Route:** `/admin/returns/[returnId]`
**Access:** Ops, Super Admin
**Source:** F-06.2 *Admin Return Review*. Triggers TC-21 (2-business-day review SLA), uses E-08 / E-10 emails.
**Reached from:** SCR-A-05 row click (anywhere outside the inline `[Approve] [Reject]` buttons), or directly from a system alert link.

---

**Page header (full width):**

```
[ ← Back to Returns ]

RT-0092      [TC-21 OVERDUE 4h]      [Under Review badge]
Order HS-2026-004821 · Alexandra Chen · Submitted 24 Apr 2026, 16:02
                                  [Approve return ▾]  [Reject return]
```

The "TC-21 OVERDUE" badge appears only when the SLA is breached (red, solid). The status badge always renders. The two action buttons mirror the inline list actions but stay sticky at the top of the page so the admin never has to scroll back up.

**`[Approve return ▾]`:** dropdown caret → `Cash refund / Exchange / Replacement / Store credit`. Clicking the main button (not the caret) opens the approval drawer with the previously-used refund type pre-selected (default: Cash refund).

**`[Reject return]`:** opens the same reject modal defined in v1.0 SCR-A-05.

---

**Two-column layout:**

**Left column (~60%) — customer claim:**

*Section 1: Returned items*

For each line item being returned:
- Thumbnail 48×48px, product name (12px bold), SKU (11px monospace #888), configuration text (11px #666).
- Qty being returned + unit price.
- Reason chip from the customer's submission. The 6 reason values from the BRD §15 are mapped to chip colours:
  - `Damaged on arrival` → red chip
  - `Wrong item / configuration` → red chip
  - `Item does not match description / images` → amber chip
  - `Item arrived outside confirmed lead time window` → amber chip
  - `Changed my mind` → grey chip
  - `Other` → grey chip + the customer's free-text appears below

**Made-to-order line item:** a small blue "Made to Order" badge appears next to the product name. The reason chip "Damaged on arrival" or "Item does not match description" makes the line eligible for cash refund per OD-08; any other reason on a made-to-order line shows an inline note: *"OD-08: Made-to-order — exchange or store credit only unless faulty. Cash refund requires Super Admin approval."*

*Section 2: Customer message + photos*

A white card. Title "Customer message". Body shows the customer's free-text claim (max 2000 chars, soft limit on submission).

Below that, a photo gallery — each thumbnail 96×96px, max 6 photos per submission. Hover shows a zoom icon. **Click opens a lightbox** (full-screen overlay, dark backdrop, photo centred at viewport-fit-contain). Lightbox controls: arrow keys / `[← Prev] [Next →]` buttons, `Esc` to close, `[Download all]` button (zips all photos). Photos are served via TC-09 15-min S3 presigned URLs — if the lightbox sits open longer than 15 min the next nav click silently re-fetches a fresh URL (no error to the admin).

**No photos uploaded:** the gallery card is replaced by a small grey banner: *"Customer did not attach photos."* This is information, not a warning — photos are optional except when reason = "Damaged on arrival", which the storefront already enforces.

*Section 3: Item condition assessment (admin)*

A radio group, only enabled once status moves to "Item Received" (greyed out before that, with placeholder text "Available after item is received at warehouse"):

```
( ) Unused / original packaging        → Full refund
( ) Minor use / packaging removed     → 85% refund
( ) Damaged by customer               → No refund (exchange only at Ops discretion)
```

These three options are pulled verbatim from `references/domain-rules.md` Returns & Refunds. Selecting a tier auto-fills the refund amount in the approval drawer.

*Section 4: Original order summary (read-only)*

A muted card with: order number (link to SCR-A-04), order date, total paid, payment method, currency. Shown for context only; the admin never edits this here.

---

**Right column (~40%) — workflow:**

*Section 1: Return state machine timeline*

Vertical timeline matching the BRD §15 state machine:

```
●  Return Requested        24 Apr 16:02 · Customer: A. Chen
│  ↳ Reason: Wrong configuration delivered (Walnut received, oak ordered)
│
●  Under Review            24 Apr 16:11 · System auto-transition
│  ↳ TC-21 deadline: 26 Apr 16:11 · Now 4h overdue
│
○  Return Approved / Rejected
│
○  Item in Transit
│
○  Item Received
│
○  Refund Initiated
│
○  Refund Completed
```

Same dot/colour rules as SCR-A-04. The current step is gold + bold + sub-text in red when SLA breached. Future steps grey/empty.

*Section 2: Return approval drawer (slides in from right when `[Approve return]` clicked)*

This drawer is wider than the v1.0 inline accordion (it covers ~480px) because the admin is now inside the detail context.

```
APPROVE RT-0092 — A. Chen

Refund type *
( ) Cash refund to original payment method
( ) Exchange — replacement order
( ) Store credit — issued as discount code
( ) Replacement — send replacement free of charge
   ⚠ Eligibility: BRD §15.1 + condition assessment

Refund amount (USD) *   [$_______]
   Suggested by tier:
   • Unused/original pkg → 100% = $X,XXX.XX
   • Minor use/pkg removed → 85% = $X,XXX.XX
   • Damaged by customer → $0
   Manual override allowed but logged in the audit trail.

Restocking fee
[$ 0.00]   Optional. Default 0. Capped at 15% of line total.

Return method *
( ) Customer ships back via courier (provide returns label)
( ) Schedule white-glove pickup
( ) Customer drops off at warehouse

Customer ship-back deadline (TC-20)
   14 days from approval = 24 May 2026 [auto-calculated]

Internal notes (visible to other admins, not to customer)
[                                              ]

Email preview
   ▼ Show E-08 (Return Approved) preview

[ Confirm approval — send E-08 ]    [ Cancel ]
```

**Refund-type radio behaviour:**
- Selecting `Exchange` adds a sub-section *"Replacement configuration"* with a SKU search field (autocompletes against the catalogue) — used to create a $0 sister order on the customer's account.
- Selecting `Store credit` shows a generated discount code (CSC-RT-0092-XXXX, single-use, expires in 12 months by default — editable).
- Selecting `Replacement` requires a SKU and inventory check — if the SKU is currently OOS, an inline amber warning: *"SKU is out of stock. Customer will be notified of expected restock date (TC-28)."*

**OD-08 / Made-to-order special case:** if any returned line is `made_to_order = true` AND `condition_tier ∈ ("minor_use", "damaged_by_customer")`, the radios `Cash refund` and `Replacement` are disabled with tooltip *"OD-08: Made-to-order cash refunds require Super Admin override."* Super Admin sees an "Override OD-08" toggle that re-enables them; toggling it adds a mandatory 200-char justification field.

**`Confirm approval` button:**
1. Disabled until refund type, refund amount, and return method all set.
2. Click → loading spinner → API call → on success: drawer closes, status badge flips to "Return Approved", timeline advances, success toast top-right: *"Return RT-0092 approved. E-08 email sent to customer."*
3. Failure modes:
   - 409 conflict (another admin already actioned) → toast *"This return was actioned by [other admin]. Refresh to see latest."*
   - 5xx → red banner inside drawer *"Could not save. Try again or contact your administrator."*; values retained.
   - E-08 send failure (TC-29 retry queue) → still success, banner *"Approval saved. Email queued — will retry up to 3× / 1 min, then 1× / 15 min."*

*Section 3: Mark item received (only visible when status = "Item in Transit")*

A gold-outlined button *"Mark item received"* + a small text *"Starts TC-22 1-business-day refund SLA."* Clicking opens a confirmation modal with item condition assessment (same radios as left column §3). Submitting transitions to "Item Received" and unlocks the *"Initiate refund"* button (SCR-A-08).

*Section 4: Initiate refund (only visible when status = "Item Received")*

Primary gold button *"Initiate refund — $X,XXX.XX"*. Clicking opens SCR-A-08 (refund modal).

*Section 5: Internal notes log*

Same pattern as v1.0 SCR-A-04 §4.

*Section 6: Audit log link*

`View full audit trail →` to `/admin/audit?record=RT-0092`.

---

**Edge cases (Return Detail):**

| Scenario | Design |
|---|---|
| Return is for a bespoke / custom-dimension item | A red banner above the items section: *"Bespoke item — non-returnable per BRD §15.1 unless faulty. Approval requires reason ∈ {Damaged on arrival, Item does not match specification}."* The Approve button is disabled until that condition is met. |
| Return is for a Trade account | Trade badge visible in header. Extended return window (60 days, BRD §15) is automatically applied — eligibility check shown as: *"Within 42 days of delivery — eligible (Trade window: 60 days)."* |
| Customer ship-back deadline (TC-20) breached | A yellow banner appears: *"Customer ship-back deadline elapsed on [date]. Item not received within 14 days of approval. Auto-cancel return?"* with `[Cancel return — notify customer]` button (sends E-10 with reason "Ship-back deadline elapsed"). |
| Stripe webhook for refund (TC-30) not received within 24h after Refund Initiated | An amber alert ribbon at the top: *"Stripe `charge.refunded` event not received for RT-0092 within 24h. Verify in Stripe dashboard."* + `[Open in Stripe ↗]` link. Status stays at "Refund Initiated" until webhook arrives. |
| Two admins viewing the same return; both click Approve | First click wins; second admin sees a 409 toast and the page reloads to show the new state. |
| Customer requests partial return (1 of 3 items) | Each line has its own checkbox + qty. Approval drawer respects only the checked lines. Refund amount auto-calculates from checked lines + condition tier. |
| Admin attempts to delete attached photos | Not allowed. Photos are part of the legal claim record (TC-32: 7-year retention). Only the customer can supplement; admin can attach internal photos via the notes section. |

---

### SCR-A-08 · Refund Processing modal

**Route:** opens as a modal layered over `/admin/returns/[returnId]`. No standalone route.
**Access:** Ops, Super Admin.
**Source:** F-06.3 *Refund Processing*. Triggers TC-22 SLA, calls Stripe `refunds.create` with TC-16 idempotency key.

This is a **modal** (not a drawer) because the refund action is irreversible and demands the admin's full attention — the page behind dims to 60%.

---

**Modal layout (centred, ~520px wide):**

```
INITIATE REFUND — RT-0092

You are about to issue a refund of:

       ┌─────────────────────────────────┐
       │                                 │
       │       $2,099.40 USD             │
       │       to Visa •••• 4242         │
       │                                 │
       └─────────────────────────────────┘

Original charge:    ch_3OkZ8G2aBcDeFgHi  (Stripe)
Order:              HS-2026-004821
Customer:           Alexandra Chen

──────────────────────────────────────────────
Refund breakdown
   Line 1 · Elan Sofa (Walnut)           $1,899.40
   Restocking fee                         −$  0.00
   Original delivery (waived)             $  200.00
   ─────────────────────────────────────────────
   Total refund                          $2,099.40

──────────────────────────────────────────────
Idempotency key (TC-16)   rt-0092-rfd-1715342201
   Auto-generated. Re-clicking [Confirm] within 24h
   will not double-refund.

☐ I confirm this refund. The amount will be returned
   to the customer's original payment method within
   5–10 business days. E-09 (Refund Initiated) will
   be sent to the customer immediately.

       [Cancel]      [Confirm refund — $2,099.40]
```

**Behaviour:**

1. **Confirm button** disabled until checkbox ticked.
2. On click: button shows loading spinner *"Calling Stripe…"*. The TC-15 30-second timeout applies — if Stripe doesn't respond in 30s, the call is cancelled (idempotency key preserved), modal shows: *"Stripe didn't respond in time. The refund may still be processing. Check the order in 1 minute and try again if needed — your idempotency key prevents duplicates."*
3. On Stripe success: modal closes; Return Detail timeline advances to *"Refund Initiated"*; toast *"Refund issued. E-09 sent to customer. Awaiting Stripe `refund.succeeded` webhook (TC-30)."*; KPI Card on Dashboard refreshes.
4. On Stripe declined (rare — usually expired card): modal stays open with red banner *"Stripe declined the refund: [reason]. Try a manual refund via Stripe dashboard, then mark the return as Refund Completed manually."* + `[Mark refund completed manually]` button (logs `refund_completed_manual = true` in audit trail, requires note).

**Edge cases:**

| Scenario | Design |
|---|---|
| Original payment was via "Trade account net terms" (Phase 2) | Modal text changes: *"This was paid via 30-day trade terms. Issue refund as: (•) Credit note against next invoice / ( ) Bank transfer (manual — record reference below)."* The Stripe block is replaced by an internal notes block. |
| Refund amount > original charge (somehow edited) | API rejects; modal banner *"Refund cannot exceed original charge of $X,XXX.XX."* |
| Admin clicks Confirm but loses network mid-call | The TC-16 idempotency key handles retry safely. The modal shows a yellow banner *"Connection issue — re-try when online. Your idempotency key prevents duplicate refunds."* |
| Stripe rate-limited | Toast *"Stripe rate limit hit. Refund queued; will auto-retry in 60s."* — no admin action needed. |
| Restocking fee was set in the approval drawer | Pre-fills the breakdown row. Admin can adjust ±$0.01 until Confirm — beyond that range needs Super Admin role. |

---

### SCR-A-09 · Customer Detail

**Route:** `/admin/customers/[customerId]`
**Access:** Ops (read + limited actions), Super Admin (full + GDPR).
**Source:** F-01.6 *Customer Account Profile Management*, F-08.2 *Right to be Forgotten*.
**Reached from:** SCR-A-06 row click; "View customer" link on any order, return, review.

This screen replaces the brief stub in v1.0 §SCR-A-06.

---

**Page header (full width):**

```
[ ← Back to customers ]

Alexandra Chen   [Active]   [Trade]    alexandra.chen@email.com
Member since 12 Jan 2025 · Last login 3 May 2026 · 8 orders · $48,212 LTV

                     [Email customer]  [Reset password]  [Deactivate ▾]
```

**Status badge:** "Active" green / "Deactivated" grey / "Anonymised (GDPR)" muted with lock icon. The Trade badge appears only for Verified Trade Accounts (BR-003 / BRD §7).

**Action buttons (right):**
- **Email customer** — opens a small dropdown of E-01..E-12 templates; selecting one composes an admin-initiated message via the same template engine. Modal preview before sending.
- **Reset password** — sends the standard password-reset email (TC-07: 1h link, single use). A confirm dialog: *"Send password reset email to alexandra.chen@email.com? The customer must follow the link within 1 hour."* No manual password entry — admin can never set a customer password.
- **Deactivate ▾** — the caret reveals: `Deactivate account / Force logout / Suspend reviews`. Each opens the same confirmation pattern (PAT-01).

---

**Three-column layout:**

```
┌────────────────────────────┬───────────────────┬───────────────────┐
│ LEFT (50%)                 │ CENTRE (25%)      │ RIGHT (25%)        │
│ Order history              │ Profile facts      │ Account actions    │
│ Returns                    │ Saved addresses    │ Internal notes     │
│ Reviews                    │ Saved payment      │ GDPR (Super Admin) │
│ Lifetime value & cohort    │ methods            │                    │
└────────────────────────────┴───────────────────┴───────────────────┘
```

**LEFT — Order history**

A table identical in style to SCR-A-03 but scoped to this customer. Columns: Order # · Date · Total · Status · SLA · Action. Pagination = 10 rows. A `[Show all (47)]` link expands to full list.

**LEFT — Returns**

If the customer has ever requested a return, a card appears with: count badges per status (`2 open · 5 completed · 1 rejected`) + a table of last 5 returns. Click → SCR-A-07.

**LEFT — Reviews**

Last 5 reviews submitted by this customer with star rating, product name, status (Approved / Pending / Rejected). `[View all]` → reviews queue filtered.

**LEFT — Lifetime value card**

```
Total spend          $48,212.00
Avg order value      $6,026.50
Orders this year     3
First order          12 Jan 2025
Largest order        $14,200.00 (HS-2025-001183)
```

**CENTRE — Profile facts**

```
Email           alexandra.chen@email.com  [Verified ✓]
Phone           +1 415 555 0142  [SMS opt-in: ✓]
Account type    Trade — verified 18 Apr 2026
Trade business  Chen Studio LLC
VAT/EIN         12-3456789
Account mgr     J. Clarke
Currency        USD (default)
Locale          en-US
```

The currency / locale fields are read-only here (customer-managed via storefront).

**CENTRE — Saved addresses**

List of addresses with a primary indicator. Each row shows full address + a `[View order history at this address]` link. **No edit** — admin cannot change a customer's address (data integrity + GDPR principle of accuracy + customer self-service via storefront).

**CENTRE — Saved payment methods**

Stripe-tokenised cards only. Shows last-4 and brand. *"Stripe customer ID: cus_XXXXXX [Open in Stripe ↗]"* link for Super Admin.

**RIGHT — Account actions**

Stack of buttons (each follows PAT-01 confirm dialog):
- `[Deactivate account]` — Ops + Super Admin. Customer cannot log in. Existing orders unaffected.
- `[Reactivate account]` — only visible if currently deactivated.
- `[Force logout — invalidate refresh token]` — kills TC-02 token. Customer is logged out next call.
- `[Reset password]` — duplicate of header button.
- `[Resend email verification]` — only if email not verified.
- `[Issue store credit]` — opens a small modal: amount, reason, expiry. Generates a single-use code attached to the customer.
- `[Manually trigger TC-31 retention warning]` — only Super Admin, used in dev.

**RIGHT — Internal notes**

Standard internal notes log (same component as SCR-A-04 §4). Visible to all admins; not customer-facing. Used for *"Phoned to confirm white-glove preference"* type entries.

**RIGHT — GDPR section (Super Admin only)**

> Hidden for Ops. The entire panel is omitted from the DOM for non-Super-Admin sessions (BR-009; not just CSS-hidden).

```
GDPR / CCPA

Data export request   [Export data (JSON)]
   Generates a downloadable JSON of all PII + orders.
   Logs the action in audit trail.

Erasure request       [Process erasure request →]
   Opens SCR-S-04. Cannot be undone. TC-34: complete within 30 days.

CCPA opt-out          ☑ Customer opted out of data sale
   Set when customer used "Do Not Sell My Personal Information" footer link.
   Take effect: TC-37 (immediate).

Marketing consent     ☑ Email marketing  ☐ SMS marketing
   Last updated by customer 12 Apr 2026.
```

**Edge cases (Customer Detail):**

| Scenario | Design |
|---|---|
| Customer has been GDPR-anonymised (TC-32) | Header name shows "[Anonymised customer]"; email shows hash. Order history rows are visible (financial retention) but customer column is muted. All write actions are disabled with tooltip *"Customer record has been anonymised under GDPR. No further actions possible."* |
| Customer has an active checkout reservation (TC-13) | A blue info banner: *"Customer is currently checking out (15-min reservation expires at HH:MM)."* No effect on actions, but Force-logout shows extra warning *"This will void the active checkout reservation."* |
| Trade customer with VAT number that fails VIES (OD-06) | Amber pill next to VAT field: *"VAT validation pending — manual verification needed."* Click opens SCR-TR-02. |
| Customer is locked out (TC-10: 5 failed logins → 15 min) | Status badge becomes "Locked (TC-10)". A `[Unlock now]` button replaces "Force logout". Logs admin action. |
| Customer's last login is null (never logged in after registration) | Show *"Never logged in"* instead of relative date. Suggest *"Re-send verification email"* if email not verified, else *"Customer registered but never returned — consider win-back campaign."* |
| Two admins editing notes simultaneously | Notes are append-only — both notes are kept in the log with timestamps. No conflict. |
| Force-logout clicked while customer has unsaved cart | Cart is preserved (server-side cart). On next login they see it intact (BRD: BR-002 cart merge / persistence). |

---

### SCR-A-10 · Inventory / Stock View

**Route:** `/admin/products?view=stock`
**Access:** Ops (read-only stock view), Super Admin.
**Source:** F-02.5 *Admin Product Management* (read-only branch for Ops). BRD §8.4 lists this as Operations capability.

**Critical role rule:** Ops must NOT see the full Product Form (SCR-B-02) — they cannot edit price, name, images, descriptions, or SEO. They edit stock only. The screen layout reflects that.

---

**Above-table area:**

```
"Inventory"                         [Adjust stock — bulk] [Export CSV]
[Search by SKU or product name]    [Status: All / Low / OOS / Made to Order]
[Collection: All collections ▾]    [Show only my recent edits ☐]
```

The `[Adjust stock — bulk]` button opens a side panel allowing CSV upload of `sku, qty_delta, reason` rows — useful after a physical stocktake.

---

**Table columns:**

| Column | Content |
|---|---|
| Thumbnail | 32×32px image |
| Product | Name + SKU (monospace) |
| Configuration | Configuration summary (e.g. "Walnut / Natural oil / Linen B / 240cm") |
| Stock state | Coloured pill: `In stock` (green), `Low stock` (amber), `Out of stock` (red), `Made to Order` (blue), `Held — checkout` (purple) |
| Qty | The current `stock_qty` in bold. For `Held — checkout` rows, displays *"3 (2 held)"* with the held count in muted text. |
| Threshold | The low-stock threshold (read-only display) |
| Last alert | Date of last low-stock alert fired, with TC-27 cooldown indicator |
| Action | `[Adjust]` button |

**Row colour rules:**
- Out of stock with active checkout holds → faint purple tint (need-to-watch).
- Low stock + last-alert sent today → faint amber tint.

**Click on row** → opens an *adjust stock drawer* (not the full product form):

```
ADJUST STOCK — Elan Sofa / Walnut

Current stock              3 units
Held in active checkout    2 units (TC-13: 15-min reservations)
Made to Order              ☐ (read-only)

Adjustment *
( ) Set absolute value to:    [____]
( ) Add delta:                [+/- ____]

Reason *
[ Stocktake correction ▾ ]
   Stocktake correction / Damaged / Returned to vendor / Internal use / Other

Internal note (optional)
[                                              ]

Effective immediately. The change will:
   • Update the stock_qty value
   • Trigger TC-28 back-in-stock notifications IF crossing 0 → ≥1
   • Reset TC-27 24h alert cooldown if dropping to ≤ threshold

[ Cancel ]                          [ Save adjustment ]
```

A stock adjustment is logged in audit trail (admin ID, before, after, reason, note). Ops cannot delete the SKU, change its threshold, change its price, or rename the product from this screen.

**Inline alerts** at top of table:

- Red banner: *"4 SKUs are out of stock. [Filter to OOS]"*
- Amber banner: *"7 SKUs at or below low-stock threshold. Last alerts sent within TC-27 24h cooldown. [View alerts]"*

---

**Edge cases:**

| Scenario | Design |
|---|---|
| Setting stock_qty = 0 on a non-MtO SKU with active wishlist subscriptions | Confirmation modal: *"This will leave 12 customers with active back-in-stock notifications. They will be re-notified when stock returns. Proceed?"* |
| Setting stock_qty above 0 on an MtO SKU | A small note: *"This SKU is configured as Made to Order. Setting an in-stock quantity will switch it to in-stock display in the storefront. Do you want to disable Made to Order?"* `[Disable MtO and save] [Save without changing MtO]` |
| Threshold higher than current stock + adjustment | Saves successfully, but inline note: *"Stock will fall below threshold — alert will fire after TC-27 cooldown."* |
| Adjustment delta would make stock negative | Validation error: *"Stock cannot go below zero. Use 'Set absolute value' to mark a SKU as out of stock."* |
| Admin imports a bulk stocktake CSV with 200 rows, 3 invalid | Wizard preview shows: 197 will update, 3 will fail with reasons (unknown SKU, negative result). Admin can confirm to apply the 197 valid rows; the 3 are downloaded as a fix-it CSV. |
| Concurrent stock change by another admin | TC-13 / cart system uses atomic `INCRBY`. The drawer detects optimistic-lock conflict and reloads with the latest values; the admin retypes their adjustment. |
| Held-in-checkout count > stock_qty (shouldn't happen, race condition) | A red row banner: *"⚠ Inventory inconsistency — held > stock. Contact engineering. Audit log: [trace ID]"*. Adjustment temporarily disabled. |

---

### SCR-A-11 · Shipping Configuration

**Route:** `/admin/shipping`
**Access:** Ops, Super Admin.
**Source:** F-04.4 *Tax & Shipping Calculation*; BRD §8.4 *Shipping configuration: zones, flat-rate and weight-based rates, white-glove partner management*.

> **Boundary with SCR-S-08:** SCR-A-11 covers operational shipping (zones, rates, partners, cut-offs). Financial-only shipping fields (subsidy %, free-shipping threshold, white-glove min order) live in SCR-S-08 (Super Admin) per BR-009.

---

**Sub-tab navigation (within page):**

`Zones` (default) · `Rates` · `White-glove partners` · `Cut-off rules`

Each tab is its own form view. All edits go through PAT-01 *Save with confirm + audit trail*.

---

**Tab 1 — Zones**

Displays a grouped list of countries/regions, grouped into named zones. Each zone is a card:

```
┌─ ZONE · United States ─────────────────────────────┐
│  46 states + DC + APO/FPO                           │
│  States included: AL, AK, AZ, AR, CA, ...     [▼]  │
│  Includes Alaska & Hawaii: ☐                        │
│  Default delivery method: White-glove if eligible   │
│  Tax handling: Per-state nexus rules                │
│                              [Edit zone] [Disable]  │
└─────────────────────────────────────────────────────┘
```

Clicking `[Edit zone]` opens a side panel:

```
ZONE NAME *           [United States                       ]
COUNTRY/REGION *      [USA — All states ▾]
Specific subdivisions [Multi-select dropdown of states/provinces]
Default method        ( ) Standard parcel
                      ( ) Freight (large items)
                      (•) White-glove (eligible items)
Min order             [$_______]
Max weight per parcel [______] kg
Lead time buffer      [+__] days   Added to product lead time
Tax handling          ( ) Single rate · ( ) Per-subdivision
Active                ☑

[ Save zone ]   [ Cancel ]
```

**Adding a new zone:** `[+ New zone]` button at the top.

---

**Tab 2 — Rates**

A table per zone:

| From weight (kg) | To weight | Rate (USD) | Rate (EUR) | Rate (GBP) | Method |
|---|---|---|---|---|---|
| 0 | 5 | $9.99 | €9.50 | £7.99 | Standard |
| 5.01 | 30 | $24.99 | €22.50 | £18.99 | Standard |
| 30.01 | 80 | $59.99 | €56 | £45 | Freight |
| 80.01 | ∞ | White-glove only | — | — | White-glove |

**White-glove threshold:** `Weight > 80kg` is the default white-glove eligibility rule (matches the SCR-B-02 product form text). Editable here.

Rows are inline-editable; clicking a cell turns it into a number input. Save propagates only after `[Save all rate changes]` at the bottom — prevents partial saves.

**Add row** at the bottom of each zone table.

**Zone-level toggles:**
- `[Override per-product] ☐` — when enabled, individual products can override the zone rate via a per-product shipping override field (rare, used for very large or very fragile pieces).

---

**Tab 3 — White-glove partners**

Card per partner:

```
┌─ Partner · Metro Couriers (San Francisco) ─────────┐
│  Coverage: CA, NV, OR, WA                           │
│  Pricing model: $480 base + $40/15min onsite        │
│  Capacity: 8 deliveries / day                       │
│  API integration: ☐ (manual booking)                 │
│  Contact: ops@metrocouriers.com · +1 415 555 0192   │
│  Active ☑                  [Edit] [View bookings]   │
└─────────────────────────────────────────────────────┘
```

`[View bookings]` → a bookings table showing scheduled white-glove appointments for the next 14 days. Useful before promising a delivery window in SCR-A-04 dispatch form.

---

**Tab 4 — Cut-off rules**

Defines what counts as "today" for the TC-17 / TC-18 SLA clocks. This was a hidden assumption in v1.0; making it explicit prevents disputes.

```
Order cut-off time *      [16:00] (Pacific Time)
   Orders placed after this time start the TC-17 clock the next business day.

Business days *           ☑ Mon ☑ Tue ☑ Wed ☑ Thu ☑ Fri ☐ Sat ☐ Sun
Public holidays           [Manage holidays →]
   2026: 1 Jan, 19 Jan, 16 Feb, 25 May, 4 Jul, ...

Time zone display         (•) Each admin's local TZ
                          ( ) Always America/Los_Angeles
```

Holidays page (`/admin/shipping/holidays`) is a simple list with `[+ Add holiday]` for the next 18 months.

**Edge cases (Shipping):**

| Scenario | Design |
|---|---|
| Admin sets a cut-off at midnight | Allowed but warning *"This effectively gives admins 24h to confirm orders placed any time before next-day cutoff. Verify with Ops lead."* |
| Zone overlap (e.g. EU country listed in both "EU" and "Germany only") | Validation: *"Country DE is already in zone 'European Union'. Remove from one before saving."* |
| Removing a country from a zone with active orders to that country | Allowed; in-flight orders keep their original rate. New orders use the next-best matching zone. Inline note: *"X in-flight orders use the previous rate. New orders only are affected."* |
| Disabling a white-glove partner with bookings in the next 14d | Modal: *"This partner has 5 bookings in the next 14 days. Disabling does not cancel bookings. Reassign manually or contact the partner."* |
| Currency conversion mismatch | The rate table uses fixed display currencies, not auto-FX. Editing only one currency raises a yellow warning *"USD changed; EUR and GBP not updated. Confirm parity manually."* |

---

### SCR-A-12 · Operations Analytics

**Route:** `/admin/reports`
**Access:** Ops, Super Admin (Trade gets a B2B-filtered subset on the same route — see file 3 SCR-TR-06 / file 4 SCR-S-06).
**Source:** BRD §19 KPIs.

---

**Header:**

```
"Analytics"                                [Date range: Last 7 days ▾] [Export ▾]
[Compare with: previous period · same period last year · custom]
```

`Export ▾` reveals: `CSV (current view)`, `Excel (all tabs)`, `PDF (formatted report)`.

---

**Tab navigation:**

`Overview` (default) · `Orders` · `Returns` · `Inventory` · `Customers` · `SLA compliance`

---

**Overview tab — KPI grid:**

3-column grid of cards, ordered by BRD §19:

```
[ Today's revenue           ]  [ Orders today          ]  [ AOV (today)           ]
[ Conversion rate            ]  [ New customers today   ]  [ Returning customers   ]
[ Pending returns            ]  [ Open returns LTV-at-risk ] [ Refund-rate (rolling 30d)]
[ Trade activation rate      ]  [ Trade vs B2C revenue  ]  [ Top-selling configs (7d)]
```

Each card: large value, delta vs comparison period, sparkline (small inline chart). Clicking a card → relevant deeper tab.

---

**SLA compliance tab:**

Table with rows per SLA + a 30-day rolling chart per row:

| SLA | Target | This week | Last week | 30d rolling | Trend |
|---|---|---|---|---|---|
| TC-17 Order confirm 4h | ≥95% | 96% | 94% | 95.4% | ↑ |
| TC-18 Dispatch 48h | ≥95% | 88% | 91% | 89.7% | ↓ |
| TC-21 Return review 2bd | ≥95% | 100% | 96% | 97.1% | ↑ |
| TC-22 Refund initiation 1bd | ≥95% | 75% | 82% | 78.4% | ↓ |
| TC-26 Review moderation 48h | ≥95% | 92% | 96% | 94.3% | ↓ |

Click a row → drill-down: per-admin breakdown (who breached, when, on which records). This is **Super Admin only**; Ops sees aggregate only (no individual blame).

---

**Returns tab — return reasons breakdown:**

Stacked bar chart (per week, by reason from BRD §15.1). Highlights the top reason. Useful for product-quality conversations with the Content team.

---

**Customers tab — cohort retention chart:**

Heat-map: cohort (signup month) × month-since-signup, % returning. Trade vs B2C tabs.

---

**Edge cases:**

| Scenario | Design |
|---|---|
| Date range covers period before store launch | Chart shows "Insufficient data" for the leading days. |
| Trade KPIs visible to Ops | No — Ops sees aggregate revenue split (Trade vs B2C totals only); per-trade-account drill is Trade or Super Admin. |
| Export PDF over a 90-day range | Shows progress modal; PDF is generated server-side and emailed when ready (file > 5MB). |
| Comparison period contains a Stripe outage day | An asterisk + note: *"Comparison period includes Stripe service degradation on 12 Mar 2026. Consider excluding this day."* `[Exclude] [Keep]` |

---

## Operations · Traceability snippet

| Screen | TC- codes | E- triggers | OD- decisions | BR-/Module rule |
|---|---|---|---|---|
| SCR-A-07 Return Detail | TC-09, TC-20, TC-21, TC-22, TC-29, TC-30 | E-08, E-10 | OD-08 | BRD §15 |
| SCR-A-08 Refund modal | TC-15, TC-16, TC-22, TC-30 | E-09 | — | F-06.3 |
| SCR-A-09 Customer Detail | TC-07, TC-10, TC-13, TC-31, TC-37, TC-38 | E-01, E-02 | — | BR-002, F-08.2 |
| SCR-A-10 Inventory | TC-13, TC-27, TC-28 | E-12 | — | F-02.5 |
| SCR-A-11 Shipping | — | — | — | F-04.4, BRD §8.4 |
| SCR-A-12 Analytics | TC-17, TC-18, TC-21, TC-22, TC-26 | — | — | BRD §19 |

For the full cross-role matrix, role × screen access matrix, open questions, and assumptions, see file 5 (`HomeStyle_Admin_Screens_v1.1_05_Shared_Patterns_and_Traceability.md`).

---

*End of file 1 of 5 — Operations*
*Companion files: 02_Content, 03_Trade, 04_SuperAdmin, 05_Shared_Patterns_and_Traceability*
*Read alongside `HomeStyle_BRD_v4.md`, `feature_specs/`, and `references/domain-rules.md`.*
