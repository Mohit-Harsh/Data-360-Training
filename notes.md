
<center><img width='200px' src='./assets/data_360_logo.png'/></center>

<h1 align='center'>Salesforce Data 360</h1>

<br>

# Module 1 — Data Cloud / Data 360: Concepts & Architecture (FOUNDATION)

**Learning objectives**

* Understand what Salesforce Data Cloud (Data 360) is and its business value (CDP concepts). ([Salesforce Architects][1])
* Know the core architectural pieces (DLO, DMO, unified profiles / identity graph, activation destinations) and how they connect. ([Salesforce Developers][2])
* Recognize rebranding notes (Data Cloud → *Data 360*) and where to look for updated docs. ([Salesforce Architects][1])

## 1. What is Data Cloud / Data 360 — high level & business value

* **Definition (one line):** Data 360 is Salesforce’s customer data platform (CDP) that unifies, harmonizes, and makes customer data actionable across the Salesforce stack. ([Salesforce Architects][1])
* **Business value:** single source of truth for customer data, faster personalization, real-time segmentation, safer governance of PII, and simplified activation to marketing/sales/service systems. ([Salesforce Architects][1])
* **When to use:** cross-cloud personalization, joined analytics across systems, powering Journeys/agents with unified customer context, or when you want “one view” without losing source fidelity.

## 2. Core architectural elements (simple definitions)

<center><img src='./assets/DataCloud_ArchDiagram052025.png'/></center>

* **Data Stream** — connector that brings data into Data 360 (or points to external data for zero-copy).
* **Data Lake Object (DLO)** — raw, ingested table(s). Think of DLOs as the “raw dump” from each source (each DLO = one table). Use DLOs when you need the original, source-shaped records. ([Salesforce Developers][2])
* **Data Model Object (DMO)** — a curated, business-facing view (a mapped/standardized model). DMOs are created by mapping fields from one or more DLOs into a stable schema used by downstream processes (segmentation, activation). DMOs are like “views” or the prepared dataset. ([Salesforce Developers][2])
* **Unified Profiles / Identity Graph** — identity resolution rulesets that link multiple source profiles into a single *Unified Individual* (single customer record used for segmentation & activation). ([Trailhead][3])
* **Calculated Insights** — derived/calculated metrics or signals (e.g., lifetime value) used in segmentation. ([Salesforce][4])
* **Activation destinations** — where segments or unified data are sent (Marketing Cloud Journeys, CRM updates, ad platforms, files, streaming endpoints). Activation can be streaming (near-real-time) or batch. ([Salesforce][5])

## 3. Zero-copy (data federation) — short explanation

* **What it is:** ability to query and use external data without physically copying it into Data 360 (federation / live query). Good when you must avoid duplication or keep very large datasets in place. ([Salesforce][6])
* **When to choose zero-copy:** very large tables, regulatory/PII constraints, or when you need the freshest record without ETL. Consider performance and mapping needs — sometimes a physical copy (ingest) is still preferable.

## 4. How the pieces connect (one-paragraph flow)

1. **Ingest or federate** data via Data Streams into DLOs (or reference external tables via zero-copy).
2. **Map DLO fields → DMO fields** to create harmonized, business-friendly objects (Customer, Transaction, Event). ([Salesforce][7])
3. **Create identity resolution rulesets** and unify to produce Unified Individuals (identity graph). ([Trailhead][3])
4. **Build real-time segments** (audiences) using DMOs & calculated insights.
5. **Activate** segments to destinations (streaming or batch) for Journeys, ads, CRM updates, or analytics. ([Salesforce][5])

## 5. Quick tips

* **Mapping is contract work:** DMOs are the contract consumers use — think carefully about stable field names and types. Changing a DMO field type later can be disruptive. ([Salesforce][9])
* **Identity rules matter:** loose rules → over-merging; strict rules → duplicate unified profiles. Start conservative, test thoroughly. ([Trailhead][3])
* **Zero-copy tradeoffs:** great for governance and freshness, but evaluate query latency and mapping complexity before choosing. ([Salesforce Ben][10])
* **Activation mode:** use streaming for near-real-time needs (agentic experiences / immediate journeys), batch for large periodic exports. ([Salesforce][5])

* Data 360 architecture overview — Salesforce Architect docs. ([Salesforce Architects][1])
* Data Model Objects & mapping guide — Salesforce Help / Developer docs. ([Salesforce][7])
* Trailhead module: Get to know Unified Profiles (hands-on identity examples). ([Trailhead][3])
* Zero-copy overview & guides (when to use, partner integrations). ([Salesforce][6])

[1]: https://architect.salesforce.com/fundamentals/data-360-architecture?utm_source=chatgpt.com "Data 360 Architecture"
[2]: https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-object-model.html?utm_source=chatgpt.com "Object Model in Data 360"
[3]: https://trailhead.salesforce.com/content/learn/modules/data-and-identity-in-salesforce-cdp/get-to-know-unified-profiles?utm_source=chatgpt.com "Get to Know Unified Profiles | Salesforce Trailhead"
[4]: https://help.salesforce.com/s/articleView?id=data.c360_a_glossary_guide.htm&language=en_US&type=5&utm_source=chatgpt.com "Data 360 Glossary of Terms"
[5]: https://help.salesforce.com/s/articleView?id=data.c360_a_dmo_activation.htm&language=en_US&type=5&utm_source=chatgpt.com "Activation for Data 360 Data Model Objects"
[6]: https://www.salesforce.com/data/connectivity/zero-copy/?utm_source=chatgpt.com "Data Cloud – Zero Copy Connectivity"
[7]: https://help.salesforce.com/s/articleView?id=data.c360_a_map_custom_data_model_objects.htm&language=en_US&type=5&utm_source=chatgpt.com "Map Data Model Objects"
[8]: https://help.salesforce.com/s/articleView?id=data.c360_a_omni_home.htm&language=en_US&type=5&utm_source=chatgpt.com "Create Omnichannel Journeys Using Unified Individual ..."
[9]: https://help.salesforce.com/s/articleView?id=001233477&language=en_US&type=1&utm_source=chatgpt.com "Data 360: Data Type of Data Lake Object and Data Model ..."
[10]: https://www.salesforceben.com/salesforce-data-cloud-zero-copy-when-and-when-not-to-use-it/?utm_source=chatgpt.com "Salesforce Data Cloud Zero Copy: When (and When Not) ..."

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

* Common DLO sources: Sales Cloud exports, event streams (e-commerce), POS CSVs, external warehouses (S3/GCS). You can also reference unstructured DLOs for files (UDLOs). ([Salesforce][3])
* DLO properties: schema mirrors source; fields are created at ingest; changing DLO field types after creation is restricted—plan carefully. ([Salesforce][4])


## 3. DMO

* Types: standard DMOs (prebuilt like Contact, Account) and custom DMOs (create when your business needs a specific harmonized view). Create custom DMOs when you need fields or semantics not provided by standard DMOs. ([Salesforce Developers][1])
* Behavior: a DMO can draw from one or multiple DLOs; mapping defines how source fields become DMO fields and how relationships are modeled. ([Salesforce][2])


## 4. Mapping rules & data types — practical rules

* **Compatibility:** DLO → DMO field data types must be compatible; once set, types are hard to change — decide types carefully (strings vs numeric vs date). ([Salesforce][5])
* **Primary keys & required mappings:** Profile/Other DMOs often require you to map the primary key; some DMOs require specific system fields to be mapped for identity or activation to work. Validate required mappings before activation. ([Salesforce][6])
* **Auto-mapping:** Data Cloud offers auto-mapping helpers (model card) — useful for initial mapping but always review and adjust for business rules. ([Salesforce][7])

## 5. Normalization & enrichment — where and how

* **Normalization (when to do it):** fix inconsistent formats (phone, email, currency), flatten nested JSON, split composite fields into atomic fields — typically done between DLO ingestion and DMO mapping (use interim DLOs or transforms). ([Salesforce Ben][8])
* **Enrichment (when to do it):** add derived fields (loyalty_score, lifetime_value), lookup references (geolocation), or external enrichments (third-party demographics) — usually created as calculated insights or written back to DMOs/DLOs as needed. ([Salesforce][9])

## 6. Quick tips

* **Plan types first:** once DLO/DMO fields are created, changing types is hard — design schemas thoughtfully. ([Salesforce][4])
* **Interim DLOs are your friend:** use them to clean noisy data before mapping into DMOs, especially for contact-point fields. ([Salesforce Ben][8])
* **Map only what you need:** avoid ingesting unnecessary columns — smaller DLOs are cheaper and faster. ([Scandiweb][13])
* **Document mapping decisions:** capture why you chose aggregations (SUM vs MAX), primary keys, and transforms — prevents later regressions. ([Salesforce][12])

## Lab — Data Mapping (DLO to DMO)

Mapping Data from `Contact` (Data Lake Object) to `Employee` (Data Model Object). 

### 1. Create Data Stream (Source Connection) 🔗

- In Data Cloud, go to `Data Streams` $\to$ New.
- Select `Salesforce CRM` from Connected Sources.
- Click on `View Objects` $\to$ Select `Contact` Object.
- Object Category $\to$ `Other`.
- Select the required fields: `Contact ID, FirstName, LastName, Email`.
- Click Next and then click Deploy.
- Data Lake Object will be created automatically.

### 2. Create Data Model Object

- Go to Data Model Objects in Data Cloud $\to$ Click New
- Add Fields: `Associate ID (Primary Key), First Name, Last Name, Email`.
- Save and Activate DMO.

### 3. Map Contact (DLO) to Associate (DMO)

- Go to `Data Streams` $\to$ Open the `Contact` Data Stream.
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


[1]: https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-object-model.html?utm_source=chatgpt.com "Object Model in Data 360"
[2]: https://help.salesforce.com/s/articleView?id=data.c360_a_map_custom_data_model_objects.htm&language=en_US&type=5&utm_source=chatgpt.com "Map Data Model Objects"
[3]: https://help.salesforce.com/s/articleView?id=service.fs_asp_data_streams.htm&language=en_US&type=5&utm_source=chatgpt.com "Create Data Streams and Data Lake Objects"
[4]: https://help.salesforce.com/s/articleView?id=001233477&language=en_US&type=1&utm_source=chatgpt.com "Data 360: Data Type of Data Lake Object and Data Model ..."
[5]: https://help.salesforce.com/s/articleView?id=data.c360_a_data_type_field_mappings.htm&language=en_US&type=5&utm_source=chatgpt.com "Data Types in Field Mappings"
[6]: https://help.salesforce.com/s/articleView?id=data.c360_a_required_data_mappings.htm&language=en_US&type=5&utm_source=chatgpt.com "Data Mapping Requirements in Data Cloud"
[7]: https://help.salesforce.com/s/articleView?id=data.c360_a_auto_mapping_model_card.htm&language=en_US&type=5&utm_source=chatgpt.com "Data Cloud Auto Mapping Model Card"
[8]: https://www.salesforceben.com/salesforce-data-cloud-contact-point-mapping-best-practices/?utm_source=chatgpt.com "Salesforce Data Cloud Contact Point Mapping Best Practices"
[9]: https://help.salesforce.com/s/articleView?id=ind.dpe_datalake_obj_as_wb.htm&language=en_US&type=5&utm_source=chatgpt.com "Use Data Cloud DLOs and DMOs as Target Objects"
[10]: https://help.salesforce.com/s/articleView?id=data.c360_a_add_data_lake_objects_to_a_data_space.htm&language=en_US&type=5&utm_source=chatgpt.com "Add Data to a Data Space Using Data Lake Objects"
[11]: https://help.salesforce.com/s/articleView?id=data.c360_a_identity_resolution_terms.htm&language=en_US&type=5&utm_source=chatgpt.com "Identity Resolution Terminology"
[12]: https://help.salesforce.com/s/articleView?id=sf.c360_a_data_mapping_best_practices.htm&language=en_US&type=5&utm_source=chatgpt.com "Data Mapping Best Practices"
[13]: https://scandiweb.com/blog/data-ingestion-in-salesforce-data-cloud/?utm_source=chatgpt.com "Data Ingestion in Salesforce Data Cloud: Practical Guide"

---
