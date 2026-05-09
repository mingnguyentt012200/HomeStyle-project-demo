# HomeStyle — Admin Panel Screen Descriptions
## File 5 of 5 · Shared patterns, traceability, open questions, assumptions
**v1.1 Add-on · May 2026 · Companion to v1.0 · April 2026**

> **About this file pack:** v1.1 was originally a single addendum. This pack splits the addendum by admin role for easier hand-off:
>
> 1. `HomeStyle_Admin_Screens_v1.1_01_Operations.md` — SCR-A-07..A-12
> 2. `HomeStyle_Admin_Screens_v1.1_02_Content.md` — SCR-B-05..B-11
> 3. `HomeStyle_Admin_Screens_v1.1_03_Trade.md` — SCR-TR-02..TR-06
> 4. `HomeStyle_Admin_Screens_v1.1_04_SuperAdmin.md` — SCR-S-06..S-10
> 5. **`HomeStyle_Admin_Screens_v1.1_05_Shared_Patterns_and_Traceability.md`** *(this file)* — PAT-01..05, traceability, open Qs, assumptions
>
> **Read order:** v1.0 §1 first (shell, sidebar, topbar, TC-05 timeout, auto-save banner, global search) → this file → role files. The shell defined in v1.0 §1 applies to every screen referenced below without repetition.

**Source map:**
- TC-XX → `HomeStyle_BRD_v4.md` §17 (Time Constraints)
- OD-XX → `HomeStyle_BRD_v4.md` §9 (Open Decisions)
- E-XX → `HomeStyle_BRD_v4.md` §14 (Email Triggers)
- F-XX.X → `feature_specs/F_XX.X_*.md`

**Role colour key:** Operations = teal · Content = purple · Trade = amber · Super Admin = red

---

## Scope of this file

This file holds the shared content that is referenced by all four role files:

- **§5** Cross-cutting UI patterns (PAT-01..05) — define once in Figma, reuse everywhere.
- **§6** Traceability matrix — Screens × Business rules cited, plus Role × Screen access matrix.
- **§7** Open Questions for the client.
- **§8** Assumptions.
- **§9** Index of new screens (consolidated from all role files).

The PAT-* patterns are not screens; they are component specs that every screen in the role files depends on.

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

## 9. Index of new screens (consolidated)

| ID | Screen | Role | File | Source spec |
|---|---|---|---|---|
| SCR-A-07 | Return Detail (full review workflow) | Ops, Super Admin | 01_Operations | F-06.2 |
| SCR-A-08 | Refund Processing modal | Ops, Super Admin | 01_Operations | F-06.3 |
| SCR-A-09 | Customer Detail | Ops, Super Admin | 01_Operations | F-01.6, F-08.2 |
| SCR-A-10 | Inventory / Stock view | Ops, Super Admin | 01_Operations | F-02.5 |
| SCR-A-11 | Shipping Configuration | Ops, Super Admin | 01_Operations | F-04.4 |
| SCR-A-12 | Operations Analytics | Ops, Super Admin | 01_Operations | KPIs §19 |
| SCR-B-05 | Collection List | Content, Super Admin | 02_Content | F-02.1 |
| SCR-B-06 | Collection Editor | Content, Super Admin | 02_Content | F-02.1 |
| SCR-B-07 | Designer Management | Content, Super Admin | 02_Content | F-02.3 |
| SCR-B-08 | CMS Page Management | Content, Super Admin | 02_Content | BRD §8.4 |
| SCR-B-09 | Blog Management | Content, Super Admin | 02_Content | BRD §8.4 |
| SCR-B-10 | Email Template Editor | Content, Super Admin | 02_Content | F-07.3 |
| SCR-B-11 | CSV Import Wizard | Content, Super Admin | 02_Content | F-02.5 |
| SCR-TR-02 | Trade Application Detail | Trade, Super Admin | 03_Trade | F-01.1 (B2B branch) |
| SCR-TR-03 | Active Trade Accounts | Trade, Super Admin | 03_Trade | BRD §7 |
| SCR-TR-04 | Pricing Tiers | Trade, Super Admin | 03_Trade | BRD §8.4 |
| SCR-TR-05 | Account Managers | Trade, Super Admin | 03_Trade | BRD §8.4 |
| SCR-TR-06 | B2B Order Oversight | Trade, Super Admin | 03_Trade | F-05.3 (B2B filter) |
| SCR-S-06 | Reports & Exports | Super Admin (+ Ops/Trade subsets) | 04_SuperAdmin | BRD §8.4 |
| SCR-S-07 | Retention Jobs | Super Admin | 04_SuperAdmin | F-08.3, TC-31/32/33/35 |
| SCR-S-08 | Tax & Shipping settings | Super Admin | 04_SuperAdmin | F-04.4 |
| SCR-S-09 | Notifications settings | Super Admin | 04_SuperAdmin | F-07.4 |
| SCR-S-10 | General + Data Retention sub-tabs | Super Admin | 04_SuperAdmin | BRD §8.4 |
| PAT-01..05 | Cross-cutting UI patterns | All | 05 (this file) | n/a |

---

*End of file 5 of 5 — Shared patterns + traceability*
*Companion files: 01_Operations, 02_Content, 03_Trade, 04_SuperAdmin*
*Read alongside `HomeStyle_BRD_v4.md`, `feature_specs/`, and `references/domain-rules.md`.*
