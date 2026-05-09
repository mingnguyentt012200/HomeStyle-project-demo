# HomeStyle — Admin Panel Screen Descriptions
**Add-on Pack v1.1 · May 2026 · Companion to v1.0 · April 2026**
*Covers screens that were referenced in v1.0 navigation but not fully described, plus all admin sub-screens needed for design hand-off.*

> **How to use this document:** This is an *addendum*. The shell, topbar, sidebar, TC-05 timeout modal, auto-save banner, and global search are defined in v1.0 §1 and apply to every screen below without repetition. Read v1.0 §1 first.
>
> Every screen below cites the originating feature spec from `feature_specs/` (e.g. *F-05.3*) and the relevant TC-/E-/OD- codes from `HomeStyle_BRD_v4.md`. Where a v1.0 screen is referenced by ID (SCR-A-04, etc.), open v1.0 alongside.

**Role colour key (used in this doc and the Figma file):**
- Operations = teal · Content = purple · Trade = amber · Super Admin = red

**Source map for every business rule cited:**
- TC-XX → `HomeStyle_BRD_v4.md` §17 (Time Constraints)
- OD-XX → `HomeStyle_BRD_v4.md` §9 (Open Decisions)
- E-XX → `HomeStyle_BRD_v4.md` §14 (Email Triggers)
- F-XX.X → `feature_specs/F_XX.X_*.md`

---

## 0. Index of new screens

| ID | Screen | Role | Source spec |
|---|---|---|---|
| SCR-A-07 | Return Detail (full review workflow) | Ops, Super Admin | F-06.2 |
| SCR-A-08 | Refund Processing modal | Ops, Super Admin | F-06.3 |
| SCR-A-09 | Customer Detail | Ops, Super Admin | F-01.6, F-08.2 |
| SCR-A-10 | Inventory / Stock view | Ops, Super Admin | F-02.5 |
| SCR-A-11 | Shipping Configuration | Ops, Super Admin | F-04.4 |
| SCR-A-12 | Operations Analytics | Ops, Super Admin | KPIs §19 |
| SCR-B-05 | Collection List | Content, Super Admin | F-02.1 |
| SCR-B-06 | Collection Editor | Content, Super Admin | F-02.1 |
| SCR-B-07 | Designer Management | Content, Super Admin | F-02.3 |
| SCR-B-08 | CMS Page Management | Content, Super Admin | BRD §8.4 |
| SCR-B-09 | Blog Management | Content, Super Admin | BRD §8.4 |
| SCR-B-10 | Email Template Editor | Content, Super Admin | F-07.3 |
| SCR-B-11 | CSV Import Wizard | Content, Super Admin | F-02.5 |
| SCR-TR-02 | Trade Application Detail | Trade, Super Admin | F-01.1 (B2B branch) |
| SCR-TR-03 | Active Trade Accounts | Trade, Super Admin | BRD §7 |
| SCR-TR-04 | Pricing Tiers | Trade, Super Admin | BRD §8.4 |
| SCR-TR-05 | Account Managers | Trade, Super Admin | BRD §8.4 |
| SCR-TR-06 | B2B Order Oversight | Trade, Super Admin | F-05.3 (B2B filter) |
| SCR-S-06 | Reports & Exports | Super Admin | BRD §8.4 |
| SCR-S-07 | Retention Jobs | Super Admin | F-08.3, TC-31/32/33/35 |
| SCR-S-08 | Tax & Shipping settings | Super Admin | F-04.4 |
| SCR-S-09 | Notifications settings | Super Admin | F-07.4 |
| SCR-S-10 | General + Data Retention sub-tabs | Super Admin | BRD §8.4 |
| PAT-01..05 | Cross-cutting UI patterns | All | n/a |

---

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
**Access:** Ops, Super Admin (Trade gets a B2B-filtered subset on the same route).
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

Click a row → drill-down: per-admin breakdown (who breached, when, on which records). This is Super Admin only; Ops sees aggregate only (no individual blame).

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

---

## 2. Role: Admin — Content (additions)

### SCR-B-05 · Collection List

**Route:** `/admin/collections`
**Access:** Content, Super Admin.
**Source:** F-02.1 *Browse by Room & Category*. BRD §8.4 *Product management* references collections as part of catalog hierarchy.

---

**Above-table area:**

```
"Collections"                                          [+ New collection]
[Search collection name]                  [Tree view ☑]   [Flat view]
```

**Tree view (default):** A nested expandable tree.

```
▼ Living Room (parent)                               12 products
   • Sofas                                            8
   • Armchairs                                        4
   • Coffee tables                                    3
▼ Bedroom
   • Beds                                             5
   • Wardrobes                                        2
▶ Dining Room                                         9 products
▶ Outdoor                                             3 products
```

Drag handle (⋮⋮) at the left of each row reorders within parent. Drag onto another node = nest. Drag to root = unnest. Drop validation: a collection cannot become its own descendant; the drop indicator turns red and the move is rejected with toast *"Circular nesting prevented."*

Click on a node → SCR-B-06 (editor).

**Flat view:** Same data but as a flat table: Name · Parent · Slug · Product count · Visible storefront ☑/☐ · Updated · Actions (Edit / Disable / Delete).

**Delete logic:** A collection with 0 products → instant delete after PAT-01 confirm. A collection with products → blocked: *"This collection contains 8 products. Move them or remove the assignment first."* Click `[Manage products]` to bulk-reassign.

---

**Edge cases:**

| Scenario | Design |
|---|---|
| Two admins re-ordering the same tree simultaneously | Last-save-wins; the second admin gets a toast *"Tree was reordered by [other admin]. Refreshing."* |
| Disabling a parent with active children | Modal: *"Disabling a parent collection will hide it from the storefront menu but children remain reachable via direct URL. Confirm?"* |
| URL slug change on a parent | Auto-redirects child URLs (301). Warning: *"Children URLs will redirect. Existing bookmarks remain valid."* |

---

### SCR-B-06 · Collection Editor

**Route:** `/admin/collections/[collectionId]/edit` or `/admin/collections/new`
**Access:** Content, Super Admin.

---

**Sticky topbar (within page):**

```
[ ← Back ]    "Sofas"    [Visible ▾]    [Save draft] [Preview] [Publish]
```

Same pattern as SCR-B-02. Status options: `Draft / Visible / Hidden`.

---

**Two-column layout:**

**LEFT:**

*Section: Core details*

```
Name *           [Sofas                                          ]
Slug *           [sofas                                           ]
                 homestyle.com/c/living-room/sofas
Parent           [Living Room ▾]   (root if none)
Description      [Rich text editor]
                 Shown above the product grid on storefront.
SEO title        [auto-filled]    [60/60]
SEO description  [auto-filled]    [157/160]
```

*Section: Products in this collection*

A drag-to-reorder grid of product cards (thumbnail + name). Click a card → SCR-B-02. `[+ Assign product]` opens a search modal — selecting a product adds it to this collection (and removes from another only if the admin chooses "move" instead of "copy").

**Sort options:**
- Manual (drag order)
- Alphabetical
- Best-selling 30d (auto)
- Newest first
- Price (low → high)

The sort field is saved per collection and used by storefront F-02.1.

*Section: Filters auto-included*

Read-only — derived from products' attributes. *"This collection auto-shows filters: Material (5 values), Finish (4 values), Size (3 values), Price range."* Admin can hide a filter via a checkbox column.

**RIGHT:**

*Section: Hero image*

```
[ Upload hero image ↑ ]
   Recommended 1920×640 px · JPG/PNG/WebP · Max 5MB.
```

*Section: Visibility rules*

```
☑ Visible to B2C customers
☑ Visible to Trade accounts
☐ Visible only to Trade accounts
```

*Section: Schedule (optional)*

```
Publish at        [date/time]   Auto-publishes on this date.
Hide at           [date/time]   Auto-hides on this date.
```

A small note: *"Scheduled jobs run hourly. Allow up to 1h margin around the time."*

*Section: Featured slot*

A toggle: `☑ Feature on storefront homepage`. Only 4 collections may be featured at once — exceeding shows: *"You already have 4 featured collections. Unfeature one first."*

**Edge cases:**

| Scenario | Design |
|---|---|
| Collection has 0 products and is published | Yellow banner *"This collection has no products. The storefront shows an empty grid with a 'Coming soon' message."* |
| Slug change on a published collection | Standard 301 redirect warning (same pattern as SCR-B-02). |
| Hero image is portrait orientation | Auto-cropped to 1920×640 with focal-point picker (default centre). |
| Trade-only visibility + featured on homepage | Blocked: *"Trade-only collections cannot be featured on the public homepage."* |

---

### SCR-B-07 · Designer Management

**Route:** `/admin/designers`
**Access:** Content, Super Admin.
**Source:** F-02.3 (designer credit on PDP).

---

**List view:**

Card grid (3 cols on desktop). Each card: portrait photo, name, bio first 80 chars, product count, status (Active / Inactive). Clicking a card opens the editor.

`[+ New designer]` button top-right.

**Editor (side panel, 480px):**

```
Photo            [Upload portrait ↑]   Recommended 600×600
Name *           [Leander Wong              ]
Slug *           [leander-wong              ]
Bio              [Rich text]
Era              [Modern / Mid-century / Classic / Other ▾]
Country          [Canada ▾]
Decade active    [____]–[____]
Featured ☐
Linked products  [Search & assign ▾]   N products linked

[Save designer]
```

**Edge cases:**

| Scenario | Design |
|---|---|
| Deleting a designer with linked products | Block: *"This designer is credited on N products. Unlink first or merge into another designer."* `[Merge into…]` opens a search to pick a target designer; all credits move there. |
| Designer photo too small | Inline warning *"Image is below 400×400 — will appear pixelated on storefront."* |

---

### SCR-B-08 · CMS Page Management

**Route:** `/admin/cms`
**Access:** Content, Super Admin.
**Source:** BRD §8.4.

---

**List columns:** Title · URL · Type (`Static page` / `Landing page` / `Legal`) · Visible ☑/☐ · Last updated · Actions.

**Page editor (`/admin/cms/[pageId]/edit`):**

Sticky topbar: `[Save draft] [Preview] [Publish]`.

**LEFT:** WYSIWYG rich-text editor. Toolbar: H1/H2/H3, bold, italic, underline, link, image insert, embed (YouTube/Vimeo only — allow-list), block quote, bullet list, numbered list, divider, "Insert product card" (search & embed a live product card), "Insert collection grid" (embed a collection's products as a grid). Switching to *Source view* (HTML) is allowed but stripped of `<script>`, `<iframe>` (except allow-list), and inline event handlers.

**RIGHT:** SEO fields (title, description, og:image), URL slug, publish schedule (same pattern), visibility (B2C / Trade), parent navigation menu.

**Special CMS pages (system-protected):** `Privacy Policy`, `Cookie Policy`, `Terms`, `Returns Policy`, `Trade Terms`, `Contact Us`, `404`. These are flagged in the list with a 🔒 icon. Their slugs cannot be changed (they are referenced by the storefront footer and legal flows). Their content can be edited; a *"Last legal review"* date field appears, with a yellow warning if older than 12 months.

**Edge cases:**

| Scenario | Design |
|---|---|
| Admin attempts to delete `Privacy Policy` | Blocked. *"This is a system-required legal page. It can be edited but not deleted."* |
| Publish a page with no body content | Warning: *"This page has no body content. Storefront will show a blank page. Continue?"* |
| Embedded iframe from disallowed domain | Stripped on save with note *"Embed from [domain] removed (not on allow-list). Contact your administrator to add the domain."* |

---

### SCR-B-09 · Blog Management

**Route:** `/admin/blog`
**Access:** Content, Super Admin.
**Source:** BRD §8.4.

Mirrors SCR-B-08 with these specifics:

- **Categories** (manageable in a sub-tab): up to 12 categories, e.g. *Style guides*, *New launches*, *Designer interviews*.
- **Tags** (folksonomy, free-form).
- **Author** field (linked to admin user, but display name is editable per post — useful for guest authors).
- **Post layout templates**: `Standard`, `Long-form (table of contents)`, `Designer interview (Q&A)`, `Lookbook (image gallery)`.
- **Related products picker**: up to 6 product cards per post.
- **Estimated read time** (auto-calculated from word count).
- **RSS feed** auto-includes published posts; no admin action needed.

Edge cases mirror SCR-B-08 plus:

| Scenario | Design |
|---|---|
| Author admin account deactivated | Posts retain the original author display name; "Posted by" link goes to a static profile page or hides if no profile. |
| Schedule a post for a date in the past | Validation: *"Publish date is in the past — publish immediately?"* `[Publish now] [Reschedule]` |

---

### SCR-B-10 · Email Template Editor

**Route:** `/admin/emails`
**Access:** Content, Super Admin.
**Source:** F-07.3 *Transactional Email System*. BRD §14 lists triggers E-01 through E-12.

This is one of the most-used Content screens. Treat it carefully — a broken email template breaks every customer touchpoint downstream.

---

**List view:**

Table: Trigger code · Name · Last sent · Last 7d send count · Open rate · Status · Actions.

```
E-01  Welcome — registration            Last sent 12 min ago    1,204 sent · 38% open    [Active]   [Edit]
E-02  Email verification                                                                  ...
E-03  Password reset
E-04  Order confirmed (admin)
E-05  Shipping confirmation
E-06  Delivery confirmed
E-07  Return request received
E-08  Return approved
E-09  Refund initiated
E-10  Return rejected
E-11  Trade application received
E-12  Wishlist back-in-stock
E-13  Marketing promotion (manual broadcast)
```

---

**Editor (full page):**

Sticky topbar: `[Save draft] [Send test email ▾] [Publish]`.

**Three-column layout:**

```
┌──────────┬──────────────────────┬──────────────┐
│ Variables │ Editor (subject + body)│ Live preview │
└──────────┴──────────────────────┴──────────────┘
```

**LEFT — Variables panel:**

A scrollable list of every Liquid variable available for this trigger (e.g. for E-05: `{{customer.first_name}}`, `{{order.number}}`, `{{tracking.url}}`, `{{shipping.eta_window}}`). Click a variable to insert at cursor. A `[Sample data]` toggle replaces variables with realistic mock data in the live preview.

**CENTRE — Editor:**

```
Subject *           [Your HomeStyle order is on its way ✨]
                    Use {{}} to insert variables. Avoid all-caps.
Preheader (preview)  [Your tracking link inside.]
                    First 90 chars Gmail/Apple Mail show below subject.

Body
[ Rich-text + Liquid editor                                ]
   [B I U] [link] [H1 H2 H3] [list] [variable ▾] [block ▾]
   Blocks:  Header, Hero image, Order summary table,
            Tracking CTA button, Footer, Divider, Spacer.
   Markdown source toggle for power users.

Plain-text version
[ Auto-generated from rich-text. Editable. ]
```

**RIGHT — Live preview:**

Two tabs: `Desktop` (600px wide) / `Mobile` (320px wide). Renders the email with sample data. A `[Re-render]` button forces a fresh render (in case Liquid template syntax changes). A `[Send test email ▾]` dropdown sends the rendered email to an admin-supplied test address.

---

**Save / publish behaviour:**

- `Save draft` keeps changes invisible to production. Multiple admins can edit; auto-save every 60s (TC-04 protection).
- `Publish` requires PAT-01 confirm: *"Publishing will affect ALL future E-XX emails sent. Continue?"*. Logs version (admin ID, timestamp, before/after diff) — viewable in `[Version history]`.
- `[Version history]` lets admin view, diff, and restore any previous version. Restoring is itself a publish action and is logged.

---

**Edge cases:**

| Scenario | Design |
|---|---|
| Variable `{{order.number}}` removed from a transactional template | Pre-publish warning: *"Removing required variable `order.number` will produce broken emails — customer will receive 'Your HomeStyle order' with no order reference."* |
| Liquid syntax error | Inline red highlight on the offending line + error in Live preview pane. Publish blocked. |
| Admin attempts to edit E-13 (marketing) without consent gating | Warning: *"E-13 is sent only to customers with marketing consent (TC-38). Confirm consent gating."* |
| Test email sent but bouncing | Test panel shows last-send status (Delivered / Bounced / Spam); helps admin diagnose. |
| Reverting to a 6-month-old version that references variables removed since | Pre-restore warning lists missing variables and blocks restore until the body is fixed. |
| Email template uses an image hosted on an external domain | Warning: *"External image — recommended to upload to S3 (TC-09)."* + `[Upload to S3]` button auto-rehosts. |

---

### SCR-B-11 · CSV Import Wizard (Products)

**Route:** opens as a modal-style multi-step wizard from SCR-B-01 `[Import CSV]` button.
**Access:** Content, Super Admin.
**Source:** F-02.5.

---

**Step 1 — Upload:**

```
Step 1 of 4 · Upload your CSV

Drag & drop your file here, or [Browse files]
   Max 10MB · UTF-8 · Headers required

[Download template]   [Download example file]
```

Drop validation: file type, size, encoding. On invalid file: red banner with the specific reason. Sample data link helps content admins who haven't seen the schema.

---

**Step 2 — Map columns:**

The wizard reads the header row and auto-suggests mapping for known column names. Each row of the mapping table is `Your CSV column → HomeStyle field`. Required HomeStyle fields show a red dot if unmapped.

```
Your column          HomeStyle field
SKU                  → SKU *                    [✓ exact match]
Title                → Product name *           [✓ matched]
Description (HTML)   → Description *            [✓ matched]
Price US             → Price (USD) *
Price EU             → Price (EUR)
Stock                → Stock qty *
Lead Time            → Lead time
[unmapped]           → [select field ▾]
```

A *"Skip this column"* option exists for non-mapping columns.

---

**Step 3 — Preview & validate:**

A summary card:

```
1,247 rows analysed
   • 1,180 will UPDATE existing products (matched by SKU)
   • 52 will be CREATED as new products
   • 15 rows have ERRORS — fix or skip

[ Show all errors ]
```

Error row example: *"Row 412: SKU `HS-ELAN-3S-XX` — duplicate of row 87. Skip this row?"* Admin can fix inline (edit cell), skip individual rows, or download an error-only CSV to fix offline.

---

**Step 4 — Confirm & run:**

```
Final review before import:

   1,180 products will be updated
      52 products will be created
      15 rows will be skipped (errors)

   Estimated runtime: ~ 4 minutes
   Run mode:  ( ) Foreground (you'll see live progress)
              (•) Background (you'll get an email when done)

[ Cancel ]                              [ Run import ]
```

Foreground: progress bar (% + rows/sec) within wizard; admin can leave the page (job continues).

**Edge cases:**

| Scenario | Design |
|---|---|
| Import would set price to $0.00 | Per-row warning; defaults to skip unless admin overrides. |
| Import would archive a product (`status=archived`) currently in active orders | Block: archive must be done from product list with the v1.0 SCR-B-01 archive flow (more guard-rails). |
| Network failure during background run | Job is retried up to 3× automatically. After all retries, an admin alert is created with the partial-progress checkpoint so the import can be resumed from the last successful row. |
| Import file uses semicolon delimiter (European Excel) | Auto-detected; admin asked to confirm: *"Semicolon-delimited file detected. Use semicolons? [Yes] [No, treat as comma]"* |

---

---

## 3. Role: Admin — Trade *(Phase 2 additions)*

> Phase 2 scope. Defined now so the Figma file is ready when the trade portal launches.

### SCR-TR-02 · Trade Application Detail

**Route:** `/admin/trade/[applicationId]`
**Access:** Admin — Trade, Super Admin.
**Source:** F-01.1 (B2B branch); BRD §8.4 *Trade account management*; OD-06 (VAT validation strategy).

---

**Page header:**

```
[ ← Back to applications ]

Application TA-2026-00148    [Pending review]    SLA: 18h remaining
Submitted by: jamie@chen-studio.com  ·  Chen Studio LLC  ·  18 Apr 2026 10:14
                                  [Approve ▾]    [Reject]    [Request more info]
```

---

**Three-section layout (single column):**

*Section 1: Business details*

```
Legal business name *      Chen Studio LLC
Trading name               Chen Studio
Country of registration    United States · California
Business reg. number       LLC-CA-2018-09214
VAT number / EIN           12-3456789
   [✓ Validated via VIES on 18 Apr 2026 11:02]   ← OD-06 Option A/B
Trade type                 Interior Designer
Years trading              7
Annual furniture spend     $50K – $250K
Number of locations        2
Website                    chen-studio.com   [Open ↗]
```

The VIES validation row shows one of three states: green check + date (validated), amber warning (unvalidated — manual check needed), red cross (mismatched). When OD-06 chooses Option B (manual), the green tick is replaced by *"Manual verification required"* + `[Mark as verified]`.

*Section 2: Documents (uploaded)*

A grid of document cards. Each card: filename, size, upload time, type tag (`Tax certificate`, `Business registration`, `Designer credentials`, `Insurance`). Click → opens in a viewer modal (PDF / image inline). Each PDF is served via TC-09 15-min presigned URL.

`[Download all (.zip)]` link top-right.

*Section 3: References (optional)*

Up to 3 trade references with company name, contact, phone/email. Each row has a `[Mark as called]` checkbox + a free-text result field — turns into an internal note.

*Section 4: Decision panel (sticky bottom)*

```
Decision

(•) Approve — set pricing tier:
       ( ) Tier 1 — 10% off catalogue
       (•) Tier 2 — 15% off catalogue
       ( ) Tier 3 — 20% off catalogue (Super Admin only)
       ( ) Custom (per-product overrides)

    Account manager *
       [Select admin... ▾]   J. Clarke / R. Patel / ...
       Auto-assign by region: ☑

    VAT exemption           ☑ Apply VAT exemption at checkout
                            (BR-004; default ON for verified trade)

( ) Reject — reason *
       [Select reason ▾]
       - Insufficient business documentation
       - VAT/tax ID could not be verified
       - Trade type outside our scope
       - Insufficient annual spend
       - References did not check out
       - Other (free text required)
       [_________________________________________________]
       Reapply allowed: ☑ after [30] days (BRD §15.X)

( ) Request more info — message to applicant
       [_________________________________________________]
       Sent as E-XX (custom) email. Application stays Pending.

Internal notes (other admins, not applicant)
[                                              ]

[ Submit decision — send email ]
```

**Behaviour:**

- Approve → on submit: account flips to Verified Trade, E-11 (account approved) sent, account manager assigned, trade pricing visible immediately to that user.
- Reject → on submit: E-XX (rejection) sent with reason; applicant blocked from reapplying for the configured cool-off (default 30d, BRD).
- Request more info → emails the applicant; application remains Pending (TC-21 SLA pauses until applicant replies, with a max-wait of 7 days before auto-rejected with reason "No response").
- All three actions trigger PAT-01 confirm with a summary of what's about to happen.

---

**Edge cases:**

| Scenario | Design |
|---|---|
| VAT number returns OD-06 mismatch (name doesn't match registration) | Red banner *"VIES name mismatch: registered name 'Chen Studios Inc' ≠ application name 'Chen Studio LLC'. Verify before approving."* Approve still allowed (Trade admin discretion). |
| Applicant has been rejected before | Banner: *"This applicant was rejected on 12 Feb 2026 (reason: insufficient docs). Reapplication received 18 Apr 2026 — 65 days later. Cool-off elapsed."* |
| Applicant uploaded malicious file (caught by AV scan) | The file card shows *"⚠ Quarantined — virus detected."* + `[Notify applicant]` button (sends an automated request to re-upload). Application remains Pending. |
| Two trade admins reviewing same app | First decision wins; second sees *"Already actioned by [admin]"* and is redirected to active queue. |
| Approving without picking an account manager | Validation: *"Account manager is required for approval."* |
| Pricing tier 3 selected by non-Super Admin | Field disabled with tooltip: *"Tier 3 requires Super Admin approval. Save as draft and notify Super Admin."* |

---

### SCR-TR-03 · Active Trade Accounts

**Route:** `/admin/trade/accounts`
**Access:** Admin — Trade, Super Admin.

---

**Above-table:**

```
"Active trade accounts"                                 [+ Manual onboarding]
[Search by business or contact]   [Pricing tier ▾]   [Account manager ▾]   [Status ▾]
```

**Status filters:** Active · Suspended · Inactive (no orders 12mo+) · Pending VAT re-verification.

**Table columns:** Business name · Contact · Pricing tier · Account manager · LTV · Last order · Status · Actions.

**Row actions:** `View →` (account detail), `Reassign manager`, `Suspend ▾` (with reason), `Re-verify VAT`.

**Account detail page** (`/admin/trade/accounts/[id]`):

Three tabs: `Profile · Orders · Pricing & overrides`.

- Profile tab: same fields as SCR-TR-02 + audit log.
- Orders tab: B2B orders only (filtered); same structure as SCR-A-03 with VAT-exempt indicator.
- Pricing & overrides tab: shows the assigned tier + a list of per-product price overrides. `[+ Add override]` lets admin set a special price on a specific SKU for this trade account (e.g. ongoing project). All overrides logged.

**Edge cases:**

| Scenario | Design |
|---|---|
| Manual onboarding (`+ Manual onboarding`) | Same form as SCR-TR-02 §1, but pre-marked "Verified — manual onboarding by [admin]" — used when a sales rep onboards offline and needs the system to reflect it. |
| Suspending an account with open orders | Modal: *"This account has 2 open orders. Suspend stops new logins but does not affect open orders. Continue?"* |
| Re-verify VAT triggered | Calls VIES; updates status badge; if mismatch, account moves to "Pending VAT re-verification" (cannot place new orders, can browse). |

---

### SCR-TR-04 · Pricing Tiers

**Route:** `/admin/trade/tiers`
**Access:** Admin — Trade (read), Super Admin (write).
**Source:** BRD §8.4 *Trade pricing management*.

---

A simple table:

| Tier | Discount | Min annual spend | # Accounts | VAT-exempt default | Default for new approvals |
|---|---|---|---|---|---|
| Tier 1 | 10% | $0 | 28 | ☑ | ☑ |
| Tier 2 | 15% | $50,000 | 14 | ☑ | ☐ |
| Tier 3 | 20% | $250,000 | 4 | ☑ | ☐ |
| Custom | per-account | n/a | 7 | ☑ | ☐ |

`[+ New tier]` opens a side panel.

**Critical rule:** Discounts are applied via separate price tier (BRD: "Trade prices are stored as a separate price tier (not a percentage discount applied at runtime, to avoid display leakage)"). The tier discount is a *catalog generation* setting — clicking "Save tier" triggers a regenerate-trade-prices job. A progress banner shows: *"Recalculating trade prices for [N] products (~3 min)…"* — admin can leave the page.

**Edge cases:**

| Scenario | Design |
|---|---|
| Editing a tier's discount with active accounts | Modal: *"This will recalculate prices for [N] accounts. Existing in-flight orders are unaffected. Confirm?"* |
| Deleting a tier with active accounts | Block. Migrate accounts first via SCR-TR-03 row action. |
| Custom tier with 0 accounts | Allowed — the empty placeholder is fine. |

---

### SCR-TR-05 · Account Managers

**Route:** `/admin/trade/managers`
**Access:** Super Admin.
**Source:** BRD §8.4.

A simple roster — admin users (Ops or Trade roles) flagged as account managers. Each row: name, role, accounts assigned (count), regions, capacity (configurable max), recent activity.

**`[+ Add manager]`:** picks from existing admin users + sets regions + max accounts.

**Bulk reassignment tool:** if a manager is deactivated, their accounts surface as *"No account manager assigned"*; from this screen, `[Reassign all from X to Y]` lets a Super Admin bulk-move them.

**Edge cases mirror SCR-S-02** for admin-user lifecycle.

---

### SCR-TR-06 · B2B Order Oversight

**Route:** `/admin/orders?account_type=trade`
**Access:** Admin — Trade (read all), Super Admin.

A view of SCR-A-03 filtered to trade orders. Adds two columns:

- **Account manager** — who's responsible.
- **Payment terms** — `Card` / `Net 30 invoice` / `Bank transfer`.

Trade admins see the same Order Detail (SCR-A-04) but with **no dispatch action** — only operations admins fulfil. Trade admin can leave internal notes and view all data; the dispatch form is hidden with text *"Fulfilment is handled by Operations. Use the notes section to flag concerns."*

---

---

## 4. Role: Super Admin (additions)

### SCR-S-06 · Reports & Exports

**Route:** `/admin/reports/library`
**Access:** Super Admin (full set), Ops/Trade (subset filtered to their scope).
**Source:** BRD §8.4 *Reports: revenue by date range / product / category, trade vs consumer split, top-selling configurations — exportable to PDF and Excel.*

---

**Layout:** Left side — list of report types (cards). Right side — selected report's parameters + run button.

**Report types (cards):**

```
[ Revenue by date range ]    [ Top-selling configs ]    [ Returns by reason ]
[ Trade vs B2C revenue  ]    [ Customer cohort     ]    [ SLA compliance    ]
[ Inventory turnover    ]    [ Refund analysis     ]    [ Discount usage    ]
```

Click a card → right pane updates with that report's parameters.

**Parameters pane (example: Revenue by date range):**

```
Date range *           [Last 30 days ▾]
Group by *             ( ) Day  (•) Week  ( ) Month  ( ) Product  ( ) Category
Currency               [USD ▾]
Channel filter         ☑ B2C   ☑ Trade
Output format          ( ) On-screen  (•) Excel  ( ) PDF  ( ) CSV
Email when ready ☐     [admin@homestyle.com]

[ Run report ]
```

`[Run report]` queues a job. For on-screen, the result renders inline (chart + table). For Excel/PDF/CSV, the file generates server-side; small jobs finish in <30s and download immediately, large jobs (>30s) email the admin a download link (expires in 7 days). All exported PDFs include a header (HomeStyle wordmark, generation timestamp, admin name, parameters) and a footer (page #, *"Confidential — internal use only"*).

**Saved reports:** Each report can be saved as a *"My saved reports"* template with the current parameters. Saved reports also support scheduling (daily / weekly / monthly) — the file is emailed on schedule.

**Edge cases:**

| Scenario | Design |
|---|---|
| Date range > 1 year and groupBy = Day | Warning *"This will produce >365 rows; consider Week or Month."* |
| Currency selected ≠ admin's locale | Conversion footnote on the export: *"Converted at FX rate as of [date]. Settlement currency for each order is preserved in raw rows."* |
| Scheduled report owner is deactivated | Schedule pauses with a Super Admin alert: *"Report 'Weekly revenue' owner is deactivated. Reassign or delete."* |

---

### SCR-S-07 · Retention Jobs

**Route:** `/admin/system/retention`
**Access:** Super Admin only.
**Source:** F-08.3 *Data Retention Jobs*; TC-31 (3y PII), TC-32 (7y financial), TC-33 (90d logs), TC-35 (90d backup purge).

---

**Layout:** A list of cron-style jobs.

| Job | Cadence | Last run | Next run | Records affected (last) | Status | Actions |
|---|---|---|---|---|---|---|
| TC-31 PII retention warning (3y inactivity) | Daily 02:00 | 5 May 02:00 | 6 May 02:00 | 12 emails sent | ✓ OK | `[Run now] [View log]` |
| TC-31 PII anonymisation (after warning + 30d no response) | Daily 02:30 | 5 May 02:30 | 6 May 02:30 | 4 customers anonymised | ✓ OK | `[Run now] [View log]` |
| TC-32 Financial retention check (7y) | Monthly 1st 03:00 | 1 May | 1 Jun | 0 records purged | ✓ OK | |
| TC-33 Application log purge (90d rolling) | Daily 03:00 | 5 May | 6 May | 412,891 rows | ✓ OK | |
| TC-35 Backup purge (post-erasure) | Daily 04:00 | 5 May | 6 May | 0 backups touched | ✓ OK | |
| TC-27 Low-stock alert daemon | Hourly | 5 May 14:00 | 5 May 15:00 | 3 alerts fired | ✓ OK | |
| TC-28 Back-in-stock notifier | Hourly | 5 May 14:00 | 5 May 15:00 | 18 customers notified | ✓ OK | |

**Click a job → detail page:**

```
TC-31 PII retention warning

Schedule          Daily 02:00 UTC
Description       Sends a 30-day warning email (E-XX) to customers who
                  have been inactive for 3 years (TC-31). After 30 days
                  with no login, the anonymisation job runs.

Last 10 runs
   5 May 02:00   ✓ OK   12 emails sent · 2.4s
   4 May 02:00   ✓ OK   8 emails sent · 1.9s
   ...

[Run manually]   [Pause schedule]   [View logs]
```

**Critical rule:** retention jobs are **never user-initiated for individual records** — they run on schedule. Manual runs are for testing and Super Admin emergencies only, and require a secondary confirm with reason.

**Edge cases:**

| Scenario | Design |
|---|---|
| A retention job has been failing for >24h | A red row banner + a system alert. Job page shows the last error stack trace. `[Acknowledge & escalate]` opens a runbook link. |
| Manual run of TC-31 anonymisation | Triple-confirm modal explaining permanence + an opt-in *"This is a test/dry-run"* checkbox that runs without committing changes. |
| Pausing a retention job for >7 days | Auto-creates a Super Admin alert — long pauses risk regulatory non-compliance. |

---

### SCR-S-08 · Tax & Shipping settings

**Route:** `/admin/settings/tax-shipping`
**Access:** Super Admin only (BR-009: Ops cannot touch financial config).
**Source:** F-04.4.

---

This is a sub-tab of v1.0 SCR-S-01 not detailed in v1.0. Three sections:

*Tax mode:*

```
Tax handling model
   (•) Inclusive (EU) — display prices include VAT
   ( ) Exclusive (US) — tax shown separately at checkout
   ( ) Mixed (per-region) — auto by IP/country

Default VAT rate (EU)        [20] %
Per-country overrides        [+ Add override]
   • Germany 19%
   • Ireland 23%
   • France 20%
US sales tax engine          [TaxJar ▾]   API key: ••••••••
   Nexus states              [Multi-select ▾]   CA, NY, FL, TX
```

*Shipping settings:*

A short form pointing to SCR-A-11 (canonical) for zone/rate edits. Includes only a few financial-only fields:

```
Shipping subsidy %        [0] %    Discount applied to shipping for orders > $X
White-glove min order     [$1,500]
Free shipping threshold   [$2,500]   Apply to: ☑ B2C ☐ Trade
```

*Currency settings:*

```
Settlement currency       USD (locked — internal storage)
Display currencies        ☑ USD  ☑ EUR  ☑ GBP
FX provider               [openexchangerates.org ▾]   API key: ••••••••
FX refresh cadence        Every 24h at 06:00 UTC
Manual FX override        [Edit rates →]   Used in emergencies; always logged.
```

**Edge cases:**

| Scenario | Design |
|---|---|
| Changing tax model on a live store | Modal: *"This will affect every product price displayed on the storefront. Existing orders unaffected. Confirm with Finance team. Continue?"* |
| Removing a display currency that customers have selected | Auto-reset their preferences to the next available currency at next session; warning: *"~340 customers currently use EUR; they will see USD on next visit."* |

---

### SCR-S-09 · Notifications settings

**Route:** `/admin/settings/notifications`
**Access:** Super Admin.
**Source:** F-07.4 *Admin Alerts*. Companion to v1.0 SCR-S-03.

This is the admin-side counterpart of "Configure alerts" referenced in v1.0 SCR-S-03 — defined in full here.

---

**Layout:** Per-channel forms.

*Email recipients per alert category:*

```
SLA breach — order            [ops@homestyle.com] [+ Add]
SLA breach — return           [ops@homestyle.com]
SLA breach — refund           [finance@homestyle.com]
Stripe webhook missing        [finance@homestyle.com]
Low-stock critical            [ops@homestyle.com]
Trade application pending     [trade@homestyle.com]
Admin login lockout (TC-11)   [security@homestyle.com]
GDPR erasure due in 5d        [legal@homestyle.com]
Retention job failure         [it@homestyle.com]
```

*Slack:*

```
Webhook URL                 [https://hooks.slack.com/services/T0/B0/X]
Default channel             [#homestyle-ops]
Severity filter             ☑ Critical only ☐ All

[Test webhook]
```

*SMS / pager (optional, off by default):*

```
Twilio account SID          [_________________________]
On-call rotation            [PagerDuty ▾]
Trigger only on:            ☑ TC-11 admin lockout (security incident)
                            ☑ Retention job failure
                            ☐ SLA breaches (too noisy)
```

*Quiet hours:*

```
Quiet hours apply to        ( ) All notifications  (•) Slack only
From [22:00] To [07:00]   Time zone: [America/Los_Angeles ▾]
Critical alerts always send: ☑
```

**Edge cases:**

| Scenario | Design |
|---|---|
| Slack webhook URL is invalid | `[Test webhook]` returns red error + posts no message. Save blocked until valid. |
| Quiet hours misconfigured (start = end) | Validation *"Quiet hours window must be > 0 minutes."* |
| Pager rotation calls a deactivated admin | Pager system handles fallback per its own rules; HomeStyle shows last-success status only. |

---

### SCR-S-10 · General + Data Retention sub-tabs (System Settings)

**Route:** `/admin/settings/general` and `/admin/settings/retention`
**Access:** Super Admin.

These were referenced in v1.0 SCR-S-01 sub-nav but not described. Defined now as small forms.

*General sub-tab:*

```
Store name                    [HomeStyle]
Storefront URL                [https://homestyle.com]
Admin URL                     [https://admin.homestyle.com] (read-only)
Logo (light bg)               [upload ↑]
Logo (dark bg)                [upload ↑]
Favicon                       [upload ↑]
Default locale                [en-US ▾]
Default currency              [USD ▾]
Maintenance mode              ☐ Enable
   Storefront message         [We'll be back in 30 minutes — sorry for the inconvenience.]
   Admin panel always accessible during maintenance.
```

*Data retention sub-tab:*

```
TC-31 Customer PII retention       [3] years inactivity
   Warning email sent at           [3y - 30d]   (locked: regulatory)
TC-32 Order/financial retention    [7] years from order date  (locked: regulatory)
TC-33 Server/app log retention     [90] days
TC-34 GDPR erasure deadline        [30] days  (locked: regulatory)
TC-35 Backup purge after erasure   [90] days  (locked: regulatory)
TC-36 Cookie consent validity      [12] months
TC-37 CCPA opt-out                 immediate (locked: regulatory)
TC-38 Marketing unsubscribe SLA    [10] business days  (locked: regulatory)
```

Locked fields show a small lock icon + tooltip *"Regulatory minimum — cannot be reduced. Contact Legal if a longer retention is needed."*

---

---

## 5. Cross-cutting UI patterns

These patterns apply across many admin screens. Define once in Figma and reuse.

---

### PAT-01 · Confirmation dialog (destructive / state-changing)

**Use for:** publish, archive, deactivate, delete (where allowed), bulk operations, refunds, GDPR actions, retention manual runs.

**Layout:** centred modal, ~440px wide, dark backdrop 60%.

```
┌───────────────────────────────────────────────────┐
│  [icon — colour by severity]                      │
│  Heading: Action verb + object                    │
│  Body: One-sentence consequence in plain English. │
│        If irreversible, say so explicitly.        │
│                                                   │
│  [Optional: structured side-effects list]         │
│    • Side effect 1                                │
│    • Side effect 2                                │
│                                                   │
│  [Optional: confirmation typing]                  │
│    Type "DELETE" to confirm:    [_______]         │
│                                                   │
│  [ Cancel ]                  [ Primary action ]   │
└───────────────────────────────────────────────────┘
```

**Severity colours:**
- Yellow icon: re-versible (publish, deactivate)
- Red icon: irreversible or financial (refund, GDPR erasure, delete)

**Typing-to-confirm:** required when the action is irreversible AND affects ≥1 customer-facing record (GDPR erasure, manual retention runs, bulk product delete).

**Keyboard:** `Esc` cancels. `Enter` does **not** auto-confirm a destructive action — admin must click the primary button (intentional friction).

---

### PAT-02 · Image upload + crop

**Use for:** product images, swatches, hero images, blog featured images, designer portraits, logos.

**Layout:** drop-zone or click-to-upload → upload progress → crop modal.

**Crop modal:** crop frame + rotation + zoom. Aspect ratio is **locked per use-case**:
- Product: 1:1 square
- Hero: 3:1 wide
- Designer portrait: 1:1
- Blog featured: 16:9

Focal point picker for cases where the image will be re-cropped at multiple aspect ratios on the storefront. Shows live previews at each storefront breakpoint.

**Constraints:** max 10MB / 8000×8000px. PNG, JPG, WebP. Background uploads use S3 multipart for files >5MB; admin can navigate away.

---

### PAT-03 · Bulk-action toolbar

When admin selects ≥1 row in any table (via row checkboxes — present in product list, order list, customer list, returns list):

```
[ ✓ 12 selected ]   [Bulk action ▾]   [Clear selection]
```

The toolbar **replaces** the table header during selection, so it is impossible to miss. Bulk actions per table are role-gated and limited (no bulk-delete in Phase 1 except for Drafts).

Bulk action confirmation always uses PAT-01.

---

### PAT-04 · Loading / empty / error states

Every list and detail page has these three states. Treat them as first-class designs.

**Loading:** skeleton rows for tables; skeleton cards for grids. No spinners on initial load — skeletons feel faster.

**Empty:** illustration + clear sentence + primary action. Wording examples:
- "No orders yet today. Orders will appear here as customers checkout." [Go to dashboard]
- "No products in this collection. Add products from the catalog." [+ Assign products]
- "All clear — no active alerts." (no action — this is a reward state)

**Error:** specific cause + retry action. Never *"Something went wrong"* alone.
- "Couldn't load orders. Check your connection." [Retry]
- "Stripe API is unreachable. Refunds will be queued until connection restores." [View status page ↗]

---

### PAT-05 · Forbidden page (403)

When an admin reaches a URL outside their role permissions (BR-009/010/011/etc.):

```
┌───────────────────────────────────────────────────┐
│   🔒                                                │
│   You don't have permission to view this page.    │
│   This area is for [required role].                │
│                                                   │
│   You're signed in as: J. Clarke (Operations)     │
│                                                   │
│   [ ← Back to your dashboard ]                    │
│                                                   │
│   Need access? Contact your Super Admin.          │
└───────────────────────────────────────────────────┘
```

The page sits inside the normal admin shell (sidebar + topbar) so the admin can navigate elsewhere immediately. The 403 is logged in the audit trail (admin + URL + timestamp) — useful for spotting probing or misclicks.

---

---

## 6. Appendix — Traceability matrix

### Screens × Business rules cited

| Screen | TC- codes | E- triggers | OD- decisions | BR-/Module rule |
|---|---|---|---|---|
| SCR-A-07 Return Detail | TC-09, TC-20, TC-21, TC-22, TC-29, TC-30 | E-08, E-10 | OD-08 | BRD §15 |
| SCR-A-08 Refund modal | TC-15, TC-16, TC-22, TC-30 | E-09 | — | F-06.3 |
| SCR-A-09 Customer Detail | TC-07, TC-10, TC-13, TC-31, TC-37, TC-38 | E-01, E-02 | — | BR-002, F-08.2 |
| SCR-A-10 Inventory | TC-13, TC-27, TC-28 | E-12 | — | F-02.5 |
| SCR-A-11 Shipping | — | — | — | F-04.4, BRD §8.4 |
| SCR-A-12 Analytics | TC-17, TC-18, TC-21, TC-22, TC-26 | — | — | BRD §19 |
| SCR-B-05 Collections list | — | — | — | F-02.1 |
| SCR-B-06 Collection editor | — | — | — | F-02.1 |
| SCR-B-07 Designers | — | — | — | F-02.3 |
| SCR-B-08 CMS | — | — | — | BRD §8.4 |
| SCR-B-09 Blog | — | — | — | BRD §8.4 |
| SCR-B-10 Email templates | TC-29, TC-38 | E-01..E-13 | — | F-07.3 |
| SCR-B-11 CSV import | — | — | — | F-02.5 |
| SCR-TR-02 Trade app detail | TC-21 (review) | E-11 | OD-06 | BRD §8.4 |
| SCR-TR-03 Active accounts | — | — | OD-06 | BRD §7 |
| SCR-TR-04 Pricing tiers | — | — | — | BRD §8.4 |
| SCR-TR-05 Account managers | — | — | — | BRD §8.4 |
| SCR-TR-06 B2B oversight | TC-17, TC-18 | — | — | F-05.3 |
| SCR-S-06 Reports | — | — | — | BRD §8.4 |
| SCR-S-07 Retention jobs | TC-31, TC-32, TC-33, TC-35 | — | — | F-08.3 |
| SCR-S-08 Tax & shipping | — | — | — | F-04.4 |
| SCR-S-09 Notifications | TC-11, TC-29 | — | — | F-07.4 |
| SCR-S-10 General/Retention | TC-31..TC-38 | — | — | F-08.3 |
| PAT-01..05 Patterns | — | — | — | n/a |

### Role × Screen access matrix (reflects BRD §7 + `domain-rules.md`)

| Screen | Ops | Content | Trade | Super Admin |
|---|---|---|---|---|
| SCR-A-07 Return Detail | ✓ | ✗ | ✗ | ✓ |
| SCR-A-08 Refund modal | ✓ | ✗ | ✗ | ✓ |
| SCR-A-09 Customer Detail | ✓ (no GDPR) | ✗ | ✗ | ✓ (full) |
| SCR-A-10 Inventory | ✓ | ✗ | ✗ | ✓ |
| SCR-A-11 Shipping | ✓ | ✗ | ✗ | ✓ |
| SCR-A-12 Analytics | ✓ (Ops scope) | ✗ | ✓ (B2B subset) | ✓ |
| SCR-B-05 Collection List | ✗ | ✓ | ✗ | ✓ |
| SCR-B-06 Collection Editor | ✗ | ✓ | ✗ | ✓ |
| SCR-B-07 Designers | ✗ | ✓ | ✗ | ✓ |
| SCR-B-08 CMS | ✗ | ✓ | ✗ | ✓ |
| SCR-B-09 Blog | ✗ | ✓ | ✗ | ✓ |
| SCR-B-10 Email templates | ✗ | ✓ | ✗ | ✓ |
| SCR-B-11 CSV import | ✗ | ✓ | ✗ | ✓ |
| SCR-TR-02 Trade app detail | ✗ | ✗ | ✓ | ✓ |
| SCR-TR-03 Active accounts | ✗ | ✗ | ✓ | ✓ |
| SCR-TR-04 Pricing tiers | ✗ | ✗ | read | ✓ |
| SCR-TR-05 Account managers | ✗ | ✗ | ✗ | ✓ |
| SCR-TR-06 B2B oversight | ✗ | ✗ | ✓ (read) | ✓ |
| SCR-S-06 Reports | ✓ (Ops scope) | ✗ | ✓ (B2B) | ✓ (full) |
| SCR-S-07 Retention | ✗ | ✗ | ✗ | ✓ |
| SCR-S-08 Tax & shipping | ✗ | ✗ | ✗ | ✓ |
| SCR-S-09 Notifications | ✗ | ✗ | ✗ | ✓ |
| SCR-S-10 General/Retention | ✗ | ✗ | ✗ | ✓ |

---

## 7. Open Questions (BA → Client)

1. **OD-06 (VAT validation):** Confirm whether VIES validation is automatic (Option A) or manual-with-flag (Option B). SCR-TR-02 supports both but needs the toggle locked before Phase 2 launch.
2. **OD-08 (Made-to-order refunds):** Confirm "exchange or store credit only unless faulty" is the binding rule. SCR-A-07 disables cash-refund radios for MtO non-fault returns; ensure Super Admin override is acceptable to Finance.
3. **Restocking fee cap:** SCR-A-07 and SCR-A-08 default to 15% cap. Confirm with Finance.
4. **Email template versioning:** SCR-B-10 supports unlimited version history. Confirm any retention limit (e.g. last 24 months).
5. **Tier 3 trade pricing approval:** SCR-TR-02 requires Super Admin to assign Tier 3. Confirm the approval workflow (auto-route, manual hand-off, etc.).
6. **Quiet hours scope:** SCR-S-09 currently allows quiet hours on Slack only. Confirm whether email alerts should also be queued.
7. **Bulk delete scope:** PAT-03 currently disallows bulk-delete except Drafts. Confirm acceptable bulk operations for Phase 1.
8. **Self-service for retention overrides:** SCR-S-07 prevents pausing for >7d without alerting. Confirm this threshold with Legal.

---

## 8. Assumptions

- Sidebar nav items in v1.0 §1.3 are authoritative; this doc adds screens reachable from those items but does not alter nav structure.
- All admin actions on this doc are logged in the audit trail (per `domain-rules.md` §Admin Panel — Shared Rules). Where a screen mentions "logged" or "audit trail link", the canonical record is `/admin/audit`.
- Email templates E-01..E-12 referenced are from BRD §14; E-13 (marketing broadcast) is added by this doc as a separate template type with consent gating.
- The 80kg white-glove threshold is treated as the default (matches v1.0 SCR-B-02). SCR-A-11 makes it configurable per zone for partner overrides.
- SCR-TR-* screens are scoped for Phase 2 launch; the design can be built now and gated by feature flag.
- Where v1.0 and this addendum overlap (e.g. v1.0 SCR-A-06 stub vs SCR-A-09 full), this addendum supersedes for design hand-off.

---

*End of document — HomeStyle Admin Panel Screen Descriptions v1.1 Addendum*
*Companion to v1.0. Read alongside `HomeStyle_BRD_v4.md`, `feature_specs/`, and `references/domain-rules.md`.*
