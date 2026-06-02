# HomeStyle — Product Catalog Reference
**Premium Design Furniture | Domain Reference Document | v1.0**

| Field | Value |
|---|---|
| Version | 1.0 |
| Status | Draft |
| Date | May 2026 |
| Author | Product Owner / BA |
| Classification | Confidential — Internal |
| Cross-references | BRD §17.1, F-02.1, F-02.5, F-02.2, F-02.3 |

> **Purpose of this document:** This is the single source of truth for HomeStyle's product catalog taxonomy — the navigation hierarchy, category-to-attribute (EAV) mapping, collection rules, and admin category management rules. It is a *reference document*, not a feature spec. Feature specs (F-02.x) describe system behavior; this document defines *what the catalog contains and how it is organised*. All BA artifacts, UI specs, and developer implementations should cross-reference this document rather than duplicate its content.

> **Reference model:** West Elm (westelm.com) — a premium home furnishings brand with a comparable product range, design-led navigation, and multi-level catalog taxonomy. HomeStyle's taxonomy is adapted from this model to reflect our specific product range, trade buyer context, and Phase 1/2 roadmap.

---

## Table of Contents

1. [Catalog Architecture Overview](#1-catalog-architecture-overview)
2. [Navigation Taxonomy — L1 to L3](#2-navigation-taxonomy--l1-to-l3)
3. [Cross-Cut Navigation Axes](#3-cross-cut-navigation-axes)
4. [EAV Attribute Matrix by Category](#4-eav-attribute-matrix-by-category)
5. [Collection-to-Category Mapping Rules](#5-collection-to-category-mapping-rules)
6. [Delivery Classification by Category](#6-delivery-classification-by-category)
7. [Admin Category Management Rules](#7-admin-category-management-rules)
8. [Phase 2 Category Expansion Plan](#8-phase-2-category-expansion-plan)
9. [Traceability](#9-traceability)

---

## 1. Catalog Architecture Overview

HomeStyle operates two parallel but distinct hierarchies that must not be confused:

| Hierarchy | Purpose | Defined in |
|---|---|---|
| **Data Model Hierarchy** (5 levels) | How products are structured in the database for pricing, inventory, and configuration. | BRD §17.1 / BR-PRD-01..10 |
| **Navigation Taxonomy** (3 levels + cross-cuts) | How customers and admins discover products through the storefront and admin panel. | **This document** |

These two hierarchies intersect at the **Product (Parent)** level. A product has one `category` field (from the Navigation Taxonomy) which drives its applicable EAV attribute schema (BRD BR-PRD-08) and determines which navigation nodes it appears under. It can additionally belong to multiple **Collections** (BRD BR-PRD-07) which drive the cross-cut navigation axes.

```
Navigation Taxonomy (customer-facing)        Data Model Hierarchy (database)
─────────────────────────────────────        ──────────────────────────────
L1: Department (Room)                        Level 1: Product Family / Collection
  L2: Category                  ←──────→    Level 2: Product (Parent)  ← category field lives here
    L3: Subcategory                          Level 3: Option Types (EAV)
                                             Level 4: Variant / SKU
Cross-cuts: Designer, Material,              Level 5: Bundle / Virtual SKU
            Style, New In, Sale
```

---

## 2. Navigation Taxonomy — L1 to L3

### Navigation Design Principles (from West Elm reference)

West Elm organises its storefront by **customer intent and room context** rather than by product type alone. This is the model HomeStyle adopts:

- L1 (Department) maps to the **room or domain** where the customer will use the product. It appears in the main header navigation.
- L2 (Category) maps to the **furniture type or function** within that room. It drives SSR listing pages (`/collection/[slug]`) and category-level SEO pages.
- L3 (Subcategory) maps to a **specific product form** within a category. It appears as a refinement filter or mega-menu sub-link. Not all L2 categories require an L3; this is at editorial discretion.

---

### 2.1 Phase 1 Navigation Taxonomy

#### L1: Living Room

| L2 Category | Slug | L3 Subcategories | Notes |
|---|---|---|---|
| Sofas & Sectionals | `living-room/sofas-sectionals` | Sofas, Sectionals, Sofa Beds, Loveseats | Highest AOV category — feature prominently |
| Lounge Chairs | `living-room/lounge-chairs` | Armchairs, Accent Chairs, Chaise Lounges, Ottomans & Footstools | Includes occasional chairs and poufs |
| Coffee & Side Tables | `living-room/tables` | Coffee Tables, Side & End Tables, Nesting Tables | — |
| Shelving & Storage | `living-room/shelving-storage` | Bookshelves & Bookcases, Media Units & TV Stands, Sideboards | — |

#### L1: Bedroom

| L2 Category | Slug | L3 Subcategories | Notes |
|---|---|---|---|
| Beds & Headboards | `bedroom/beds-headboards` | Bed Frames, Upholstered Beds, Headboards Only, Daybeds | Size variants: Single, Double, King, Super King |
| Bedside Tables | `bedroom/bedside-tables` | — | — |
| Wardrobes & Armoires | `bedroom/wardrobes` | — | Large format — white-glove delivery only |
| Dressers & Chests | `bedroom/dressers-chests` | — | — |
| Bedroom Chairs | `bedroom/bedroom-chairs` | Accent Chairs, Vanity Stools, Bedroom Benches | — |

#### L1: Dining Room

| L2 Category | Slug | L3 Subcategories | Notes |
|---|---|---|---|
| Dining Tables | `dining/dining-tables` | Fixed Tables, Extendable Tables, Round Tables | — |
| Dining Chairs | `dining/dining-chairs` | Side Chairs, Armchairs, Upholstered Dining Chairs | — |
| Benches | `dining/benches` | — | — |
| Bar & Counter Stools | `dining/stools` | Bar Stools, Counter Stools | — |
| Sideboards & Buffets | `dining/sideboards` | — | — |

#### L1: Home Office

| L2 Category | Slug | L3 Subcategories | Notes |
|---|---|---|---|
| Desks | `home-office/desks` | Writing Desks, Corner Desks, Standing Desks | — |
| Office Chairs | `home-office/office-chairs` | Task Chairs, Occasional Office Chairs | — |
| Shelving & Bookcases | `home-office/shelving` | — | Shared product pool with Living Room Shelving (multi-Collection BR-PRD-07) |
| Filing & Storage | `home-office/storage` | — | — |

#### L1: Outdoor

| L2 Category | Slug | L3 Subcategories | Notes |
|---|---|---|---|
| Outdoor Seating | `outdoor/seating` | Sofas & Sectionals, Lounge Chairs, Benches | Weather-rated materials only |
| Outdoor Dining | `outdoor/dining` | Tables, Chairs, Stools | — |
| Outdoor Lounging | `outdoor/lounging` | Sun Loungers, Daybeds, Hammocks | — |
| Outdoor Storage | `outdoor/storage` | — | — |

---

### 2.2 Phase 1 Navigation — Slug & SEO Naming Convention

| Rule | Detail |
|---|---|
| Slug format | Lowercase, hyphens only, max 60 characters (`/collection/[l1-slug]/[l2-slug]`) |
| Meta title pattern | `[L2 Category Name] — HomeStyle Premium Design Furniture` |
| Meta description | Configurable per collection via Admin CMS (BC-03 / F-02.1 BR-03) |
| L3 subcategory URL | Rendered as a filter param, not a separate route: `/collection/living-room/sofas-sectionals?type=sectionals` |
| Empty category | Zero active products → category hidden from navigation automatically (F-02.1 BR-02) |

---

## 3. Cross-Cut Navigation Axes

Cross-cut axes allow customers to navigate *across* the L1–L3 hierarchy by a shared attribute. These appear as separate header nav entries alongside the room-based departments. Each cross-cut axis is implemented as a **Collection** in the data model (BR-PRD-07), not as a separate category.

| Axis | Nav Label | Slug Prefix | Populated By | Notes |
|---|---|---|---|---|
| By Designer | "By Designer" | `/designer/[designer-slug]` | `designerId` field on Product (Parent) | Each designer has a profile page (BC-05). Designer credit shown on all product cards (F-02.1 BR-07). |
| By Material | "By Material" | `/material/[material-slug]` | Primary material Option Type value on at least one active SKU | Materials with < 3 active products hidden from axis navigation. |
| By Style | "By Style" | `/style/[style-slug]` | Style tag on Product (Parent) — admin-assigned | Styles: Mid-Century Modern, Scandinavian, Contemporary, Industrial, Japandi, Coastal |
| New In | "New In" | `/new-in` | `created_at` within last 90 days, status Active | Auto-populated. Decays off after 90 days without admin intervention. |
| Sale | "Sale" | `/sale` | Any active SKU with `sale_price` set | Auto-populated from pricing data. Trade tier sale prices excluded from public listing. |

---

## 4. EAV Attribute Matrix by Category

This table defines which **Option Types** (EAV attributes — BRD §17.1 Level 3, BR-PRD-08) are applicable per **L2 Category**. The `category` field on a Product (Parent) record determines which Option Types the Configurator Builder (SCR-A-08) presents to Content admins.

> **Mandatory vs Optional:** Mandatory attributes must have at least one valid value defined before a product can be published (status `Draft` → `Active`). Optional attributes may be left empty — they will not appear on the storefront configurator for that product.

### Living Room

| L2 Category | Mandatory Option Types | Optional Option Types | Notes |
|---|---|---|---|
| Sofas & Sectionals | Fabric Grade, Leg Material, Leg Finish, Size (seats / cm width) | Fill Type, Configuration (left/right-hand), Armrest Style | Fabric Grade drives the largest price delta — critical for configurator |
| Lounge Chairs | Material, Finish, Base Type, Size (W×D×H cm) | Fabric Grade, Armrests (Y/N), Seat Cushion Fill | Base Type: 4-star swivel, sled, solid wood, hairpin |
| Coffee & Side Tables | Material, Finish, Size (W×D×H cm) | Shape (round / rectangular / oval), Glass Top (Y/N) | — |
| Shelving & Storage | Material, Finish, Size (W×H×D cm) | Number of Shelves, Doors (Y/N), Cable Management (Y/N) | — |

### Bedroom

| L2 Category | Mandatory Option Types | Optional Option Types | Notes |
|---|---|---|---|
| Beds & Headboards | Material, Finish, Bed Size (Single / Double / King / Super King) | Headboard Height (Low / Mid / Tall), Storage (None / Ottoman / Drawers), Upholstery Grade | Bed Size always mandatory — drives SKU matrix |
| Bedside Tables | Material, Finish, Size (W×D×H cm) | Number of Drawers, Leg Style | — |
| Wardrobes & Armoires | Material, Finish, Size (W×H×D cm) | Door Type (hinged / sliding), Interior Configuration | Large-format only: white-glove delivery mandatory |
| Dressers & Chests | Material, Finish, Size (W×H×D cm) | Number of Drawers | — |
| Bedroom Chairs | Material, Finish, Fabric Grade | Size (W×D×H cm) | Shares option schema with Lounge Chairs where applicable |

### Dining Room

| L2 Category | Mandatory Option Types | Optional Option Types | Notes |
|---|---|---|---|
| Dining Tables | Material, Finish, Size (seats / cm length), Shape | Extension Type (none / butterfly / self-storing leaf) | Extension Type mandatory only when Extendable = Yes |
| Dining Chairs | Material, Finish, Fabric Grade | Armrests (Y/N), Stackable (Y/N) | Fabric Grade mandatory only for upholstered chairs |
| Benches | Material, Finish, Size (cm length) | Upholstery Grade | — |
| Bar & Counter Stools | Material, Finish, Seat Height (Bar / Counter), Fabric Grade | Footrest (Y/N), Swivel (Y/N), Back Style (with/without) | — |
| Sideboards & Buffets | Material, Finish, Size (W×H×D cm) | Number of Doors, Number of Drawers | — |

### Home Office

| L2 Category | Mandatory Option Types | Optional Option Types | Notes |
|---|---|---|---|
| Desks | Material, Finish, Size (W×D×H cm) | Cable Management (Y/N), Drawers (Y/N), Return / L-Shape (Y/N) | — |
| Office Chairs | Material, Fabric Grade | Armrests (Y/N), Height-Adjustable (Y/N), Lumbar Support (Y/N) | — |
| Shelving & Bookcases | Material, Finish, Size (W×H×D cm) | Number of Shelves, Back Panel (Y/N) | — |
| Filing & Storage | Material, Finish, Size (W×H×D cm) | Locking (Y/N) | — |

### Outdoor

| L2 Category | Mandatory Option Types | Optional Option Types | Notes |
|---|---|---|---|
| Outdoor Seating | Material, Finish, Weather Rating (IP class), Size | Cushion Grade, Configuration (left/right) | Weather Rating mandatory — drives logistics and warranty scope |
| Outdoor Dining | Material, Finish, Size, Shape | Extension Type, Stackable Chairs (Y/N) | — |
| Outdoor Lounging | Material, Finish, Weather Rating | Cushion Grade, Recline Positions | — |
| Outdoor Storage | Material, Finish, Size (W×H×D cm) | Locking (Y/N), Weatherproof Seal (Y/N) | — |

---

### 4.1 Shared Option Type Definitions

These Option Type values are shared across categories. Content admins select from this master list; category-specific values can be added by Super Admin only (BRD BC-67 / F-02.5 endpoint `POST /api/v1/admin/catalogue/option-types`).

| Option Type | Sample Valid Values | Notes |
|---|---|---|
| Material | Solid Walnut, Solid Oak, Solid Ash, Marble (Carrara), Marble (Nero), Travertine, Lacquered Steel, Brushed Brass, Rattan, Teak, Powder-Coated Aluminium | Values are marketing-facing (not supplier codes) |
| Finish | Natural Oil, Wax, Matt Lacquer, Gloss Lacquer, Smoked, Ebonised, White, Black, Brushed, Patinated | Applied on top of Material where relevant |
| Fabric Grade | Grade A, Grade B, Grade C, Grade D | Grade drives price uplift. Grade A = entry; Grade D = premium leather / boucle |
| Base Type | 4-Star Swivel, Sled Base, Solid Wood Block, Hairpin Legs, Tapered Legs, Platform Base, Pedestal | — |
| Bed Size | Single (90×200 cm), Double (135×190 cm), King (150×200 cm), Super King (180×200 cm), EU Single (90×200 cm), EU King (160×200 cm) | EU sizes apply for EU market SKUs |
| Shape | Round, Oval, Rectangular, Square | Primarily for tables |
| Weather Rating | IP44, IP55, IP65 | Outdoor products only |
| Seat Height | Bar (73–79 cm), Counter (61–66 cm) | Stools only |

---

## 5. Collection-to-Category Mapping Rules

Collections are the data model entity that drives navigation (BR-PRD-07). One product can belong to multiple Collections simultaneously. This section defines the authoritative rules for how categories map to collections.

### 5.1 Primary Collection Rule

Every product must be assigned to at least one **room-based Collection** matching its L2 category. This is the product's *canonical* collection — the destination of 301 redirects when the product is archived (BR-PRD-10).

| L2 Category | Canonical Room Collection |
|---|---|
| Sofas & Sectionals | Living Room |
| Lounge Chairs | Living Room |
| Coffee & Side Tables | Living Room |
| Shelving & Storage | Living Room |
| Beds & Headboards | Bedroom |
| Bedside Tables | Bedroom |
| Wardrobes & Armoires | Bedroom |
| Dressers & Chests | Bedroom |
| Bedroom Chairs | Bedroom |
| Dining Tables | Dining Room |
| Dining Chairs | Dining Room |
| Benches | Dining Room |
| Bar & Counter Stools | Dining Room |
| Sideboards & Buffets | Dining Room |
| Desks | Home Office |
| Office Chairs | Home Office |
| Shelving & Bookcases | Home Office |
| Filing & Storage | Home Office |
| Outdoor Seating | Outdoor |
| Outdoor Dining | Outdoor |
| Outdoor Lounging | Outdoor |
| Outdoor Storage | Outdoor |

### 5.2 Additional Collection Assignments

A product may additionally belong to any number of the following Collections without limit (BR-PRD-07):

| Collection Type | Example | Assigned By |
|---|---|---|
| Designer Collection | "Aria by Marco Vitti" | Content Admin — requires `designerId` set on product |
| Style Collection | "Japandi Edit", "Mid-Century Modern" | Content Admin — style tag |
| Material Collection | "Solid Walnut Edit", "Marble & Stone" | Auto-derived from primary material Option Type of at least one published SKU |
| Curated Edit | "Summer Living Edit", "Home Office Refresh" | Content Admin — seasonal / editorial |
| Cross-Room | "Shelving & Storage" appearing in both Living Room and Home Office | Dual canonical assignment (exception — requires Super Admin approval) |

### 5.3 Cross-Room Products

Some product types (e.g., bookshelves, benches, accent chairs) reasonably belong to multiple room collections. Rule: Content Admin may add a second room Collection; a **third room Collection requires Super Admin approval** to prevent catalog dilution.

---

## 6. Delivery Classification by Category

Delivery method affects carrier integration, checkout flow, and lead time display. This classification is determined by category, not entered per-SKU. The SKU `gross_weight_kg` and `L×W×H` fields (mandatory per BR-PRD-04) allow the system to confirm classification and rate-quote via the carrier API (BC-51).

| Delivery Class | Trigger | Carrier | Lead Time | White-Glove |
|---|---|---|---|---|
| **White-Glove Large** | Any SKU with longest dimension > 120 cm OR gross weight > 30 kg | Specialist furniture carrier | 5–15 business days (standard); per product lead time for MTO | Yes — room of choice, packaging removal, basic assembly |
| **Standard Parcel** | All dimensions ≤ 120 cm AND gross weight ≤ 15 kg | FedEx / UPS (US); DHL / DPD (EU) | 3–7 business days | No |
| **Made-to-Order (MTO)** | `lead_time_weeks` set on SKU AND `is_mto = true` | White-Glove Large carrier | Per SKU lead time (displayed on PDP) | Yes |

**Category defaults (Phase 1):**

| L2 Category | Default Delivery Class |
|---|---|
| Sofas & Sectionals | White-Glove Large |
| Lounge Chairs | White-Glove Large |
| Coffee & Side Tables | Standard Parcel (most) / White-Glove if > 120 cm |
| Shelving & Storage | White-Glove Large |
| Beds & Headboards | White-Glove Large |
| Bedside Tables | Standard Parcel |
| Wardrobes & Armoires | White-Glove Large |
| Dressers & Chests | White-Glove Large |
| Bedroom Chairs | White-Glove Large |
| Dining Tables | White-Glove Large |
| Dining Chairs | Standard Parcel (most) / White-Glove if upholstered + heavy |
| Benches | Standard Parcel / White-Glove if > 30 kg |
| Bar & Counter Stools | Standard Parcel |
| Sideboards & Buffets | White-Glove Large |
| Desks | White-Glove Large (most) / Standard Parcel for small writing desks |
| Office Chairs | Standard Parcel |
| Shelving & Bookcases | White-Glove Large |
| Outdoor Seating | White-Glove Large |
| Outdoor Dining | White-Glove Large |
| Outdoor Lounging | White-Glove Large |

> Note: The system validates actual dimensions/weight at SKU creation and overrides the category default if thresholds are crossed. Default classification is a content authoring guide only.

---

## 7. Admin Category Management Rules

These rules govern how Content and Super Admin users manage the taxonomy itself (not individual products).

| Rule ID | Rule | Role |
|---|---|---|
| CAT-01 | L1 Departments are **fixed** in Phase 1. New departments (e.g., "Kids", "Pets") require a product roadmap decision and Super Admin action. | Super Admin only |
| CAT-02 | L2 Categories can be **added** by Super Admin. Adding a category requires: a unique slug, a canonical room Collection assignment, and an EAV attribute schema definition before any products can be assigned. | Super Admin only |
| CAT-03 | L3 Subcategories can be **added or retired** by Content Admin. They are implemented as filter tags, not separate routes — no slug is required. | Content Admin |
| CAT-04 | A category **cannot be deleted** if it contains active or archived products. It must first be emptied (products reassigned or permanently erased per GDPR). | Super Admin only |
| CAT-05 | Category **renaming** updates the display label only. Slug is immutable post-creation — changes require a 301 redirect setup and SEO review. | Super Admin only |
| CAT-06 | EAV Option Types can be **added** to a category by Super Admin at any time without schema migration (BR-PRD-08). Existing products in that category do not automatically gain new option values — Content Admin must update each product. | Super Admin only |
| CAT-07 | EAV Option Type **values** (e.g., adding "Grade E" to Fabric Grade) can be added by Content Admin. Removing a value requires confirmation if any active SKUs use it — system shows a count of affected SKUs before confirming. | Content Admin (add) / Super Admin (remove) |
| CAT-08 | Collections used for cross-cut navigation (By Designer, By Material, By Style) are **auto-managed** by the system from product data — they cannot be manually sorted in Phase 1. Editorial sorting is a Phase 2 CMS feature. | System-managed |
| CAT-09 | The **"New In"** collection is auto-populated by `created_at` date. Products older than 90 days are automatically removed. No manual override in Phase 1. | System-managed |
| CAT-10 | All taxonomy changes (category add/rename, EAV schema changes) are **audit-logged** with admin ID, timestamp, and before/after diff (extends BC-40, F-02.5 BR-12). | System |

---

## 8. Phase 2 Category Expansion Plan

The EAV model (BR-PRD-08) is specifically designed to accommodate these Phase 2 categories without schema changes. This table documents the planned expansion for roadmap planning purposes.

| L1 Department | L2 Category | Key New EAV Attributes | Notes |
|---|---|---|---|
| Lighting | Pendants | Wattage, Bulb Type, Cable Length (cm), Fitting (E27/B22/GU10), IP Rating, Shade Material | Referenced in BRD BC-67 as the primary Phase 2 EAV test case |
| Lighting | Floor Lamps | Wattage, Bulb Type, Shade Material, Switch Type | — |
| Lighting | Table Lamps | Wattage, Bulb Type, Shade Material, Base Material | — |
| Lighting | Wall Lights | Wattage, Bulb Type, IP Rating | IP Rating mandatory for bathroom-rated fittings |
| Accessories & Decor | Cushions & Throws | Material, Fill (cushion), Size, Fabric Grade | — |
| Accessories & Decor | Mirrors | Frame Material, Frame Finish, Shape, Size (W×H cm), Hanging Orientation | — |
| Accessories & Decor | Rugs | Material, Construction (hand-tufted / flat-weave / shaggy), Pile Height, Size | — |
| Accessories & Decor | Vases & Objects | Material, Finish, Size (H cm) | Low AOV — parcel delivery default |

> Phase 2 categories will require: new Collection records, new L2 category configuration in admin, EAV attribute schema setup by Super Admin, and content team onboarding. No code changes to the catalog engine are required per BR-PRD-08.

---

## 9. Traceability

| This Document Section | BRD Reference | Feature Spec Reference | Admin UI Reference |
|---|---|---|---|
| §1 — Architecture overview | §17.1 | F-02.5 (data model) | — |
| §2 — Navigation taxonomy | BC-01, BC-05 | F-02.1 (Browse by Room), F-02.2 (Search & Filters) | SCR-A-07 (Product Editor) |
| §3 — Cross-cut axes | BC-01, BC-05, BR-PRD-07 | F-02.1, F-02.2 | Admin CMS Collections |
| §4 — EAV attribute matrix | §17.1 Level 3, BR-PRD-08, BC-67 | F-02.5 (Configurator Builder), F-02.3 (PDP) | SCR-A-08 (Configurator Builder) |
| §5 — Collection mapping rules | BR-PRD-07, BR-PRD-10 | F-02.5 (archive redirect rule) | SCR-A-07 |
| §6 — Delivery classification | §17.7 (DIM Weight), BC-08, BC-51 | F-04.1 (Checkout — delivery selection) | SCR-A-01 (Shipping config) |
| §7 — Admin category rules | BR-PRD-08, BC-40, BC-67 | F-02.5 (option-types endpoint), F-08.x (audit log) | SCR-A-07, SCR-A-08 |
| §8 — Phase 2 expansion | BC-67, §7 (Phase 2 roadmap) | — | — |

---

*Document owner: Product Owner / BA. Review cycle: updated when new categories are approved for development. Changes follow standard Change Request process (BRD §Change Control).*
