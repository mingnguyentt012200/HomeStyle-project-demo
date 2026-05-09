# HomeStyle — Admin Panel Screen Descriptions
## File 4 of 5 · Super Admin role
**v1.1 Add-on · May 2026 · Companion to v1.0 · April 2026**

> **About this file pack:** v1.1 was originally a single addendum. This pack splits the addendum by admin role for easier hand-off:
>
> 1. `HomeStyle_Admin_Screens_v1.1_01_Operations.md` — SCR-A-07..A-12
> 2. `HomeStyle_Admin_Screens_v1.1_02_Content.md` — SCR-B-05..B-11
> 3. `HomeStyle_Admin_Screens_v1.1_03_Trade.md` — SCR-TR-02..TR-06
> 4. **`HomeStyle_Admin_Screens_v1.1_04_SuperAdmin.md`** *(this file)* — SCR-S-06..S-10
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

This file describes the **Super Admin** role's add-on screens introduced in v1.1. Super Admin owns all global system configuration: reports library, retention jobs, financial config (tax/shipping/FX), notifications routing, and general/retention system settings.

| ID | Screen | Source spec |
|---|---|---|
| SCR-S-06 | Reports & Exports | BRD §8.4 |
| SCR-S-07 | Retention Jobs | F-08.3, TC-31/32/33/35 |
| SCR-S-08 | Tax & Shipping settings | F-04.4 |
| SCR-S-09 | Notifications settings | F-07.4 |
| SCR-S-10 | General + Data Retention sub-tabs | BRD §8.4 |

**Module access (BRD §7 + `domain-rules.md`):** Super Admin only, with the documented exception that SCR-S-06 has scoped views available to Ops (Ops scope) and Trade (B2B scope) — see SCR-S-06 below for the per-role visibility rules.

**Critical role boundaries to preserve in design:**
- **BR-009:** Financial config (tax model, FX provider, free-shipping threshold, white-glove min order, currency settlement) lives ONLY in SCR-S-08. Ops sees operational shipping (zones, rates, partners, cut-offs) in SCR-A-11 — not the financial layer.
- **BR-013:** Retention jobs are never user-initiated for individual records. Manual runs are for testing and Super Admin emergencies, gated by triple-confirm and a dry-run option.
- **TC-34/35:** Regulatory minimums for retention/erasure are locked in SCR-S-10 with a lock icon and tooltip — they cannot be reduced through UI; only extended.
- **PAT-01 typing-to-confirm:** mandatory for irreversible actions touching customer-facing records (manual TC-31 anonymisation, bulk product delete, GDPR erasure).
- **Audit trail:** every Super Admin action is recorded (admin ID + action + timestamp + entity ID) per `domain-rules.md` §Admin Panel — Shared Rules. Audit links from each screen point to `/admin/audit`.

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

**Per-role visibility rules:**
- **Ops:** sees only Revenue (no per-product P&L), Returns by reason, Inventory turnover, SLA compliance (aggregate), Customer cohort. No discount usage, no Trade-only reports.
- **Trade:** sees Trade vs B2C revenue (Trade column only), B2B-filtered SLA compliance, B2B customer cohort. No global revenue, no inventory turnover.
- **Super Admin:** all of the above.

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

## Super Admin · Traceability snippet

| Screen | TC- codes | E- triggers | OD- decisions | BR-/Module rule |
|---|---|---|---|---|
| SCR-S-06 Reports | — | — | — | BRD §8.4 |
| SCR-S-07 Retention jobs | TC-31, TC-32, TC-33, TC-35 | — | — | F-08.3 |
| SCR-S-08 Tax & shipping | — | — | — | F-04.4 |
| SCR-S-09 Notifications | TC-11, TC-29 | — | — | F-07.4 |
| SCR-S-10 General/Retention | TC-31..TC-38 | — | — | F-08.3 |

For the full cross-role matrix, role × screen access matrix, open questions, and assumptions, see file 5 (`HomeStyle_Admin_Screens_v1.1_05_Shared_Patterns_and_Traceability.md`).

---

*End of file 4 of 5 — Super Admin*
*Companion files: 01_Operations, 02_Content, 03_Trade, 05_Shared_Patterns_and_Traceability*
*Read alongside `HomeStyle_BRD_v4.md`, `feature_specs/`, and `references/domain-rules.md`.*
