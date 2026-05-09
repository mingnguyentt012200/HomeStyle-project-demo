# HomeStyle — Admin Panel Screen Descriptions
## File 3 of 5 · Trade role *(Phase 2)*
**v1.1 Add-on · May 2026 · Companion to v1.0 · April 2026**

> **About this file pack:** v1.1 was originally a single addendum. This pack splits the addendum by admin role for easier hand-off:
>
> 1. `HomeStyle_Admin_Screens_v1.1_01_Operations.md` — SCR-A-07..A-12
> 2. `HomeStyle_Admin_Screens_v1.1_02_Content.md` — SCR-B-05..B-11
> 3. **`HomeStyle_Admin_Screens_v1.1_03_Trade.md`** *(this file)* — SCR-TR-02..TR-06
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

This file describes the **Admin — Trade** role's add-on screens introduced in v1.1. Trade is a **Phase 2** role, defined now so the Figma file is ready when the trade portal launches. Trade covers B2B account approval, account management, pricing tiers, account managers, and B2B order oversight (read-only — no fulfilment).

| ID | Screen | Source spec |
|---|---|---|
| SCR-TR-02 | Trade Application Detail | F-01.1 (B2B branch) |
| SCR-TR-03 | Active Trade Accounts | BRD §7 |
| SCR-TR-04 | Pricing Tiers | BRD §8.4 |
| SCR-TR-05 | Account Managers | BRD §8.4 |
| SCR-TR-06 | B2B Order Oversight | F-05.3 (B2B filter) |

**Module access (BRD §7 + `domain-rules.md`):** Trade + Super Admin. SCR-TR-04 (Pricing Tiers) is read-only for Trade; only Super Admin can write tier discounts. SCR-TR-05 (Account Managers) is Super Admin only. Ops and Content have no access to the screens in this file.

**Critical role boundaries to preserve in design:**
- **BR-011:** Trade does not fulfil orders. SCR-TR-06 lets Trade see B2B orders but the dispatch form (v1.0 SCR-A-04) is hidden — *"Fulfilment is handled by Operations."* Trade can leave internal notes only.
- **OD-06 (VAT validation):** SCR-TR-02 supports both VIES auto-validation (Option A) and manual-with-flag (Option B). The toggle in SCR-S-10 system settings controls which path is active. Both branches are designed; one is selected at Phase 2 launch.
- **Trade pricing leakage prevention (BRD §7):** Trade prices are stored as a separate price tier — never as a runtime % discount. SCR-TR-04 changes trigger a regenerate-trade-prices job (not a runtime calc).
- **Tier 3 approval:** Tier 3 pricing requires Super Admin sign-off. SCR-TR-02 disables the Tier 3 radio for non-Super-Admin sessions with a tooltip; the application is saved as a draft and routed for approval.

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

## Trade · Traceability snippet

| Screen | TC- codes | E- triggers | OD- decisions | BR-/Module rule |
|---|---|---|---|---|
| SCR-TR-02 Trade app detail | TC-21 (review) | E-11 | OD-06 | BRD §8.4 |
| SCR-TR-03 Active accounts | — | — | OD-06 | BRD §7 |
| SCR-TR-04 Pricing tiers | — | — | — | BRD §8.4 |
| SCR-TR-05 Account managers | — | — | — | BRD §8.4 |
| SCR-TR-06 B2B oversight | TC-17, TC-18 | — | — | F-05.3 |

For the full cross-role matrix, role × screen access matrix, open questions, and assumptions, see file 5 (`HomeStyle_Admin_Screens_v1.1_05_Shared_Patterns_and_Traceability.md`).

---

*End of file 3 of 5 — Trade*
*Companion files: 01_Operations, 02_Content, 04_SuperAdmin, 05_Shared_Patterns_and_Traceability*
*Read alongside `HomeStyle_BRD_v4.md`, `feature_specs/`, and `references/domain-rules.md`.*
