# Business Use Case

### Brand

**Nimbus Outfitters** — a mid-market retailer selling apparel and accessories online and in-store.

### Goals

*   Unify customer identities from **Salesforce CRM** (Sales/Service data).
*   Power **Copilot** with **policy** and **competitive pricing** documents stored in **SharePoint** for accurate, compliant responses.
*   Create segments from CRM purchase and service history; activate campaigns; measure with Insights.
*   Keep architecture **standard-first**: standard objects and DMOs wherever possible.

### Systems & Sources

*   **Salesforce CRM** (Sales/Service Cloud): `Account`, `Contact`, `Case`, `Product2`, `Pricebook2`, `PricebookEntry`, `Order`, `OrderItem`.
*   **SharePoint** (documents only):
    *   `policy_docs` (returns, warranty, shipping, privacy, price-match).
    *   `competitive_pricing` (competitor price sheets, exceptions).

### Data Cloud Architecture

*   **Data Streams**: Salesforce CRM (structured), SharePoint libraries (unstructured docs).
*   **DSO/DLO**: auto-created for CRM sources.
*   **DMO (Standard)**: `Individual`, `UnifiedIndividual`, `ContactPointEmail`, `ContactPointPhone`, `Address`, `Account`, `Product`, `Order`, `OrderItem`, `Case`.
*   **Custom DMOs**: *Not required*. (Optional: `CompetitivePrice__mdm` only if you want **structured** competitor price analytics derived from a CSV you might add later; not used in this version.)

### Data Spaces

*   **Retail** (Sales/Marketing scope).
*   **Service** (Support/Case scope).

### Outcomes

*   **Unified Profiles** across CRM duplicates.
*   **Insights**: LTV, order recency, open cases.
*   **Segments**: High-value, recent purchasers, service-sensitive.
*   **Copilot**: Answers using **Data Graph** + **SharePoint policies/pricing docs**, citing policy versions/dates.

***

# Dummy Data

Use these CSVs to seed your CRM or to emulate CRM-origin DLOs in the Data Cloud stream (standard objects only).

## 1. CRM Core (Sales/Service Cloud)

> **Account.csv**

```csv
Name,Type,BillingCountry
Nimbus Retail Store,Customer,US
Nimbus Online,Partner,US
Acme Corporate,Customer,US
```

> **Contact.csv**

```csv
FirstName,LastName,Email,Phone,MobilePhone,HasOptedOutOfEmail,DoNotCall,MailingStreet,MailingCity,MailingState,MailingPostalCode,MailingCountry
Ava,Lee,ava.lee@example.com,+1-415-555-0101,+1-415-555-1101,false,false,10 Pine St,San Francisco,CA,94102,US
Noah,Patel,noah.patel@example.com,+1-650-555-0102,+1-650-555-1102,true,false,25 Oak Ave,San Mateo,CA,94401,US
Lucas,Wong,lucas.wong@example.com,+1-408-555-0103,+1-408-555-1103,false,true,80 Elm Rd,San Jose,CA,95112,US
Mia,Gonzalez,mia.gonzalez@example.com,+1-510-555-0104,+1-510-555-1104,false,false,55 Maple Ln,Oakland,CA,94607,US
```

> **Case.csv**

```csv
ContactId,Subject,Status,Origin,Priority,CreatedDate
Return request for jacket,New,Web,Medium,2025-12-20T10:00:00Z
Size exchange inquiry,Working,Phone,Low,2025-12-19T09:00:00Z
Payment issue,New,Email,High,2025-12-22T14:30:00Z
```

> **Product2.csv**

```csv
Name,ProductCode,IsActive,Family
Classic Denim Jacket,CDJ-001,true,Apparel
Trail Running Shoes,TRS-042,true,Footwear
Everyday Tee,EVT-011,true,Apparel
```

> **Order.csv**

```csv
Id,AccountId,EffectiveDate,Status,TotalAmount,Description
801A000001,001A000001,2025-10-15,Activated,199.00,Store purchase
801A000002,001A000002,2025-11-05,Activated,79.00,Online order
801A000003,001A000003,2025-12-01,Activated,120.00,Shipped
```

> **OrderItem.csv**

```csv
Id,OrderId,PricebookEntryId,Quantity,UnitPrice
802A000001,801A000001,01uA000002,1,120.00
802A000002,801A000001,01uA000003,3,25.00
802A000003,801A000002,01uA000001,1,79.00
```

## 2. SharePoint (Documents Only)

> **policy\_docs/** (PDFs; examples)

    returns_policy_v3.pdf
    shipping_policy_2025.pdf
    warranty_policy_apparel_footwear.pdf
    privacy_notice_v2.pdf
    price_match_policy_v1.pdf

**Example snippets** (for testing retrieval):

*   `returns_policy_v3.pdf`: “Returns accepted within **30 days** of delivery. Items must be unworn with tags. Receipt or order number required.”
*   `price_match_policy_v1.pdf`: “We match qualifying competitor prices at purchase or within **7 days**. Excludes flash sales, clearance, bundles.”

> **competitive\_pricing/** (PDFs or internal spreadsheets exported to PDF)

    competitor_prices_Q4_2025.pdf
    price_match_exceptions_2025.pdf

*(No structured CSV ingestion from SharePoint in this version.)*

***

# Hands‑On Labs (Concise)

> Steps are purposefully brief. Use **standard DMOs** wherever possible.

## Module 1

1.  **Data Cloud Setup**

*   Enable Data Cloud.
*   Create **Data Spaces**: `Retail`, `Service`.
*   Register **Data Sources**: Salesforce CRM; SharePoint (libraries: `policy_docs`, `competitive_pricing`).

2.  **Licenses and Permission Sets**

*   Assign **Data Cloud Admin/User**.
*   Grant **Data Space** access.

3.  **Data Streams**

*   **Create CRM Data Stream** (Retail): `Account`, `Contact`, `Case`, `Product2`, `Order`, `OrderItem`.
*   **Register SharePoint Libraries** (Retail): `policy_docs`, `competitive_pricing`.

4.  **DSO, DLO, DMO \[Standard and Custom]**

*   Verify CRM **DSO/DLO** auto-created.
*   Use **Standard DMOs**: `Individual`, `ContactPointEmail`, `ContactPointPhone`, `Address`, `Account`, `Product`, `Order`, `OrderItem`, `Case`.
*   *(No custom DMOs required for policies/pricing docs; they are unstructured and used for retrieval.)*

5.  **Data Spaces**

*   Scope CRM streams to `Retail` (and `Service` for Cases if needed).
*   Validate isolation and role-based access.

6.  **Data Explorer and Query Editor**

*   Inspect counts for Contacts, Orders, Cases.
*   Query: “Top 5 products by revenue,” “Open cases by priority,” “Orders in last 30 days.”

7.  **Data Ingestion: SharePoint**

*   Connect SharePoint site/tenant; grant library permissions.
*   Ingest & index **`policy_docs`** and **`competitive_pricing`** libraries for Module 3 (RAG).

8.  **Data Mapping**

*   **Map DLO → DMO**:
    *   `Contact` → `Individual`, `ContactPointEmail`, `ContactPointPhone`, `Address`.
    *   `Account` → `Account`.
    *   `Product2` → `Product`.
    *   `Order` / `OrderItem` → `Order` / `OrderItem`.
    *   `Case` → `Case`.
*   *(No DMO mapping for SharePoint documents; they are indexed, not modeled.)*

***

## Module 2

1.  **Identity Resolutions**

*   Enable Identity Resolution for `Individual`.

2.  **Matching Rules**

*   Create: **Email exact**.
*   Create: **Phone exact + Postal exact**.
*   Create: **Name + Address fuzzy**.

3.  **Unified Individual & Links**

*   Run Identity Resolution.
*   Review `UnifiedIndividual` profiles & contributing source links.

4.  **Identity Resolution Rule Set**

*   Combine rules; set priority; schedule incremental runs.

5.  **Consolidation Rate**

*   Set threshold (e.g., 0.85 → 0.90); compare merges; document over/under-merge tradeoffs.

6.  **Reconciliation Rules**

*   Survivorship: Prefer CRM for Name; most recent verified for Email/Phone; highest confidence for Address.

7.  **Data Transformation**

*   Normalize emails (lowercase/trim).
*   Standardize phone formats (E.164).
*   Derive: `days_since_last_order`, `total_orders`, `avg_order_value`.

8.  **Calculation Insights & Streaming Insights**

*   **Calculation Insight**:
    *   `lifetime_value` = sum(`Order.TotalAmount`) by Individual.
    *   `30_day_order_count`.
*   **Streaming Insight** (CRM event-driven):
    *   `recent_case_opened` if `Case.CreatedDate` within last 24h.
    *   `recent_order_placed` if `Order.EffectiveDate` within last 7d.

9.  **Segments**

*   **High-Value Loyal**: `lifetime_value > 250` AND `30_day_order_count >= 1`.
*   **Service Sensitive**: `recent_case_opened = true` OR `Priority = High`.
*   **Win-Back**: `days_since_last_order > 60` AND `total_orders >= 1`.

10. **Data Action \[Data Action Targets, Activation Targets]**

*   Create **Activation Target**: Marketing Cloud (Email/SMS audience).
*   Create **Data Action Target**: Salesforce (create Task/Case) or webhook.
*   **Activate Segments** with identifiers (email/phone) + attributes (LTV, last order date).

11. **Data Kit: Data Cloud Deployment**

*   Create **Data Kit** including Streams, Mappings, Identity Rules, Transformations, Insights, Segments.
*   Deploy to higher environments and validate.

***

## Module 3

1.  **Chunking**

*   Register **SharePoint** libraries: `policy_docs`, `competitive_pricing`.
*   Define chunking: semantic chunks (\~800 tokens, 10–15% overlap), strip headers/footers, preserve tables.
*   Build embeddings and index.

2.  **Retrievers**

*   **Documents Retriever**: over `policy_docs` + `competitive_pricing`.
*   **Data Graph Retriever**: `UnifiedIndividual` → `Orders` → `OrderItems` → `Product` (+ `Case`). Expose: last order date, items, LTV, open cases, contact points.

3.  **Hybrid Queries**

*   Enable hybrid (BM25 + vector).
*   Route policy/price questions → document retriever; order/account questions → data graph retriever; combine when needed (e.g., “Is this customer eligible for price match on TRS‑042?”).

4.  **Ranking Models**

*   Apply semantic reranker (top‑k 20 → rerank top‑5).
*   A/B test: keyword-only vs hybrid+rerank on policy edge cases (exclusions, time windows).

5.  **Intelligent Context**

*   Copilot Skill: **“Policy & Price Assist”**
    *   **Context (Data Cloud)**: `segment_membership`, `lifetime_value`, `last_order`, `open_case_count`, `contact emails/phones`.
    *   **Grounding**: Policies & pricing from SharePoint docs; customer state from Data Graph.
    *   **Guardrails**: Respect `HasOptedOutOfEmail` / `DoNotCall` for suggested outreach; cite policy doc name/version/date in all answers.

***

## Mapping Cheatsheet (Final)

| Source DLO               | Target DMO        | Key Field Mappings                                                     |
| ------------------------ | ----------------- | ---------------------------------------------------------------------- |
| Contact (CRM)            | Individual        | Email → PrimaryEmail, Phone/Mobile → PrimaryPhone, Mailing\* → Address |
| Contact (CRM)            | ContactPointEmail | Contact.Email → Address, IsPrimary = true                              |
| Contact (CRM)            | ContactPointPhone | Contact.MobilePhone (pref.) or Phone → Number                          |
| Account (CRM)            | Account           | Name, Type, Country                                                    |
| Product2 (CRM)           | Product           | Name, ProductCode, Family                                              |
| Order/OrderItem          | Order/OrderItem   | Header totals/dates; items: ProductCode, Quantity, UnitPrice           |
| Case (CRM)               | Case              | Subject, Status, Origin, Priority, CreatedDate                         |
| **policy\_docs**         | *Doc Retriever*   | *Indexed for retrieval; not mapped to DMOs*                            |
| **competitive\_pricing** | *Doc Retriever*   | *Indexed for retrieval; not mapped to DMOs*                            |

***
