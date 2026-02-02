# Salesforce Data Cloud Hands‑On Project

## End‑to‑End Implementation Documentation

***

## 1. Purpose and Scope

This document outlines the step‑by‑step implementation of an end‑to‑end Salesforce **Data Cloud** solution. The project demonstrates ingestion, data modeling, identity resolution, analytics, segmentation, transformation, and activation using standard Salesforce Data Cloud capabilities.

**Key Objectives:**

*   Ingest Salesforce core objects into Data Cloud
*   Establish standardized data models and relationships
*   Perform identity resolution and unification
*   Generate actionable insights using Calculated Insights and Segments
*   Activate results via Data Actions and Platform Events

***

## 2. Data Ingestion

### 2.1 Create Data Streams

Data Streams are configured to ingest data from Salesforce Core CRM objects.

**Objects Ingested:**

*   Account
*   Contact
*   Order

**Steps:**

1.  Navigate to **Data Cloud Setup → Data Streams**
2.  Create individual data streams for:
    *   Account object
    *   Contact object
    *   Order object
3.  Validate successful ingestion and Data Lake Object (DLO) creation for each source

***

## 3. Data Modeling and Mapping

### 3.1 Contact Data Mapping

The **Contact DLO** is mapped to the following **Standard Data Model Objects (DMOs)**:

| Source DLO | Target DMO                     |
| ---------- | ------------------------------ |
| Contact    | Individual (Standard)          |
| Contact    | ContactPointEmail (Standard)   |
| Contact    | ContactPointPhone (Standard)   |
| Contact    | ContactPointAddress (Standard) |

**Purpose:**

*   Enables individual‑centric identity resolution
*   Standardizes contact channels (email, phone, address)

***

### 3.2 Order Data Mapping

The **Order DLO** is mapped to:

| Source DLO | Target DMO            |
| ---------- | --------------------- |
| Order      | SalesOrder (Standard) |

***

## 4. Data Relationships

### 4.1 SalesOrder to Individual Relationship

A **N:1 relationship** is created between:

*   **SalesOrder → Individual**

**Relationship Configuration:**

*   **Relationship Type:** N:1
*   **Join Field:**
    *   SalesOrder.Bill\_To\_Contact
    *   Individual.Individual\_Id

**Purpose:**

*   Associates multiple orders to a single individual
*   Enables transaction‑level analytics at the individual level

***

## 5. Identity Resolution

### 5.1 Identity Resolution Ruleset Creation

An **Identity Resolution Ruleset** is created with the following matching criteria:

    (Fuzzy Name AND Normalized Email)
    OR
    (Fuzzy Name AND Normalized Phone)

**Configuration Highlights:**

*   Uses probabilistic matching for name
*   Uses normalized formats for email and phone
*   Logical OR enables flexible matching across channels

***

### 5.2 Identity Resolution Execution

The ruleset is executed to generate unified profiles.

**Generated Objects Include:**

*   UnifiedIndividual
*   UnifiedIndividualLink
*   Identity Resolution Result objects

***

### 5.3 Consolidation Review and Optimization

*   Review **consolidation rate** post‑resolution
*   Adjust matching thresholds if over‑ or under‑consolidation occurs
*   Modify **reconciliation rules** when attribute conflicts arise
*   Re‑run the ruleset after adjustments

***

## 6. Calculated Insights

### 6.1 Calculated Insights Creation

A Calculated Insight is created using:

**DMOs Selected:**

*   UnifiedIndividual
*   SalesOrder

**Automatic Join Behavior:**

*   Data Cloud automatically derives joins using defined relationships

**Filters Applied:**

*   Orders placed in the **last 30 days**

**Aggregations (Grouped by UnifiedIndividual):**

*   **Average Purchase Value**
*   **Grand Total Amount**
*   **Total Orders**

***

## 7. Segmentation

### 7.1 Segment: High‑Value Loyal

A segment named **High‑Value Loyal** is created based on the Calculated Insights.

**Segment Criteria:**

*   Average Purchase Value **> 150**

**Purpose:**

*   Identifies high‑spending, loyal customers
*   Enables targeted engagement and activation

***

## 8. Data Transformation

### 8.1 Order Summary Data Transform

A **Data Transform** is created to generate an aggregated order summary.

**Source DMO:**

*   SalesOrder

**Joins:**

*   SalesOrder → UnifiedIndividual
*   UnifiedIndividual → Individual

**Derived and Aggregated Metrics:**

*   Number of days since last order
*   Grand Total Amount
*   Order Count
*   Average Order Value

**Grouping:**

*   Grouped by UnifiedIndividual

**Output:**

*   Persisted into a new custom DMO:  
    **Order\_Summary**

***

## 9. Data Activation

### 9.1 Data Action Target

A **Data Action Target** is configured:

*   **Target Type:** Salesforce Platform Event
*   **Event Name:** High‑Value Loyal

***

### 9.2 Data Action Creation

A Data Action is created using the **Order\_Summary** DMO.

**Trigger Condition:**

*   Average Order Value **>= 200**

**Outcome:**

*   Publishes records meeting the criteria to the **High‑Value Loyal** Platform Event
*   Enables real‑time downstream integrations or automations

***
