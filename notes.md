
<center><img width='200px' src='./assets/data_360_logo.png'/></center>

<h1 align='center'>Salesforce Data 360</h1>

<br>

# Module 1 — Data Cloud / Data 360: Concepts & Architecture (FOUNDATION)

**Learning objectives**

* Understand what Salesforce Data Cloud (Data 360) is and its business value (CDP concepts). 
* Know the core architectural pieces (DLO, DMO, unified profiles / identity graph, activation destinations) and how they connect.
* Recognize rebranding notes (Data Cloud → *Data 360*) and where to look for updated docs.

## 1. What is Data Cloud / Data 360 — high level & business value

* **Definition (one line):** Data 360 is Salesforce’s customer data platform (CDP) that unifies, harmonizes, and makes customer data actionable across the Salesforce stack. 
* **Business value:** single source of truth for customer data, faster personalization, real-time segmentation, safer governance of PII, and simplified activation to marketing/sales/service systems.
* **When to use:** cross-cloud personalization, joined analytics across systems, powering Journeys/agents with unified customer context, or when you want “one view” without losing source fidelity.

## 2. Core architectural elements (simple definitions)

<center><img src='./assets/DataCloud_ArchDiagram052025.png'/></center>

* **Data Stream** — connector that brings data into Data 360 (or points to external data for zero-copy).
* **Data Lake Object (DLO)** — raw, ingested table(s). Think of DLOs as the “raw dump” from each source (each DLO = one table). Use DLOs when you need the original, source-shaped records. 
* **Data Model Object (DMO)** — a curated, business-facing view (a mapped/standardized model). DMOs are created by mapping fields from one or more DLOs into a stable schema used by downstream processes (segmentation, activation). DMOs are like “views” or the prepared dataset.
* **Unified Profiles / Identity Graph** — identity resolution rulesets that link multiple source profiles into a single *Unified Individual* (single customer record used for segmentation & activation).
* **Calculated Insights** — derived/calculated metrics or signals (e.g., lifetime value) used in segmentation.
* **Activation destinations** — where segments or unified data are sent (Marketing Cloud Journeys, CRM updates, ad platforms, files, streaming endpoints). Activation can be streaming (near-real-time) or batch. 

## 3. Zero-copy (data federation) — short explanation

* **What it is:** ability to query and use external data without physically copying it into Data 360 (federation / live query). Good when you must avoid duplication or keep very large datasets in place. 
* **When to choose zero-copy:** very large tables, regulatory/PII constraints, or when you need the freshest record without ETL. Consider performance and mapping needs — sometimes a physical copy (ingest) is still preferable.

## 4. How the pieces connect (one-paragraph flow)

1. **Ingest or federate** data via Data Streams into DLOs (or reference external tables via zero-copy).
2. **Map DLO fields → DMO fields** to create harmonized, business-friendly objects (Customer, Transaction, Event).
3. **Create identity resolution rulesets** and unify to produce Unified Individuals (identity graph). 
4. **Build real-time segments** (audiences) using DMOs & calculated insights.
5. **Activate** segments to destinations (streaming or batch) for Journeys, ads, CRM updates, or analytics. 

## 5. Quick tips

* **Mapping is contract work:** DMOs are the contract consumers use — think carefully about stable field names and types. Changing a DMO field type later can be disruptive. ([Salesforce][9])
* **Identity rules matter:** loose rules → over-merging; strict rules → duplicate unified profiles. Start conservative, test thoroughly. 
* **Zero-copy tradeoffs:** great for governance and freshness, but evaluate query latency and mapping complexity before choosing. 
* **Activation mode:** use streaming for near-real-time needs (agentic experiences / immediate journeys), batch for large periodic exports. 

* Data 360 architecture overview — Salesforce Architect docs. 
* Data Model Objects & mapping guide — Salesforce Help / Developer docs. 
* Trailhead module: Get to know Unified Profiles (hands-on identity examples). 
* Zero-copy overview & guides (when to use, partner integrations).

---

<br>

# Module 2 — DLOs & DMOs — Modeling fundamentals (6 hours: lecture + lab)

**Prereq:** Module 1 — Data Cloud / Data 360 concepts.

**Learning objectives:** Know what DLOs and DMOs are; map raw sources to a harmonized model; choose field types & constraints; apply normalization and enrichment; build a DLO → DMO mapping and validate types.

## 1. Quick definitions

| Object | Abbreviation | Definition | Creation and Usage |
| :--- | :--- | :--- | :--- |
| **Data Lake Object** | DLO | A container for the **structured data** brought into Data Cloud. | Automatically created from data streams, but can also be created manually. A DLO is **mapped to a DMO**. |
| **Unstructured Data Lake Object** | UDLO | A container for **unstructured data** (e.g., documents, media files) referenced by Data Cloud. | Created manually. A UDLO is **automatically mapped to an existing or new UDMO**. |
| **Data Model Object** | DMO | A **harmonized grouping of structured data** created from data streams, insights, and other sources. It represents the standardized Customer 360 data model. | A DLO is **mapped to a DMO** to ensure consistency and standardization. |
| **Unstructured Data Model Object**| UDMO | A group of **unstructured data** created from an unstructured data source. | A UDLO is **automatically mapped** to an existing or new UDMO. |
| **External Data Lake Object** | External DLO | A storage container with **metadata** for data **federated** (referenced, not copied) from an **external data source** (e.g., Snowflake, AWS S3). | Acts as a **reference**, pointing to the data physically stored in an external source. |

## 2. DLO

* Common DLO sources: Sales Cloud exports, event streams (e-commerce), POS CSVs, external warehouses (S3/GCS). You can also reference unstructured DLOs for files (UDLOs). 
* DLO properties: schema mirrors source; fields are created at ingest; changing DLO field types after creation is restricted—plan carefully.


## 3. DMO

* Types: standard DMOs (prebuilt like Contact, Account) and custom DMOs (create when your business needs a specific harmonized view). Create custom DMOs when you need fields or semantics not provided by standard DMOs.
* Behavior: a DMO can draw from one or multiple DLOs; mapping defines how source fields become DMO fields and how relationships are modeled.

## 4. Mapping rules & data types — practical rules

* **Compatibility:** DLO → DMO field data types must be compatible; once set, types are hard to change — decide types carefully (strings vs numeric vs date).
* **Primary keys & required mappings:** Profile/Other DMOs often require you to map the primary key; some DMOs require specific system fields to be mapped for identity or activation to work. Validate required mappings before activation.
* **Auto-mapping:** Data Cloud offers auto-mapping helpers (model card) — useful for initial mapping but always review and adjust for business rules.

## 5. Normalization & enrichment — where and how

* **Normalization (when to do it):** fix inconsistent formats (phone, email, currency), flatten nested JSON, split composite fields into atomic fields — typically done between DLO ingestion and DMO mapping (use interim DLOs or transforms).
* **Enrichment (when to do it):** add derived fields (loyalty_score, lifetime_value), lookup references (geolocation), or external enrichments (third-party demographics) — usually created as calculated insights or written back to DMOs/DLOs as needed.
## 6. Quick tips

* **Plan types first:** once DLO/DMO fields are created, changing types is hard — design schemas thoughtfully.
* **Interim DLOs are your friend:** use them to clean noisy data before mapping into DMOs, especially for contact-point fields.
* **Map only what you need:** avoid ingesting unnecessary columns — smaller DLOs are cheaper and faster.
* **Document mapping decisions:** capture why you chose aggregations (SUM vs MAX), primary keys, and transforms — prevents later regressions.

## Lab — Data Mapping (DLO to DMO)

Mapping Data from `Contact_Home` (Data Lake Object) to `Employee` (Data Model Object). 

### 1. Create Data Stream (Source Connection) 🔗

- In Data Cloud, go to `Data Streams` $\to$ New.
- Select `Salesforce CRM` from Connected Sources.
- Click on `View Objects` $\to$ Select `Contact_Home` Object.
- Object Category $\to$ `Other`.
- Select the required fields: `Contact ID, FirstName, LastName, Email`.
- Click Next and then click Deploy.
- Data Lake Object will be created automatically.

### 2. Create Data Model Object

- Go to Data Model Objects in Data Cloud $\to$ Click New
- Add Fields: `Associate ID (Primary Key), First Name, Last Name, Email`.
- Save and Activate DMO.

### 3. Map Contact (DLO) to Associate (DMO)

- Go to `Data Streams` $\to$ Open the `Contact_Home` Data Stream.
- In data map section click on `Start`.
- Click on `Select Objects` in data model object section $\to$ Select `Associate` Object.
- Map the fields from DLO to DMO.
- Click Save and close.

| Source DLO Field (Contact_Home) | Target DMO Field (Associate) |
|---|---|
**Contact ID** |	Map to **Associate ID**
**FirstName** |	Map to **First Name**
**LastName** |	Map to **Last Name**
**Email** |	Map to Work **Email**

---

# Module 3 — Data Ingestion & Streaming (BATCH & REAL-TIME)

## 1. What is a Connector? (Overview)

A **connector** is an integration component that brings data into Data Cloud (Salesforce Data 360) or moves audiences/data out (activation). Connectors can be **streaming** (near real-time) or **batch** (scheduled), and they handle authentication, schema mapping, transport, error reporting and basic transformations.

## 2. Salesforce Integrations

These connectors are used when the source/destination is a Salesforce product or a Salesforce-hosted service.

### Salesforce CRM

**Purpose:** Sync records (Leads, Contacts, Accounts, Opportunities, custom objects) between Sales/Service Clouds and Data Cloud for a single source of customer truth.

* **Typical data:** contact fields, activities, opportunity amounts, custom object rows.
* **Auth:** OAuth 2.0 (connected app), often with certificate or JWT for automation.
* **Frequency:** CDC (Change Data Capture) for near-real-time; scheduled full syncs for backfills.
* **Key tasks:** map objects → identity resolution (match contacts/accounts) → incremental ingestion.
* **Best practices:** use CDC to avoid full extracts; limit payload to necessary fields; add lastModified timestamps.
* **Pitfalls:** schema changes in CRM break mapping; large historic backfills require staging.

### Cloud One

*(If used in your org as a named connector)*
**Purpose:** Internal connector for moving processed profile/audience data between Data Cloud environments or managed services.

* **Use cases:** moving audiences between sandboxes and production or integrating managed Data Cloud services.
* **Notes:** check org-level permissions and data sharing settings.

### Marketing (Marketing Cloud)

**Purpose:** Activate segments, audiences, and attributes to Marketing Cloud channels (email, journeys).

* **Data exchanged:** audiences, profile attributes, suppression lists, event data.
* **Auth:** OAuth 2.0 or API keys depending on Marketing Cloud setup.
* **Pattern:** Data Cloud builds audiences; Marketing connector pushes them as synchronized data extensions or via API for campaigns.
* **Best practices:** push minimal audience attributes required for personalization; use incremental audience syncs.

### Commerce Cloud

**Purpose:** Exchange customer activity, order history, product interactions and personalize commerce experiences.

* **Typical flows:** send product views and order data to Data Cloud; retrieve personalized recommendations to Commerce.
* **Considerations:** align product SKUs and customer ids; respect product catalog schema.

## 3. External Integrations

Connectors to non-Salesforce systems: activation platforms, data warehouses, apps.

### External Activation Platforms

**Purpose:** Send audiences/profiles to ad networks, personalization engines, CDP partners, DSPs.

* **Examples:** ad platforms, personalization engines, third-party CDPs.
* **Transport:** API push (HTTP), SFTP drop, or partner-specific connectors.
* **Latency:** typically near-real-time to daily depending on platform.
* **Best practices:** use hashed identifiers (email SHA256) for privacy; manage suppression logic.

### Websites & Mobile Applications

**Purpose:** Collect events (page views, clicks, app events) for real-time behavioral profiles and journeys.

* **Methods:** browser SDKs, mobile SDKs, server-side event ingestion, webhooks.
* **Data schema:** eventName, timestamp, userId/deviceId, properties.
* **Latency:** streaming for real-time personalization.
* **Best practices:** include stable identifiers where possible; use consent flags; batch events when offline.

### Snowflake

**Purpose:** Bi-directional data exchange with a data warehouse for analytics, enrichment, large-scale storage.

* **Patterns:**

  * **Ingest from Snowflake**: scheduled COPY or export + connector → Data Cloud.
  * **Push to Snowflake**: Data Cloud exports (CDC or batch) into Snowflake tables.
* **Auth:** Private network options, keypair or OAuth depending on architecture.
* **Best practices:** keep a canonical schema; use staging tables for transformations; handle schema evolution carefully.

### Ingestions API

**Purpose:** Generic REST API for programmatic ingestion of events/records (used for custom apps, servers).

* **Typical usage:** server-side event sends, bulk uploads.
* **Benefits:** flexible; supports custom data shapes and high throughput.
* **Best practices:** use batching, idempotency keys, and monitor error topics.

### Other Connectors

Examples: JDBC, S3, FTP/SFTP, Google BigQuery, Azure Blob Storage, third-party SaaS connectors.

* **When to use:** legacy systems, file drops, non-standard data stores.
* **Best practices:** prefer secure transfer (SFTP/HTTPS), keep schema registry, automate file naming/versioning.

### Private Connect

**Purpose:** Secure, private network connectivity between Salesforce and partner clouds (e.g., Snowflake) using private links.

* **Benefits:** avoids public internet, reduces latency, increases security/compliance.
* **Setup notes:** requires network configuration, partner approvals, IP whitelisting and potentially VPN/Direct Connect.

Below are clean, professional, and easy-to-follow notes that your trainees can use as a reference during the Salesforce Data 360 / Data Cloud training.

## **Lab 1: Ingest Data Using CSV**

> **Objective:** Create a Data Stream to bring CSV data into Data Cloud.
> (Do this lab in a separate Salesforce Org. The ingested data will be used in the main org for the next lab.)

### **1. Prepare the CSV File**

* Ensure your CSV file contains the required Contact fields (e.g., FirstName, LastName, Email, Contact ID).

### **2. Create a Data Stream Using the CSV File**

- Go to **Data Cloud App** → **Data Streams**.
- Click **New** → select **Upload CSV**.
- Upload your CSV file.
- When prompted:

   * **Name the Data Lake Object (DLO):** `Contact_CSV`
- Confirm the field mappings and field types.
- **Deploy** the Data Stream.


## **Lab 2: Ingest Data From Another Salesforce Org**

> **Objective:** Connect an external Salesforce Org and ingest Contact data into Data Cloud.

### **Step 1 – Create a Salesforce CRM Connector**

- Go to **Data Cloud Setup**.
- Click **Salesforce CRM** under Connectors.
- Select **Connect to Salesforce Org**.
- Enter an **Alias Name** for the external org.
- Complete the **OAuth Authentication** using credentials of the external Salesforce user.
- Once connected, the CRM source becomes available for data ingestion.


### **Step 2 – Create a Data Stream Using the CRM Connector**

- Go to **Data Cloud App** → **Data Streams** → **New**.
- Choose the previously created **Salesforce CRM Connector**.
- Select the **Contact** object from the external org.
- For **Object Category**, choose **Other**.
- **Name the Data Lake Object (DLO):** `Contact_ExtOrg`
- Review and confirm the fields.
- **Deploy** the Data Stream.


### **Step 3 – Map the Data Lake Object to a Data Model Object (DMO)**

- Go to **Data Cloud App → Data Mapping**.
- Select the DLO: **Contact_ExtOrg**.
- Map it to the **Associate** DMO.
- Create the following field mappings:

| **Data Lake Object Field** | → | **Associate DMO Field** |
| -------------------------- | - | ----------------------- |
| FirstName                  | → | First_Name              |
| LastName                   | → | Last_Name               |
| Email                      | → | Work Email              |
| Contact ID                 | → | Associate ID            |

- Save and activate the mapping.

---

