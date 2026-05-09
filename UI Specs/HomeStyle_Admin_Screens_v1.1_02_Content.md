# HomeStyle — Admin Panel Screen Descriptions
## File 2 of 5 · Content role
**v1.1 Add-on · May 2026 · Companion to v1.0 · April 2026**

> **About this file pack:** v1.1 was originally a single addendum. This pack splits the addendum by admin role for easier hand-off:
>
> 1. `HomeStyle_Admin_Screens_v1.1_01_Operations.md` — SCR-A-07..A-12
> 2. **`HomeStyle_Admin_Screens_v1.1_02_Content.md`** *(this file)* — SCR-B-05..B-11
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

This file describes the **Admin — Content** role's add-on screens introduced in v1.1. Content covers everything customer-facing on the storefront that isn't a transaction: collections, designers, CMS pages, blog, email templates, and bulk product imports.

| ID | Screen | Source spec |
|---|---|---|
| SCR-B-05 | Collection List | F-02.1 |
| SCR-B-06 | Collection Editor | F-02.1 |
| SCR-B-07 | Designer Management | F-02.3 |
| SCR-B-08 | CMS Page Management | BRD §8.4 |
| SCR-B-09 | Blog Management | BRD §8.4 |
| SCR-B-10 | Email Template Editor | F-07.3 |
| SCR-B-11 | CSV Import Wizard | F-02.5 |

**Module access (BRD §7 + `domain-rules.md`):** Content + Super Admin only. Ops and Trade have no access to the screens in this file.

**Critical role boundaries to preserve in design:**
- **BR-010:** Content cannot see orders, returns, refunds, customer payment methods, or trade pricing. None of those entities appear on Content screens.
- **BR-010:** Content owns product *content* (name, description, images, SEO, taxonomy, copy). Content owns SCR-B-11 (CSV import) and the v1.0 SCR-B-02 product form. **Stock-level edits remain with Ops** (see file 1 SCR-A-10).
- **Email templates (SCR-B-10):** broken templates break every customer touchpoint downstream. PAT-01 confirm + version history + test-send are mandatory before any save reaches production.
- **CMS legal pages (SCR-B-08):** system-protected — slug locked, deletion blocked, with a 12-month "last legal review" warning.

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

## Content · Traceability snippet

| Screen | TC- codes | E- triggers | OD- decisions | BR-/Module rule |
|---|---|---|---|---|
| SCR-B-05 Collections list | — | — | — | F-02.1 |
| SCR-B-06 Collection editor | — | — | — | F-02.1 |
| SCR-B-07 Designers | — | — | — | F-02.3 |
| SCR-B-08 CMS | — | — | — | BRD §8.4 |
| SCR-B-09 Blog | — | — | — | BRD §8.4 |
| SCR-B-10 Email templates | TC-29, TC-38 | E-01..E-13 | — | F-07.3 |
| SCR-B-11 CSV import | — | — | — | F-02.5 |

For the full cross-role matrix, role × screen access matrix, open questions, and assumptions, see file 5 (`HomeStyle_Admin_Screens_v1.1_05_Shared_Patterns_and_Traceability.md`).

---

*End of file 2 of 5 — Content*
*Companion files: 01_Operations, 03_Trade, 04_SuperAdmin, 05_Shared_Patterns_and_Traceability*
*Read alongside `HomeStyle_BRD_v4.md`, `feature_specs/`, and `references/domain-rules.md`.*
