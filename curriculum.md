# Salesforce **Data Cloud / Data 360** — End-to-End Course (basic → advanced)

**Sources & recommended official references:** Trailhead / Trailhead Academy Data Cloud fundamentals, Salesforce Developer & Help docs (Data Cloud / Data 360), and Data Cloud query & object model guides. ([trailheadacademy.salesforce.com][1])

---

## Course roadmap (high level)

1. Foundations & Concepts (Module 1–2)
2. Data Ingestion & Modeling (Module 3–4)
3. Identity Resolution & Profiles (Module 5)
4. Segmentation, Calculated Insights & Activation (Module 6–7)
5. Querying, APIs & Developer Topics (Module 8–10)
6. Governance, Privacy, Monitoring & Cost Management (Module 11)
7. Advanced Use Cases, Integrations & Real-Time Activation (Module 12–13)
8. Capstone project + assessment (Module 14)

---

## Module-by-module syllabus

### Module 1 — Data Cloud / Data 360: Concepts & Architecture (FOUNDATION)

**Duration:** 3 hours  

**Learning objectives:** Understand what Data Cloud (Data 360) is, how it fits in the Salesforce stack, and core capabilities (unified profiles, activation, real-time segmentation, zero-copy integrations). ([Salesforce][2])  

**Topics:**

* What is Data Cloud / Data 360 and business value (CDP concepts).
* High-level architecture: DLOs, DMOs, unified profiles, identity graph, activation destinations. ([Salesforce Developers][3])
* Rebranding note: Data Cloud → Data 360 (what to expect in docs). ([Salesforce Developers][4])  
  
**Lab:** Walkthrough of Data Cloud UI (Trailhead lab / Academy SDC101). ([trailheadacademy.salesforce.com][1])

---

### Module 2 — Data Lake Objects (DLOs) & Data Model Objects (DMOs) — Modeling fundamentals

**Duration:** 6 hours (lecture + lab)

**Prereq:** Module 1

**Learning objectives:** Understand DLO vs DMO, mapping raw sources to a harmonized model, field types and constraints. ([Salesforce Developers][3])

**Topics:**

* DLOs: ingestion containers & raw schema.
* DMOs: virtual/harmonized views; when to create custom DMOs.
* Mapping rules, data types, normalization, and enrichment points. ([Salesforce][5])  

**Lab:** Create a DLO, map fields to a DMO, validate types.

---

### Module 3 — Data Ingestion & Streaming (BATCH & REAL-TIME)

**Duration:** 6–8 hours

**Prereq:** Modules 1–2

**Learning objectives:** Ingest data from CRM, marketing, e-commerce, data lakes; set up streaming connectors; zero-copy integrations (Snowflake/Databricks). ([Salesforce][2])

**Topics:**

* Connectors: Salesforce sources (Sales/Service/Marketing Clouds), file/FTP, APIs, Streaming/CDC, and partner integrations.
* Data mapping patterns, schema evolution, incremental loads, error handling.
* Zero-copy architecture and using external tables (Snowflake/DB lakes). ([Salesforce][2])

**Lab:** Configure sample ingestion from CSV and from a Salesforce org.

---

### Module 4 — Data Quality & Transformation (ENRICHMENT)

**Duration:** 4–6 hours

**Prereq:** Module 3

**Learning objectives:** Implement data quality rules, transformations, normalization, enrichment (calculated fields).

**Topics:** Validation rules, deduplication pre-processing, transformations using Data Cloud calculated insights, scheduling transformations.

**Lab:** Implement a transform to standardize phone numbers and create calculated fields for lifetime value.

---

### Module 5 — Identity Resolution & Unified Profiles (CORE)

**Duration:** 6–8 hours

**Prereq:** Modules 2–4

**Learning objectives:** Design match rulesets, reconciliation rules, and understand golden record principles. Learn determinist vs probabilistic matching and key-rings/graphs. ([Salesforce][6])

**Topics:**

* Identity graph basics, match criteria, match methods (deterministic, fuzzy), reconciliation (which value wins).
* Identity rulesets lifecycle and performance considerations.
* Best practices for PII handling, consent, and compliance.
  
**Lab:** Build a ruleset: create match rules, run identity resolution on a test dataset, review unified profiles.


---

### Module 6 — Segmentation & Calculated Insights (REAL-TIME PERSONAS)

**Duration:** 6 hours

**Prereq:** Module 5

**Learning objectives:** Create segments (real-time & scheduled), build calculated insights (aggregates, recency/frequency metrics), and use segments for activation.

**Topics:**

* Segment types: streaming vs batch; segmentation builder concepts.
* Calculated insights: aggregation, windowing, derived attributes.
* Use cases: churn detection, high-value customer segments.
  
**Lab:** Create a real-time segment (e.g., "Browsed product X in last 24h + purchase in last year = No") and a calculated LTV field.


---

### Module 7 — Activation & Orchestration (Marketing/Service/Sales)

**Duration:** 6 hours

**Prereq:** Module 6

**Learning objectives:** Activate segments to Marketing Cloud, Advertising platforms, Salesforce CRM, or external systems; automate activation workflows. ([Salesforce][7])

**Topics:**

* Activation destinations and connectors (Marketing Cloud, Email, Ads, custom APIs).
* Real-time vs scheduled activation; idempotency & consent propagation.
* Use with Journey Builder / Marketing Cloud and with Sales/Service flows.
  
**Lab:** Activate a segment to Marketing Cloud Audience or export to webhook and confirm delivery.

---

### Module 8 — Querying Data Cloud (SQL / SOQL / Query APIs)

**Duration:** 6 hours

**Prereq:** Module 2 & 3 and Module 0 (SQL)

**Learning objectives:** Query DLOs/DMOs using Data Cloud SQL and SOQL where appropriate; use Query APIs; write efficient queries. ([Salesforce Developers][8])

**Topics:**

* Data Cloud SQL syntax and limitations (read-only contexts), SOQL usage against DMOs/DLOs.
* Query performance, recommended practices, and limiting costs.
* Using Query APIs and SDKs for exports & analytics. ([Salesforce Developers][9])
  
**Lab:** Write SQL queries to compute cohort metrics; use Query API to export results.


---

### Module 9 — Developer & API Integration (ADVANCED)

**Duration:** 8 hours (lecture + coding labs)

**Prereq:** Module 8 + familiarity with Apex/JS/Python

**Learning objectives:** Use Data Cloud REST APIs/SDKs, integrate with Apex, build custom applications that consume calculated insights or profiles. ([Salesforce Developers][10])

**Topics:**

* Data 360 APIs (profile API, query API), SDKs, auth patterns (OAuth), rate limits.
* Eventing and webhooks; building microservices to react to identity or segment changes.
* Examples: pull unified profile in Apex, call Data Cloud query from external app.
  
**Lab:** Build a small app that queries a profile and displays calculated insight via API.

---

### Module 10 — Analytics, Reporting & BI connections

**Duration:** 6 hours

**Prereq:** Modules 2, 8, 9

**Learning objectives:** Connect Data Cloud to Tableau/Einstein/BI tools, create dashboards from DMOs, use calculated insights for analytics. ([Salesforce Developers][11])

**Topics:**

* Tableau / Einstein integrations, building datasets from Data Cloud.
* Near real-time dashboards vs batch reports; sample KPIs (LTV, churn, engagement).
  
**Lab:** Build a Tableau extract / dashboard using Data Cloud dataset.

---

### Module 11 — Governance, Security, Privacy & Cost Management

**Duration:** 4–6 hours

**Prereq:** Modules 1–6

**Learning objectives:** Understand data governance, PII handling, consent, audit logging, and controlling Data Cloud costs (consumption patterns). ([Salesforce][12])

**Topics:**

* Data access controls, masking, encryption, deletion/retention policies.
* Audit trails, lineage, certification; compliance (GDPR, CCPA) considerations.
* Monitoring ingestion/query costs and best practices to reduce spend.

**Lab:** Implement role-based access for a DMO and simulate a data deletion request flow.

---

### Module 12 — Real-time Use Cases & Streaming Architectures (Advanced)

**Duration:** 6–8 hours

**Prereq:** Modules 3, 6, 9

**Learning objectives:** Build real-time personalization, real-time journeys, and streaming ingestion patterns (Kafka/CDC).

**Topics:**

* Real-time personalization example flows (web personalization, in-app messaging).
* Integrating device IDs, ad platforms, and streaming connectors.

**Lab:** Build a working demo: real-time segment → webhook → front-end personalization response.

---

### Module 13 — Advanced Identity, Graphs & ML-driven Insights

**Duration:** 6–8 hours

**Prereq:** Modules 5, 8, 9, 10

**Learning objectives:** Advanced identity graph work, using ML for probabilistic matching, leveraging Einstein/third-party ML models for predictions (churn/propensity). ([Salesforce Developers][11])

**Topics:**

* Graph strategies, enrichment with device & behavioral signals, ML pipelines.
* Integrating external ML models and orchestrating model outputs into calculated insights.

**Lab:** Run a simple propensity model, store scores as calculated insights, and create a segment using the scores.

---

### Module 14 — Capstone Project & Certification Preparation

**Duration:** 2–5 days (project) + 1 day exam prep

**Prereq:** All prior modules

**Capstone options (choose 1):**

* Build an end-to-end Data Cloud solution: ingest CRM + e-commerce + web events, perform identity resolution, build LTV & propensity calculated insights, create segments, and activate to Marketing Cloud for a campaign.
* Real-time personalization demo with segment activation to a web widget and CRM case creation for high-value customers.
  
**Deliverables:** Architecture diagram, implementation walkthrough, demo, cost & governance plan, test results.

---

## Quick reference: Official learning & docs (start here)

* Trailhead Academy — Data Cloud fundamentals (SDC101). ([trailheadacademy.salesforce.com][1])
* Data 360 / Data Cloud Developer Guide & Query Guide (APIs, SQL). ([Salesforce Developers][10])
* Help articles: Identity Resolution, Data Lake Objects, Data Model Objects. ([Salesforce][6])
* Certification prep: Data Cloud Consultant study guide. ([Trailhead][13])

---

[1]: https://trailheadacademy.salesforce.com/classes/sdc101-discover-salesforce-data-cloud-fundamentals?utm_source=chatgpt.com "Discover Salesforce Data Cloud Fundamentals - SDC101"
[2]: https://www.salesforce.com/in/data/what-is-data-cloud/?utm_source=chatgpt.com "What Is Data 360?"
[3]: https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-object-model.html?utm_source=chatgpt.com "Object Model in Data 360"
[4]: https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-get-started.html?utm_source=chatgpt.com "Data 360 Developer Guide"
[5]: https://help.salesforce.com/s/articleView?id=data.c360_a_map_custom_data_model_objects.htm&language=en_US&type=5&utm_source=chatgpt.com "Map Data Model Objects"
[6]: https://help.salesforce.com/s/articleView?id=data.c360_a_match_rules.htm&language=en_US&type=5&utm_source=chatgpt.com "Identity Resolution Match Rules"
[7]: https://www.salesforce.com/in/marketing/data/?utm_source=chatgpt.com "The World's #1 Customer Data Platform (CDP)"
[8]: https://developer.salesforce.com/docs/data/data-cloud-query-guide/guide/query-guide-get-started.html?utm_source=chatgpt.com "Data 360 Query Guide"
[9]: https://developer.salesforce.com/docs/data/data-cloud-query-guide/guide/dc-sql-query-apis.html?utm_source=chatgpt.com "Data 360 SQL Query APIs"
[10]: https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-dev-overview.html?utm_source=chatgpt.com "Data 360 Development Overview"
[11]: https://developer.salesforce.com/blogs/2024/05/data-cloud-einstein-ai-the-developers-pocket-guide?utm_source=chatgpt.com "Data Cloud + Einstein AI: The Developer's Pocket Guide"
[12]: https://www.salesforce.com/in/data/?utm_source=chatgpt.com "Data 360 (Formerly Data Cloud)"
[13]: https://trailhead.salesforce.com/content/learn/modules/cert-prep-data-cloud-consultant/get-started-with-salesforce-data-cloud-consultant-certification-prep?utm_source=chatgpt.com "Salesforce Data Cloud Certification Prep Guide - Trailhead"
