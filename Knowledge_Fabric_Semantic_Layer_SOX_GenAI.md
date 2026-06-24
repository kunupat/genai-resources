# Knowledge Fabric, Semantic Layer, and SOX Compliance in the Age of GenAI Agents

**A Comprehensive Architecture Guide for Enterprise-Grade AI Systems**

> *Written from the perspective of a seasoned Software Solution Architect specializing in GenAI architecture, data architecture, and SOX compliance.*

---

## Table of Contents

1. [Introduction: The Enterprise AI Context Problem](#1-introduction)
2. [Foundations: Dimensions, Metrics, and Structured Analytics](#2-foundations)
3. [Unstructured Data and the RAG Spectrum](#3-rag-spectrum)
   - 3.1 [Architectural Types of RAG](#31-architectural-types)
   - 3.2 [Advanced RAG Variants](#32-advanced-rag-variants)
   - 3.3 [Non-RAG Methods for Unstructured Data](#33-non-rag-methods)
   - 3.4 [Sub-Techniques and Engineering Patterns](#34-sub-techniques)
   - 3.5 [Handling Context Window Limits](#35-context-window-limits)
4. [Metadata Semantics: Glossary, Taxonomy, Ontology, Lineage, and Provenance](#4-metadata-semantics)
5. [The Semantic Layer for GenAI Agents](#5-semantic-layer)
6. [Knowledge Fabric Architecture](#6-knowledge-fabric)
   - 6.1 [What Is a Knowledge Fabric?](#61-what-is-knowledge-fabric)
   - 6.2 [Enterprise Knowledge Fabric Blueprint](#62-enterprise-blueprint)
   - 6.3 [Modular RAG Combinations for the Fabric](#63-modular-rag-combinations)
7. [Open Standards: OKF and Data Contracts](#7-open-standards)
   - 7.1 [Google Cloud's Open Knowledge Format (OKF)](#71-okf)
   - 7.2 [Google Cloud Knowledge Catalog](#72-knowledge-catalog)
   - 7.3 [Data Contract Standard vs. OKF](#73-data-contract-vs-okf)
8. [Financial GenAI Agents: A Reference Architecture](#8-financial-genai)
   - 8.1 [Combining Semantic Layer and Unstructured Knowledge](#81-combining)
   - 8.2 [Amazon Strands Agents SDK Implementation](#82-strands)
9. [SOX Compliance in Agentic AI Systems](#9-sox-compliance)
   - 9.1 [Provenance as the SOX Foundation](#91-provenance)
   - 9.2 [SOX Control Injection into the Architecture](#92-sox-controls)
   - 9.3 [Dual-Identity Tracing: Users and Agents](#93-dual-identity)
   - 9.4 [COSO & ISACA-Aligned SOX Checklist](#94-sox-checklist)
10. [LLMOps: Observability, Lineage, and Evaluation](#10-llmops)
11. [Conclusion and Architectural Recommendations](#11-conclusion)

---

## 1. Introduction: The Enterprise AI Context Problem

Enterprise organizations deploying Generative AI face a deceptively simple problem: their AI systems are brilliant in the abstract but ignorant of context. A foundation model can write Python code, analyze balance sheets, and synthesize research — but only if it understands what the data *means* in your organization. It cannot inherently know that `cust_status = 2` means "churned," that your fiscal year runs on a 4-4-5 week calendar, or that the `fct_revenue` table in production is the only authorized source for external reporting.

This is the **semantic problem**, and it is the central challenge of enterprise GenAI adoption in 2026. Solving it requires a layered architecture that brings together three historically separate disciplines:

- **Data architecture** — the fabric of how enterprise data is ingested, modeled, governed, and stored
- **Knowledge engineering** — the discipline of giving machines human-level semantic understanding of business concepts
- **Compliance governance** — specifically Sarbanes-Oxley (SOX) controls for financial systems, which must remain deterministic, auditable, and tamper-proof even when AI agents are making autonomous decisions

This article synthesizes all three disciplines into a cohesive, production-grade architecture. It covers the full spectrum from foundational analytics concepts through advanced RAG variants, the design of an enterprise Knowledge Fabric, the role of the Semantic Layer for AI agents, open standards like Google's Open Knowledge Format (OKF), and a concrete approach to enforcing SOX compliance in an agentic AI financial system built on Amazon Strands and AWS.

---

## 2. Foundations: Dimensions, Metrics, and Structured Analytics

Before building AI on top of enterprise data, architects must ensure that data is modeled consistently. The most elementary — and most frequently violated — distinction in analytics is the one between **dimensions** and **metrics**. [1, 2]

**Dimensions** are the descriptive categories that organize and segment data. They answer the *who, what, when, and where* of a business event. They are qualitative attributes — usually text or dates — that function as grouping and filtering axes. Examples: `Customer Segment`, `Product Category`, `Sales Region`, `Date`. [2, 3, 4, 5, 6]

**Metrics** are quantitative measurements that evaluate the performance of those dimensions — the *how many* and *how much*. They can be counted, summed, or averaged. Examples: `Revenue`, `Units Sold`, `Churn Rate`, `Average Order Value`. [1, 2, 7, 8]

Analytics reports always combine both. The query "which device type generates the most revenue?" pairs the dimension `Device Type` (Mobile, Desktop) with the metric `Revenue` ($10,000, $15,000). [3]

| Feature [1, 2, 5, 6, 9] | Dimensions | Metrics |
|---|---|---|
| **Questions Answered** | Who, what, where, when | How many, how much |
| **Data Type** | Usually text, categories, or dates | Always numbers |
| **In a Spreadsheet** | The row labels or column headers | The values inside the cells |

This distinction is architecturally critical because AI agents operating on financial data must retrieve metrics through governed, pre-calculated pathways — not by guessing at raw column semantics. The Semantic Layer, discussed in Section 5, is the mechanism that enforces this.

---

## 3. Unstructured Data and the RAG Spectrum

While structured analytics governs the world of metrics and dimensions, the majority of enterprise knowledge lives in unstructured formats — PDFs, emails, Word documents, audit memos, legal contracts, earnings call transcripts, and SEC filings. Making this content accessible to LLMs and AI agents without hallucination is the domain of **Retrieval-Augmented Generation (RAG)**. [1, 2, 3, 4, 5]

RAG has evolved from a simple keyword-lookup pattern into a family of highly sophisticated, multi-stage reasoning architectures. Understanding the full spectrum is essential to selecting the right pattern for each enterprise workload.

### 3.1 Architectural Types of RAG

RAG architectures are classified by their complexity, data-routing logic, and chronological evolution. [6]

**Naive RAG** is the foundational pipeline. It splits text into chunks, converts them to vector embeddings, stores them in a vector database, and retrieves the top-K semantically similar chunks for a given query. [7, 8, 9, 10, 11] It is appropriate for simple Q&A over small, stable document sets but fails for complex enterprise scenarios due to its inability to handle stale knowledge, irrelevant retrieval, or multi-step reasoning.

**Advanced RAG** introduces pre-retrieval and post-retrieval optimization strategies including chunk refinement, metadata filtering, reranking, and query rewriting to overcome Naive RAG's limitations. [12, 13, 14, 15, 16]

**Modular RAG** transcends linear pipelines entirely. It integrates specialized modules — search engines, memory modules, multi-routing graphs — into customizable, swappable workflows. [17, 18, 19, 20, 21] This is the pattern that underpins an enterprise Knowledge Fabric.

**Graph RAG (Knowledge Graph RAG)** combines vector databases with structural Knowledge Graphs. It extracts entities and relationships from unstructured text as `(Subject, Predicate, Object)` triples, enabling LLMs to answer complex, global queries that require reasoning across an entire dataset rather than a single document. [22, 23, 24, 25, 26]

**Agentic RAG** designs the retrieval pipeline as an orchestrator of multiple autonomous AI agents, each equipped with tools to query different data sources, synthesize answers, and double-check outputs in a ReACT (Reason + Act) loop. [37, 38, 39, 40, 41]

### 3.2 Advanced RAG Variants

Beyond the architectural categories, specific named variants address targeted problems:

**Corrective RAG (CRAG)** adds an evaluation gate between retrieval and generation. A lightweight retrieval evaluator grades document chunks for relevance and correctness. If the grade is below threshold, CRAG automatically triggers a web search or re-queries with a refined query before passing context to the LLM. [27, 28, 29, 30, 31] This is the paper-based innovation introduced in *Corrective Retrieval Augmented Generation* on arXiv. [11, 12, 13]

**Self-RAG** trains or prompts a single LLM to critique its own generation in real-time by outputting special "reflection tokens" — `[Retrieval]`, `[IsRel]`, `[IsSup]`, `[IsUse]` — that grade whether retrieval is needed, whether retrieved passages are relevant, and whether the answer is grounded. [14, 15] The seminal paper is *Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection*. [14, 15, 16, 17]

**Adaptive RAG** routes queries to different retrieval pipelines based on assessed complexity. A lightweight classifier evaluates the incoming prompt: simple queries bypass vector search entirely; complex queries trigger deep retrieval or web lookup. [18, 19] This follows from *Adaptive-RAG: Learning to Adapt Retrieval-Augmented Large Language Models through Question Complexity*. [18, 19, 20, 21, 22]

**Lean RAG** structures knowledge hierarchically — summarizing entities and building explicit relations — so the LLM traverses optimized pathways rather than performing heavy flat retrieval. It significantly reduces computational overhead on massive enterprise datasets. [23, 24, 25] The approach is formalized in *LeanRAG: Knowledge-Graph-Based Generation with Semantic Aggregation and Hierarchical Retrieval*. [25, 26, 27]

**ComoRAG** is a cognitive-inspired framework designed for long narrative reasoning across 200K+ token documents. It imitates the human prefrontal cortex using a persistent memory workspace and actively asks probing questions to resolve logical contradictions across long texts. [28, 29] See *ComoRAG: A Cognitive-Inspired Memory-Organized RAG for Stateful Long Narrative Reasoning*. [28, 30, 31, 32]

**REX RAG (Reasoning Exploration with Policy Correction)** enhances the reinforcement learning aspect of LLM reasoning. When a model is stuck in an unproductive reasoning line, REX-RAG injects probing prompts to explore alternative paths and uses importance-sampling policy correction to prevent model degradation. [33, 34] Formalized in *REX-RAG: Reasoning Exploration with Policy Correction in Retrieval-Augmented Generation*. [33, 35, 36]

**RAG-Agency (Agentic RAG)** merges RAG with autonomous AI agents. The LLM acts as an orchestrator with access to multiple tools (APIs, vector stores, calculators, web search) and loops autonomously until it has gathered sufficient evidence for a comprehensive, accurate response. [2, 37] IBM defines this architecture in *What is Agentic RAG?* [2, 38, 39]

**RAG 3.0** represents the next generation: fully Agentic RAG with native multi-modal reasoning across text, video, and audio. It shifts from a simple information retriever into a cognitive AI capable of processing multimodal content and correcting its own reasoning errors holistically. [37, 40] See *How RAG 3.0 Is Eliminating Hallucinations*. [5, 37, 40, 41, 42]

### 3.3 Non-RAG Methods for Unstructured Data

RAG is not always the right tool. Several complementary methods address scenarios where RAG's overhead or limitations outweigh its benefits. [61, 62, 63, 64, 65]

**Direct Context Stuffing (Long-Context Windows)** feeds raw unstructured text directly into the model's prompt, relying on modern models with 1M–2M token context windows. [66, 67, 68, 69, 70] It is effective for one-off analysis but expensive at scale and subject to the "lost in the middle" degradation effect.

**Fine-Tuning (FT) and Continued Pre-training** permanently adapts model weights to domain-specific language, syntax, and tone via supervised training. Fine-tuning is usually combined with RAG for optimal results. [71, 72, 73, 74, 75]

**Semantic Caching** checks whether an incoming query is semantically identical to a prior query. If a close match exists, the cached answer is served instantly, bypassing the entire RAG pipeline. This is essential for cost control at enterprise scale. [76, 77, 78, 79, 80]

| Method [81, 82, 83, 84, 85] | Latency | Implementation Cost | Best Used For |
|---|---|---|---|
| **Naive RAG** | Low | Low | Simple Q&A on small text files |
| **Graph RAG** | High | High | Finding hidden relationships across massive datasets |
| **Agentic RAG** | High | Medium-High | Complex, multi-step research and workflows |
| **Long Context** | High | Very High (Token Costs) | One-off analysis of entire books/codebases |
| **Fine-Tuning** | Ultra-Low | High | Changing model tone, format, and style |

### 3.4 Sub-Techniques and Engineering Patterns

Each RAG architectural type contains a rich ecosystem of sub-techniques. [1]

**Within Naive RAG:** Fixed-size chunking (e.g., 500 tokens with 50-token overlap); dense embedding generation using models like `text-embedding-3`; vector similarity search using Cosine Similarity, Dot Product, or L2 (Euclidean) distance; and Approximate Nearest Neighbor (ANN) indexing via HNSW (Hierarchical Navigable Small World) or IVF (Inverted File Index).

**Within Advanced RAG:**
- *Sentence Window Retrieval* — embed individual sentences for precision, but retrieve a surrounding 3–5 sentence window for richer context
- *Parent-Child (Auto-Merging) Retrieval* — small child nodes (e.g., 100 tokens) link back to larger parent nodes (1000 tokens); if enough child nodes match, the system delivers the full parent chunk
- *Metadata Enrichment* — tag chunks with date, author, and chapter to enable SQL-like predicate filtering before vector search
- *Hybrid Search (Sparse + Dense)* — combine semantic vector search with keyword-based BM25 using Reciprocal Rank Fusion (RRF)

**Within Graph RAG:**
- *Entity-Relation Triple Extraction* — LLMs parse text into `(Subject, Predicate, Object)` triples (e.g., `[Company A] → [Acquired] → [Company B]`)
- *Leiden Community Detection* — clustered indexing groups highly interconnected nodes into hierarchical communities, enabling global summarization
- *Graph Vector Hybridization* — store node text descriptions as vectors and relationships as edges, combining traversal with semantic matching

**Within Agentic RAG:**
- *Sub-Query Decomposition* — break user requests into a dependency tree of parallel sub-tasks
- *Tool-Use (Function Calling)* — give agents structured access to external APIs, Confluence wikis, Jira boards, and databases
- *Plan-And-Solve Loops* — implement ReACT or Plan-and-Solve architectures where the agent drafts a plan, gathers data, evaluates progress, and adapts dynamically

### 3.5 Handling Context Window Limits

Even with modern large context windows, placing the most critical information at the very beginning or end of a prompt is essential to avoid the "lost in the middle" degradation effect — a phenomenon where LLMs disproportionately attend to information positioned at the extremes of long prompts. For architectures requiring O(N²) quadratic-cost Transformer attention over truly massive contexts, alternative state-space architectures like **Mamba** offer O(N) linear scaling efficiency.

---

## 4. Metadata Semantics: Glossary, Taxonomy, Ontology, Lineage, and Provenance

To build AI agents that reason accurately about business data, the data must be deeply annotated with semantic metadata. In enterprise data architecture, **glossaries**, **taxonomies**, **ontologies**, **lineage**, and **provenance** represent escalating levels of structure, meaning, and accountability. [1, 2]

### Glossary: The Dictionary

A glossary is a collection of business and domain-specific terms, acronyms, and their definitions. It establishes a **shared vocabulary** and answers the question: *"What does this term actually mean in our organization?"* [3, 4, 5, 6, 7] For example, it defines "Churn" as a cancelled account within 30 days, or "ARR" as Annual Recurring Revenue calculated in a specific way. AI agents require this layer to prevent hallucinated definitions and miscalculation — a formula like `Gross Profit = Revenue - COGS` must be machine-readable and authoritative.

### Taxonomy: The Filing System

A taxonomy is a hierarchical classification system that groups terms into parent-child trees. [8, 9] An example financial taxonomy: `Assets → Current Assets → Cash & Equivalents`. Taxonomies provide **structural context**, dictating how data assets are tagged and categorized for browsing, faceted filtering, and searching. [10, 11, 12, 13, 14] If the glossary defines a term, the taxonomy decides where that term lives in the broader knowledge hierarchy.

### Ontology: The Web of Meaning

An ontology is a multidimensional network of concepts that defines not just what entities are, but **how they relate to each other** and the logical rules that govern them. [15, 16] An ontology can express rules such as: *"A Customer places an Order"* and *"An Order contains a Product."* This enables AI models to perform **semantic reasoning** — deducing implicit relationships without someone explicitly programming every connection. W3C open standards (RDF, OWL, SHACL) are the authoritative specification for building domain ontologies. [11, 16, 17]

Think of these three layers as building blocks: the **glossary** provides the agreed-upon words, the **taxonomy** stacks them in order so you can find them, and the **ontology** creates the web of relationships so you can understand how they interact. [3, 18, 19, 20, 21]

### Data Lineage: The Pipeline Map

Data lineage is the lifecycle map of a data point — where it comes from, how it was transformed, and where it flows. [1, 2, 3, 4] If the glossary, taxonomy, and ontology define *meaning* and *structure*, lineage provides *provenance* and *audit trail*. There are three distinct types:

**Business Lineage** tracks the high-level journey across business milestones. For example, an AI agent examining "Q2 Revenue" can trace it from *Signed Salesforce Opportunities* through *NetSuite Invoices* to the *Workday Financial Report*. [5]

**Technical Lineage** tracks exact databases, tables, columns, and ETL code. An agent can confirm that `Invoices.Total_Amount` was extracted via Fivetran, transformed by a dbt model, and materialized into `fct_revenue` in Snowflake. [6, 7, 8, 9]

**Semantic Lineage** tracks how calculations, aggregations, and currency conversions changed the numbers. An agent can trace that a raw €10,000 transaction on June 1st was multiplied by a 0.91 exchange rate, aggregated into "EMEA Revenue," and rolled up into "USD Net Revenue." [10, 11, 12]

Lineage is critical for AI agents because it enables trust and explainability, prevents hallucination (the agent queries the verified production table, not an outdated sandbox), and supports impact analysis when formulas change. [13, 14, 15]

### Data Provenance: The Birth Certificate

Provenance is the certified record of ownership, origin, and authenticity of a specific data point. [1, 2] While lineage maps the *path* data took, provenance answers: *"Who or what created this data, and can the AI agent legally, ethically, and securely trust it?"*

The four core pillars of provenance metadata are:
- **Source Authority** — the exact originating entity (Bloomberg terminal vs. scraped blog)
- **Identity & Ownership** — the specific user, system, or vendor that created or modified the record
- **Time Anchoring** — the precise, unalterable timestamp of creation, modification, and finalization [3]
- **Regulatory Attestation** — whether the data is SOC2, GDPR, or banking-privacy compliant

The complete semantic layer for a financial AI agent integrates all five layers:

| Layer [16] | Question for the AI Agent | Financial Example |
|---|---|---|
| **Glossary** | *What is it?* | EBITDA = Earnings Before Interest, Taxes, Depreciation, and Amortization |
| **Taxonomy** | *Where does it sit?* | Operating Expenses → sub-category of profitability metrics |
| **Ontology** | *What does it affect?* | It influences corporate valuation and loan covenant compliance |
| **Lineage** | *How was it calculated?* | Pulled from Snowflake; added `Operating_Income + Depr_Amort` |
| **Provenance** | *Can I legally trust it?* | Generated by SAP ERP on June 1st at 04:00 UTC, signed by Corporate Controller |

---

## 5. The Semantic Layer for GenAI Agents

The Semantic Layer is the core translator within the broader Knowledge Fabric. It acts as an abstraction layer positioned **directly between raw technical data infrastructure and the AI agents querying it**. [1, 2]

```
[ Raw Data Sources ]  →  [ SEMANTIC LAYER ]  →  [ GenAI / Agents ]
(Tables, Schemas, IDs)   (Translates code into    (Reasons over exact
                          Business Definitions)    Business Concepts)
```

Instead of forcing an agent to interact with cryptic database definitions (joining `tbl_usr_v2` and `rev_curr_90`), the semantic layer maps technical assets to machine-readable business concepts and standard mathematical metrics. [1, 2, 3, 4, 5]

**Is the semantic layer required for GenAI agents? Yes — absolutely.** Industry consensus views it as critical infrastructure, not a BI convenience feature. [1, 6, 7] There are four reasons:

**1. Eliminating "Fluent but Wrong" Hallucinations.** When a GenAI agent writes its own Text-to-SQL, it frequently hallucinates joins, misinterprets column names, or pulls gross revenue instead of net revenue. [2, 3, 14] With a semantic layer, the agent requests the pre-calculated, verified concept of `[Revenue]` and receives mathematically vetted SQL. Industry tests show that grounding an LLM in a structured semantic graph can make outputs up to **three times more accurate** than querying a database schema directly. [1, 2, 9, 16, 17]

**2. Models Need Meaning, Not Just Math.** A standard LLM looks at database column names and guesses. The semantic layer acts as a *Rosetta Stone*, mapping metadata and business definitions into a protocol the AI can navigate. [1, 2, 9, 16, 17]

**3. Centralized Governance and Security.** A semantic layer acts as a gatekeeper. Security policies, data masking, and column-level access permissions are encoded directly into the semantic concepts — not scattered across individual agent code blocks. If a customer service agent asks for user data, the semantic layer strips PII before the agent ever sees it. [2, 19, 20, 21]

**4. System Interoperability via MCP.** Modern architectures use the **Model Context Protocol (MCP)**, allowing a single centralized semantic layer to expose metrics and entities as "tools." Any MCP-compatible AI agent — built on Anthropic, OpenAI, or Microsoft — can automatically discover, query, and inherit those definitions without custom integration. [1, 2, 22, 23, 24]

### Financial Semantic Layer Design

For financial AI agents, the semantic layer requires an ontology-driven model with three structural tiers:

**Financial Glossary (Ground Truth)** — Machine-readable definitions including formulas (e.g., `Gross Profit = Revenue - COGS`), fiscal calendar variations (4-4-5 week vs. calendar month), and explicit boundary conditions. [2, 3, 4, 5]

**Financial Taxonomy (Data Aggregator)** — Hierarchical structures enabling drill-down and roll-up: Chart of Accounts (`Assets → Current Assets → Cash & Equivalents`), organizational dimensions (`Region → Country → Subsidiary → Cost Center`), and time hierarchies (`Fiscal Year → Quarter → Month → Week`). [7, 8, 9]

**Financial Ontology (Reasoning Engine)** — Multi-directional relationships connecting transactions to customers, contracts, product lines, and salespeople. Logical business constraints that agents can query. [13, 14, 15]

| Metric Component [16, 17, 18] | Glossary Role | Taxonomy Role | Ontology Role |
|---|---|---|---|
| **"Revenue"** | Defines standard recognition rules | Places it under "Income" in P&L | Links it to "Contract Value" and "Invoices" |
| **"Cost Center"** | Defines operational ownership | Places it under "North American Ops" | Connects it to specific GL accounts and employees |
| **"Exchange Rate"** | Defines daily spot vs. average rate | Groups it under "Market Data → Forex" | Applies it dynamically to convert transactions by date |

---

## 6. Knowledge Fabric Architecture

### 6.1 What Is a Knowledge Fabric?

A **Knowledge Fabric for GenAI** is an advanced intelligence layer that bridges the gap between raw enterprise data and Large Language Models. [1, 2] While a *Data Fabric* connects fragmented technical infrastructure, a *Knowledge Fabric* **translates that data into human-like context, meaning, and business logic** — a unified semantic map that allows AI agents and copilots to understand how an entire business operates. [1, 3, 4, 5, 6]

```
[ Disjointed Systems ]  →  [ DATA FABRIC ]  →  [ KNOWLEDGE FABRIC ]  →  [ GenAI / Agents ]
(SharePoint, CRM, SQL)     (Connects Pipes)     (Adds Business Logic,    (Delivers Accurate,
                                                 Context & Meaning)       Context-Aware Answers)
```

Key pillars of a Knowledge Fabric: [1, 7, 8, 9]
- **Semantic Linking & Ontologies** — connects data by real-world meaning, not just relational keys
- **Live Context at the Source** — connects directly to repositories like SharePoint or Salesforce, automatically chunking and vectorizing as data changes
- **Governed Multi-Hop Reasoning** — traverses relationships across organizational silos while enforcing permissions and compliance rules at every step
- **Traceable Provenance** — every AI output is tagged with metadata detailing its lineage, version, and original source

### 6.2 Enterprise Knowledge Fabric Blueprint

A production-grade, enterprise-scale Knowledge Fabric cannot rely on a single database or a single RAG pipeline. It requires a **decoupled, multi-layered platform** that abstracts data storage, standardizes knowledge governance, and exposes a unified API to LLMs and agents.

```
[ Data Source Layer ]     → Structured (SQL, ERP), Unstructured (PDFs, S3),
                            Semi-Structured (NoSQL, Logs)
         │
[ Ingestion & CDC ]       → Apache Kafka / Debezium / Spark
                            (Real-time & Batch Streaming)
         │
[ Processing & Extract ]  → OCR, LayoutLM, Entity-Relation Extraction, Embeddings
         │
[ Unified Storage Layer ] → Vector Database + Graph Database + Operational Lakehouse
         │
[ Fabric Governance ]     → Ontologies, W3C Semantic Web Standards (RDF/OWL/SHACL),
                            Role-Based Access Control (RBAC)
         │
[ Semantic API Layer ]    → GraphQL / REST Semantic Gateway with Prompt/KV Caching
         │
[ Consumption Layer ]     → Agentic RAG, Cognitive Search, Analytics, Fine-Tuning Pipelines
```

**Layer 1 — Ingestion and Change Data Capture (CDC)**

Enterprise data is constantly changing. The fabric must handle both static historical data and real-time operational updates. Use **Apache Kafka** or **Apache Flink** as the streaming backbone. Implement **CDC tools like Debezium** on top of legacy databases so that when an ERP row changes, a Kafka event triggers pipeline updates to both vector and graph stores. Standardize on **Apache Iceberg** or **Delta Lake** formats within cloud storage (S3/ADLS) for unified batch storage before processing.

**Layer 2 — Multimodal Extraction and Processing Pipeline**

Unstructured files contain structural metadata (tables, headers) that standard character-based chunking destroys. Use layout-aware tools like **Unstructured.io** or **LlamaParse** to extract hierarchical structures (Markdown, HTML tables, document hierarchies). For images, engineering diagrams, or scanned PDFs, employ **LayoutLMv3** to extract embedded text and visual tables. Use LLM-as-an-Extractor patterns via Instructor/Pydantic running on cost-efficient open-source models (e.g., Llama-3-8B) to systematically pull semantic concepts from raw text.

**Layer 3 — Storage and Indexing Layer (The Dual-Engine Core)**

To handle both structured rules and unstructured concepts, a **Hybrid Knowledge Store** is required:
- **The Graph Engine (Fabric Backbone)** — use a property graph or RDF store (Neo4j, GraphDB, Amazon Neptune) to map hard corporate dependencies, permissions, and ontologies
- **The Vector Engine** — use a highly scalable vector database (Milvus, Qdrant, Pinecone) to store semantic chunk embeddings
- **Entity-to-Vector Mapping** — every chunk in the vector database must contain a metadata foreign key pointing back to its corresponding node or entity ID in the graph database

**Layer 4 — Enterprise Standards and Governance Layer**

This layer turns a disorganized "data swamp" into a compliant corporate asset. Enforce W3C open standards (RDF, OWL, SHACL) for domain ontologies to guarantee consistent definitions of "Customer" or "Revenue" across all business lines. Utilize **OpenLineage** for data lineage tracking. Apply **Attribute-Based Access Control (ABAC)** via JWT security tokens passed down to vector and graph storage queries for hard boolean metadata filtering at search time.

**Layer 5 — Unified Semantic Orchestration Layer (The API)**

LLMs and agents should never query raw databases directly. Expose a unified **federated GraphQL / Semantic Search API** (e.g., Hasura or Apollo) that accepts natural language queries, rewrites them, and securely fetches data. Implement a **Dynamic Query Router**: aggregation queries ("What were total sales?") route to SQL/Cypher; conceptual queries ("Why did sales dip?") route to the Vector RAG system.

**Layer 6 — Expanded Enterprise Components**

The core layers above must be augmented by:

- **Identity & Access Management (IAM) Layer** — a policy enforcement point integrated with enterprise identity providers (Okta, Microsoft Entra ID) that applies Document-Level Security (DLS) before documents reach any AI component. [1, 2, 3]
- **Semantic Cache Engine** — a high-speed cache (e.g., Redis) that evaluates mathematical distance between incoming queries and historical queries, bypassing the entire RAG network for closely matched prior queries. [4, 5, 6, 7]
- **Observability, Lineage & Evaluation Layer (LLMOps)** — a platform (Arize Phoenix, Langfuse, Datadog LLM Observability) that logs data lineage, monitors latency, and continuously evaluates production outputs for Faithfulness and Answer Relevance. [14]

**Multi-Tier Semantic Caching**

Three-layered caching infrastructure is required for cost control at enterprise scale:
- **Semantic Prompt Caching** — serve identical or near-identical queries from cache
- **KV-Cache (Model Server Side)** — use vLLM or similar inference engines to cache system prompts and large corporate guidelines in GPU memory
- **Graph Caching** — cache common sub-graph queries to reduce traversal latency

**Auto-Evaluation Pipeline (CI/CD for Knowledge)**

Enterprise data degrades. Use synthetic data generation (LLM-generated hypothetical user questions from latest documents) combined with frameworks like **Ragas** or **TruLens** to automatically grade the pipeline on three metrics: Context Relevance, Groundedness (Hallucination Rate), and Answer Relevance.

### 6.3 Modular RAG Combinations for the Fabric

To support a diverse enterprise Knowledge Fabric, you need **one Modular RAG framework** designed around three layers — not eight separate systems. [7, 13, 24]

```
                  [ USER QUERY ]
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 1: Routing & Orchestration (Adaptive RAG)         │
│  Classifies query intent, dataset scope, and complexity  │
└───────────────────────┬──────────────────────────────────┘
                        │ (Route based on complexity)
        ┌───────────────┼────────────────┐
        │               │                │
[Low Complexity]  [High Complexity]  [Multi-Step/Tool]
        │               │                │
  Direct LLM     Semantic Vector    Agent Loop
  Generation         Search         (RAG-Agency)
        └───────────────┴────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 2: Execution Agents (RAG-Agency / ComoRAG)        │
└───────────────────────┬──────────────────────────────────┘
                        │ (Retrieve and execute)
                        ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 3: Verification & Guardrails (Self-RAG / CRAG)    │
└──────────────────────────────────────────────────────────┘
```

**The three best RAG combinations** for enterprise workloads:

**The Cost-Efficient Powerhouse: Adaptive RAG + Self-RAG** — Adaptive RAG acts as the front gate, routing simple queries directly to the LLM and complex queries to Self-RAG's intensive reflection loop for zero-hallucination guarantees. [7, 17, 18, 19, 20]

**The Flawless Knowledge Worker: RAG-Agency + CRAG** — An autonomous agent (RAG-Agency) executes multi-step research tasks while CRAG continuously validates retrieved chunks, intercepting bad data and re-querying fallback sources before the agent proceeds. [7, 10, 11, 21, 22]

**The Deep Researcher: ComoRAG + Lean RAG** — Lean RAG optimizes knowledge pathways into memory-efficient semantic structures; ComoRAG reads these structures to conduct deep long-form analysis without hitting context window limits. [23]

Use case mapping for the Knowledge Fabric:

| RAG Technique [7, 8, 9, 10, 11, 12] | Core Optimization | Best Enterprise Use Case |
|---|---|---|
| **Corrective RAG** | Eliminating stale/missing local knowledge | Customer Support: auto-trigger web search when local docs lack the answer |
| **Self-RAG** | High factuality and source alignment | Legal & Compliance Auditing: ensure answers match policies verbatim |
| **Adaptive RAG** | Balancing cost with response complexity | Organization-wide Intranet Search |
| **Lean RAG** | High performance on massive datasets | Supply Chain & IoT Analytics |
| **ComoRAG** | Multi-step reasoning across long documents | R&D and Intellectual Property review |
| **REX RAG** | Handling ambiguous/dead-end reasoning | Financial Fraud & Risk Assessment |
| **RAG-Agency** | Multi-turn autonomous task execution | Business Intelligence automation |
| **RAG 3.0** | Native multi-modality, zero-hallucination | Enterprise Executive Dashboards |

---

## 7. Open Standards: OKF and Data Contracts

### 7.1 Google Cloud's Open Knowledge Format (OKF)

The enterprise AI world has long suffered from a **fragmented context landscape**: every time you build a new agent, you have to reassemble internal context from scratch — from scattered metadata catalogs, wikis, shared drives, code comments, and senior engineers' heads. [1-1]

On June 12, 2026, Google Cloud introduced the **Open Knowledge Format (OKF)** — an open specification that formalizes the LLM-wiki pattern (popularized by Andrej Karpathy) into a portable, interoperable format. [4-1, 5-1] OKF is a vendor-neutral, agent- and human-friendly standard for representing the metadata, context, and curated knowledge that modern AI systems need.

**How OKF works:**

An OKF bundle is a directory of markdown files representing concepts — anything you want to capture, including tables, datasets, metrics, playbooks, runbooks, and APIs. Each concept is one file. The file path is the concept's identity. A bundle is portable: shippable as a tarball, hostable in any git repo, mountable on any filesystem. No SDK, no runtime, no proprietary account required.

OKF is an open, human- and agent-friendly format for representing knowledge — the metadata, context, and curated insight that surrounds data and systems. It is designed to be authored by people, generated by agents, exchanged across organizations, and consumed by both. The format is intentionally minimal: a directory of markdown files with YAML frontmatter. There is no schema registry, no central authority, and no required tooling.

**The OKF document structure** is extremely minimal by design. Every concept file has two parts: a YAML frontmatter block and a free-form Markdown body. The only required field is `type`:

```yaml
---
type: <Type name>                  # REQUIRED
title: <Optional display name>
description: <Optional one-line summary>
resource: <Optional canonical URI>
tags: [<tag>, <tag>]               # Optional
timestamp: <ISO 8601 datetime>     # Optional
---

... free-form markdown body with cross-links to other concepts ...
```

The distinction from RAG is useful for developers. RAG re-derives knowledge at query time from raw chunks. An OKF bundle stores curated, cross-linked concepts that an agent reads and updates directly.

**OKF in the Knowledge Fabric context:**

OKF is the missing **portability standard** for the unstructured knowledge layer of a Knowledge Fabric. Concretely:
- Export BigQuery table and metric definitions as an OKF bundle and commit it alongside the SQL it describes — business logic under version control
- Store incident runbooks as OKF concepts; an on-call agent reads `index.md`, follows cross-links, and resolves the join path it needs
- Enable cross-organizational knowledge exchange: a vendor ships a catalog export as OKF; your agent consumes it with no integration work
- Replace stale Notion or Obsidian wikis with version-controlled markdown that an agent keeps current

OKF is published as version 0.1 with status Draft. It is a starting point, not a finished standard — Google is publishing in the open from day one so the ecosystem of producers and consumers can grow.

### 7.2 Google Cloud Knowledge Catalog

While OKF is the open format specification, **Google Cloud Knowledge Catalog** (formerly Dataplex Universal Catalog, renamed April 10, 2026) is a production implementation of the broader Knowledge Fabric vision. [13-1]

Traditional data catalogs were built as manual inventories for technical users, focusing on table structures rather than the deep context that AI agents need. When agents lack business semantics and data relationships, this triggers hallucinations, high latency, and stale insights. To address this problem, we are evolving Dataplex into a dynamic, always-on Knowledge Catalog that serves as the universal context engine for your enterprise, helping agents execute complex tasks with accuracy.

**Core capabilities of Knowledge Catalog:**

Knowledge Catalog aggregates native context across your Google and partner data platforms, semantic models, and third-party catalogs, unifying them into a single, governed source of truth. It automatically harvests technical metadata from BigQuery, AlloyDB, Spanner, Cloud SQL, Firestore, and Looker. It also integrates with third-party catalogs like Atlan, Collibra, Datahub, Ab Initio, and Anomalo.

Verified queries and semantic guardrails: One leading cause of AI failure is hallucinated logic and guessed SQL joins. To prevent this, the catalog provides verified SQL patterns and pre-generated natural language questions.

One of the most common reasons AI agents fail or hallucinate is that they guess SQL joins or column definitions. Knowledge Catalog provides verified SQL patterns, semantic guardrails, and central business glossaries. Instead of guessing the unwritten rules of a database, the agent references pre-validated logic, resulting in precise and deterministic data retrieval. AI agents don't use data like humans do; they need real-time, high-precision retrieval. Knowledge Catalog integrates with the Model Context Protocol (MCP), allowing AI agents to perform semantic searches and fetch highly relevant organizational context in milliseconds.

The LookML Agent autonomously reads your strategy documents to instantly generate business-ready semantics. By aggregating these semantic models into the Knowledge Catalog, it federates your core business logic across the enterprise, ensuring agents reason using the same definitions as your analysts.

The Knowledge Catalog has been updated to ingest OKF bundles and serve them to agents, making it both a consumer and producer in the OKF ecosystem. For organizations running on Google Cloud with Looker as their semantic layer, Knowledge Catalog provides a particularly compelling path to a production Knowledge Fabric. Google is recasting its data and analytics portfolio as the Agentic Data Cloud, an architecture aimed at moving enterprise AI from pilot to production by turning fragmented data into a unified semantic layer that agents can reason over and act on more reliably at scale.

### 7.3 Data Contract Standard vs. OKF

OKF and Data Contract Standards (such as the Open Data Contract Standard / ODCS, championed by the Linux Foundation's Bitol organization) are **fundamentally different tools designed for distinct purposes**. They do not compete; they complement. [1, 2, 3]

| Feature [2, 4, 5, 7, 8, 9, 10, 11, 12, 13] | OKF | Data Contract Standard (ODCS) |
|---|---|---|
| **Primary Audience** | AI Agents, LLMs, Human Collaborators | Data Engineers, Software Engineers, CI/CD Tools |
| **Primary Goal** | Providing qualitative business context and tribal knowledge | Enforcing schema consistency, quality rules, and SLAs |
| **File Format** | Markdown + YAML frontmatter (.md) | Pure YAML or JSON (.yaml) |
| **Enforcement** | Permissive — unknown fields degrade gracefully | Strict — breaks pipelines, triggers alerts, blocks deployment |
| **Architecture** | A file-directory graph of linked Markdown concepts | A single structured API-style document per data product |

**OKF** provides the human-and-AI-readable knowledge layer — the *map and dictionary* of the data world. [8, 14] **Data Contracts** standardize the *plumbing and rules* of the data pipeline, ensuring a schema change by an upstream team doesn't silently break a downstream analytics dashboard. [4, 23, 24]

In a Knowledge Fabric, both should coexist: Data Contracts govern the ingestion pipeline's structural integrity while OKF bundles serve as the semantic context layer that AI agents query at reasoning time.

---

## 8. Financial GenAI Agents: A Reference Architecture

### 8.1 Combining Semantic Layer and Unstructured Knowledge

In real-world enterprise scenarios, a financial AI agent must span both structured metrics and unstructured documents to answer any non-trivial query. When asked, *"What was our Q1 revenue, and how did the CEO explain the change in software margins?"*, the agent must:

1. **Query the Semantic Layer** (via the governed financial ontology and Looker/dbt) for exact, audited revenue figures
2. **Query the Knowledge Base** (via RAG over earnings call transcripts and SEC filings) for qualitative management commentary

The key benefit of this architecture: the semantic layer **eliminates LLM hallucinations on numbers** (it handles the math), while the knowledge base **provides the narrative** behind the numbers. [2, 12, 15, 16, 17]

**Core architecture:**

- **Orchestration / Router** — evaluates user intent and decides whether a question requires numerical computation, qualitative document analysis, or both [6, 7, 8, 9]
- **Semantic Layer Tool** — handles governed calculations (EBITDA, Gross Margin) via pre-defined, version-controlled metric APIs; the agent never writes raw SQL [2, 10, 11]
- **Knowledge Base / RAG Tool** — handles unstructured files (SEC filings, analyst research, earnings transcripts) via semantic vector search [12, 13, 14]
- **Governed Access** — the semantic layer enforces RBAC, ensuring agents cannot fetch metrics unauthorized for the current user [2, 16, 18, 19]

### 8.2 Amazon Strands Agents SDK Implementation

Using the **AWS Strands Agents SDK** as the orchestration framework, the architecture takes shape as follows. Strands relies on a model-driven approach where you define tools and prompts, and the foundation model (Claude or Amazon Nova on Amazon Bedrock) handles dynamic orchestration, tool chaining, and the execution loop. [1, 2, 3, 4, 5]

**High-Level Reference Architecture:**

```
                    [ ENTERPRISE USER / APP INTERFACE ]
                                │ (HTTPS / WebSockets)
                                ▼
                     [ AMAZON BEDROCK AGENTCORE ]
                     (Runtime, Handoffs & State Mgmt)
                                │
                                ▼
                   [ STRANDS AGENTS SDK ORCHESTRATOR ]
                    (Supervisor, Swarms, & Router Graphs)
                                │
           ┌────────────────────┴────────────────────┐
           │ (MCP Tool Invocation)                   │ (MCP Tool Invocation)
           ▼                                         ▼
[ CUSTOM MCP SERVER: LOOKER ]          [ CUSTOM MCP SERVER: KNOWLEDGE BASE ]
  (Looker SDK / LookML)                  (Semantic Vector Search)
           │                                         │
           ▼                                         ▼
[ STRUCTURED DATA CORE ]               [ UNSTRUCTURED DATA CORE ]
  (Amazon Redshift / Snowflake / RDS)    (Amazon OpenSearch / S3)
  - Verified Financial Metrics           - SEC Filings, PDFs
  - Pre-compiled LookML Explores         - Bedrock Knowledge Bases
```

**Component Selection:**

**Orchestration & State Management** — use the **Amazon Strands Agents SDK** (open-source). Build a **Supervisor-Collaborator network** using Strands' built-in `GraphBuilder` or `agent_graph` utilities, deploying specialized, isolated agents (Portfolio Analyzer, Risk Compliance, FinOps Cost) to prevent system prompt bloat. [1, 12, 13, 14, 15] Host production runtime via **Amazon Bedrock AgentCore** for serverless enterprise-grade execution, session persistence, and multi-agent handoffs. [1, 10, 15, 16]

**The Structured Path: Looker MCP Server** — agents must not write raw Text-to-SQL. Build a **Custom Looker MCP Server** using Python's `mcp` library or the FastMCP framework [2, 20, 21], exposing existing Looker business definitions as MCP tools. When an agent needs "Gross Margin by region," it invokes the Looker MCP tool, which generates the pre-audited SQL, executes it against Redshift or Snowflake, and returns clean JSON. Financial calculation logic lives entirely in LookML, isolated from model drift. [25, 26, 27, 28]

**The Unstructured Path: RAG MCP Server** — pair the Strands Agent with **Amazon Bedrock Knowledge Bases** and **Amazon OpenSearch Service** (serverless vector engine). [10, 30, 31, 32, 33] Use Bedrock's automated chunking and parsing pipelines to ingest unstructured files from Amazon S3. Build a thin custom RAG MCP server that triggers OpenSearch hybrid/semantic retrieval and passes the top retrieved snippets back to the Strands reasoning loop with full source metadata. [1, 2, 10, 35, 36]

**Conceptual Python code (simplified):**

```python
import boto3
from strands import Agent, tool

bedrock_agent_runtime = boto3.client("bedrock-agent-runtime")

@tool
def query_semantic_layer(metric_name: str, dimensions: dict = None) -> str:
    """
    Fetches strictly governed financial metrics from the semantic layer.
    Use this for exact numbers like revenue, EBITDA, or margins.
    """
    # Map metric_name to the Looker SDK API (pre-audited LookML definition)
    return f"Metric: {metric_name}. Value: $4.2B. Period: Q1 2026. Status: Audited."

@tool
def search_unstructured_wiki(query: str) -> str:
    """
    Searches the unstructured knowledge base (PDFs, notes, reports, transcripts).
    Use this to understand the 'why', qualitative context, and narrative.
    """
    response = bedrock_agent_runtime.retrieve(
        knowledgeBaseId="YOUR_KB_ID",
        retrievalQuery={"text": query}
    )
    results = [item["content"]["text"] for item in response["retrievalResults"]]
    return "\n---\n".join(results)

finance_agent = Agent(
    model="bedrock/anthropic.claude-sonnet-4-6",
    system_prompt=(
        "You are an expert financial analyst. "
        "ALWAYS fetch concrete numbers from 'query_semantic_layer'. "
        "NEVER hallucinate numbers. Use 'search_unstructured_wiki' for qualitative context. "
        "Synthesize both into a cohesive, cited answer."
    ),
    tools=[query_semantic_layer, search_unstructured_wiki]
)
```
[1, 14]

**Governance & Guardrails** — attach **Amazon Bedrock Guardrails** to strip PII, detect prompt injections, and block prohibited topics at the model wrapper level. Pass user Cognito/IAM OAuth tokens through to the Looker MCP tool to enforce row-level security at the database level. Enable advanced parsing in Bedrock Knowledge Bases for multimodal document processing. [10, 15, 37, 38]

**Tracing & Observability** — Strands has native **OpenTelemetry** support. Route telemetry to Jaeger, Grafana Tempo, or Arize AX. Every agent "thought" and tool invocation is logged for complete auditability. [12, 35, 39, 42]

---

## 9. SOX Compliance in Agentic AI Systems

### 9.1 Provenance as the SOX Foundation

**Data provenance is directly related to and foundational for Sarbanes-Oxley (SOX) compliance.** [1] SOX Sections 302 and 404 require public corporations to establish internal controls guaranteeing that financial reports are accurate, tamper-proof, and verifiable. Because provenance provides the unalterable "birth certificate" and ownership record of financial data, it serves as the primary technical proof auditors look for to verify these controls. [2, 3, 4, 5, 6]

SOX compliance mandates in three critical areas: [7, 8, 9, 10]

**Verifying Segregation of Duties (SOX 404)** — the same person who creates a journal entry cannot approve it. Provenance records explicit user IDs and system timestamps for every state change, providing immediate proof of who created and who approved each ledger entry. [11, 12, 13]

**Enforcing Data Integrity and Non-Tampering** — if a database administrator attempts to overwrite an invoice amount directly in the database (bypassing the ERP application), the provenance hash or metadata trail will break, alerting the system to unauthorized change. [14, 15, 16]

**Accelerating the Audit Trail** — while lineage shows the pipeline path, provenance proves that data sitting in the final balance sheet originated from a verified, authorized banking ledger API rather than an unverified CSV upload. [17, 18]

**The Risk of AI Agents without Provenance Controls:** [19]
- **The "Black Box" Loophole** — if an AI agent auto-generates a journal entry, auditors must see the provenance of the prompt, the model version used, and the underlying data inputs. [20]
- **Unauthorized Model Actions** — provenance acts as a guardrail; if an agent attempts to read a low-provenance spreadsheet to adjust a forecast, the SOX control layer blocks the agent because the source lacks an approved corporate digital signature. [21]

**Treating SOX Controls as Ontological Rules:**

Your ontology can encode constraints such as: *"If an Entity is a `SOX_Scope_Financial_Metric`, its Provenance must map exclusively to a `System_of_Record` (e.g., SAP, NetSuite) and possess an `Authorized_Controller_Signature` before an AI agent can use it in a public report."*

### 9.2 SOX Control Injection into the Architecture

Enforcing SOX compliance in a generative AI financial ecosystem is a rigorous data security, deterministic code execution, and change management challenge. Because AI models are inherently non-deterministic, traditional "point-in-time" IT controls fail. Under modern audit expectations — particularly the **COSO Internal Control over GenAI framework** — the architecture must guarantee data integrity, strict Segregation of Duties (SoD), and an unalterable, human-verifiable audit trail. [1, 3, 4]

**Control 1 — Deterministic Control over Financial Metrics (COSO Principle 13)**

Restrict Strands Agents from writing code, executing calculation loops, or guessing math formulas. [1, 5, 6, 7, 8] The **Looker MCP Server** acts as the primary SOX data control layer. When an agent needs financial data, it can *only* call a hardcoded, unalterable Looker metric via the MCP tool definition. Looker converts the call into the pre-audited LookML SQL. The Looker MCP Server must log the specific **LookML View/Explore ID** and the exact SQL compiled by the Looker engine for every tool call. [1, 9]

**Control 2 — Immutable Cryptographic Audit Trail (Section 404)**

Tap into Amazon Strands' native **OpenTelemetry telemetry hooks**. Route all agent execution traces through a pipeline that signs and writes raw telemetry to a **WORM (Write Once, Read Many) Amazon S3 bucket** with Object Lock enabled. [13, 14] Log the following for every execution: the initial user identity and prompt; the model's step-by-step reasoning tokens; the exact context payloads from Looker and RAG MCP servers; and the exact citations or database rows utilized. [9]

**Control 3 — Segregation of Duties & Token Pass-Through (COSO Principle 3)**

Use **OAuth 2.0 token propagation** across the entire architecture stack. The user's validated IAM/Cognito JWT token must be passed inside the MCP tool context to both the Looker and RAG servers. Looker evaluates its native user-level row security, ensuring the agent only receives data the logged-in user is explicitly authorized to view. [13, 15]

**Control 4 — Human-In-The-Loop (HITL) for Actionable Workflows**

GenAI can assist and draft, but it cannot authorize or sign off on financial actions autonomously under SOX. [15, 17] Implement a **state machine interruption pattern** in Strands Agent Core: if an agent determines an action step is required (transferring funds, adjusting journal entries, publishing financial updates), it writes a pending state record to an internal ledger and halts execution. The system surfaces an "Approve/Reject" interface to an authorized manager. The agent cannot proceed until it receives a cryptographically signed human authorization token. [19]

**Control 5 — Document Provenance for Unstructured Data (RAG Controls)**

The RAG MCP server must return metadata fields containing the absolute file path, unique **SHA-256 document hash**, and exact page/chunk offset alongside the text payload. The Strands agent must format its final output to explicitly cite these metadata points, providing an ironclad digital audit trail. [9, 10] The Bedrock Knowledge Base must pull exclusively from a highly restricted, read-only S3 bucket acting as the **Financial Document Registry**. [2, 20]

**SOX Architectural Verification Matrix for Auditors:**

| SOX Control Objective [1, 17, 21, 22] | System Implementation | Compliance Evidence Generated |
|---|---|---|
| **Data Integrity & Accuracy** | Looker MCP Server (LookML Metrics) | Pre-compiled, immutable LookML SQL code logs |
| **Completeness of Audit Trail** | Strands OpenTelemetry + S3 Object Lock | Signed, unalterable log files of the model's exact reasoning path |
| **Access Authorization** | Cognito / IAM OAuth Token Propagation | API gateway and database logs stamped with individual user IDs |
| **Preventative Guardrails** | Bedrock Guardrails + Text-to-SQL Ban | System configurations proving the model is blocked from raw SQL |
| **Human Accountability** | Strands HITL Interruption State | Signed human approval logs tied to transaction IDs |

### 9.3 Dual-Identity Tracing: Users and Agents

For SOX compliance, logging the **Agent Identity** alongside the human user's identity is equally critical. In a modern financial agent ecosystem, a single human request may trigger a cascade of autonomous tasks across multiple specialized worker agents. If a financial anomaly or unauthorized data exposure occurs, auditors will demand to see which autonomous component executed each specific action. [3]

**Why Agent Identity is Mandatory:**

Under the **COSO Framework for AI**, organizations must demonstrate control over how automated decisions are made. If an agent routes an expense audit or interprets a contract clause, the auditor needs to know *which version* of *which specific agent definition* made that choice. [4]

Logging agent identity also enables detection of **prompt injection attacks** — by logging every agent in the telemetry chain, a SIEM system can instantly detect if a worker agent is suddenly executing actions outside its programmed scope. [5]

AI agents are software assets. They change when system prompts are updated or model parameters are adjusted. Logging the precise **Agent ID + Agent Version** proves to auditors that the system ran under approved, change-managed configurations at a specific point in time. [6, 7, 8, 9]

**The SOX Identifiers Tuple** — every log entry must contain:
- `acting_user_id` — the immutable human user identifier (e.g., Cognito sub-claim)
- `agent_component_id` — the specific agent executing the current step (e.g., `portfolio_risk_agent_v2`)
- `agent_configuration_hash` — a SHA-256 hash of the agent's system prompt, parameters, and environment settings
- `parent_span_id` / `trace_id` — OpenTelemetry IDs that stitch the entire multi-agent conversation into a single cohesive timeline [10, 11]

**Example composite SOX log entry:**

```json
{
  "trace_id": "4bf92f3577b34da6a3ce929d0e0e4736",
  "timestamp": "2026-06-17T14:15:22Z",
  "identities": {
    "human_user": "user_id_john_doe_993",
    "active_agent": "regulatory_compliance_worker_agent",
    "agent_version": "v1.4.2",
    "agent_config_sha256": "e3b0c44298fc1c149afbf4c8996fb924..."
  },
  "action": {
    "type": "mcp_tool_invocation",
    "mcp_server": "bedrock_knowledge_base_rag",
    "tool_called": "search_sec_filings",
    "payload_sent": { "query": "Q2 revenue risks", "document_scope": "2026_10Q" }
  }
}
```

### 9.4 COSO & ISACA-Aligned SOX Checklist

The **Committee of Sponsoring Organizations of the Treadway Commission (COSO)** released the first formal, authoritative framework extending traditional internal controls specifically to Generative AI. [1] Because GenAI outputs are probabilistic (non-deterministic), **COSO states that AI outputs must be treated as "assertions requiring validation" rather than facts.** External auditors use the COSO framework, often combined with **ISACA ITAC (IT Automated Controls)** guidelines, to evaluate whether an AI ecosystem meets SOX Section 404 requirements. [1, 2]

---

**📋 1. Control Environment & Lifecycle Governance (COSO Principle 1)**

- [ ] **Use-Case Registry** — catalog the entire multi-agent ecosystem in a formal corporate AI inventory detailing each agent's purpose, scope of authority, and underlying foundational models. [3, 4, 5]
- [ ] **Assigned Control Owners** — maintain a RACI matrix assigning human ownership over specific prompt libraries, LookML Explores, RAG vector embeddings, and system routing logic. [4, 6]
- [ ] **Model Parameter Baseline** — document and freeze baseline configurations (e.g., `temperature=0` for financial reasoning) under formal IT change management. [7]

**🔒 2. Data Ingestion & RAG Data Integrity (COSO Principle 13)**

- [ ] **Source Provenance Verification** — cryptographically verify integrity of all documents ingested into S3 for the RAG pipeline using SHA-256 hashing. [8, 9, 10]
- [ ] **Automated Vector Sync Tracking** — implement automated system logs documenting the exact execution timestamp and file list for every OpenSearch vector pipeline run. [11]
- [ ] **Data Leakage Prevention** — verify the RAG ingestion mechanism cannot absorb or cache unauthorized customer PII or restricted financial information. [6]

**⚙️ 3. Logical Access & Segregation of Duties (ISACA ITAC Domain 3)**

- [ ] **Dual-Identity Context Token Pass-Through** — test and prove that individual user Cognito/IAM authentication tokens propagate into the custom MCP tool blocks. [2, 11]
- [ ] **Enforced Row-Level Security** — audit Looker's access control layers to confirm that unauthorized users are blocked at the database level. [2]
- [ ] **Agent Escalation Guardrails** — enforce logic bounds ensuring a worker agent cannot execute an API call with permissions higher than those held by the human user who triggered the session. [2]

**📊 4. System Processing & Deterministic Boundaries (ISACA ITAC Domain 2)**

- [ ] **Hardcoded Tool Constraints** — prove Strands agents are restricted from generating raw Text-to-SQL or executing direct Python calculations on unstructured financial parameters.
- [ ] **Pre-Audited Metric Binding** — verify the model can only pull financial metrics (Gross Revenue, EBITDA) by invoking vetted, version-controlled LookML definitions.
- [ ] **Exception Handling & Error Logging** — ensure processing errors inside MCP servers are recorded as flagged exception logs. [2, 13]

**📝 5. Information, Communication & Auditability (COSO Principle 14)**

- [ ] **WORM Audit Trail Architecture** — stream all system logs and Strands OpenTelemetry execution traces into an Amazon S3 bucket with Object Lock enabled.
- [ ] **Composite Identity Stamps** — ensure every log entry stamps the Human User ID, Agent Name, Agent Configuration Version, and Unified Trace ID simultaneously.
- [ ] **AI Output Citations** — force downstream worker agents to format final answers with strict reference citations pointing to the specific Looker View Name or S3 document path/offset. [8, 9, 10, 14]

**🛑 6. Continuous Monitoring & Human Review (COSO Principle 16)**

- [ ] **Human-In-The-Loop (HITL) Interruption** — implement a mandatory state-machine hard stop for any transactional agent behavior, requiring human sign-off.
- [ ] **Model Drift Tracking** — establish automated threshold monitoring using OpenTelemetry parameters to alert compliance teams if the model's response accuracy falls below baseline.
- [ ] **Prompt Injection Remediation Workflows** — route anomalous prompt input flagged by Amazon Bedrock Guardrails to a specialized security queue with a contractual SLA for remediation. [4, 7, 15]

**Presenting to External Auditors**

Using the Grant Thornton GenAI Audit Guidelines, provide your compliance team with: [9, 10]
- **LookML schema code history** in GitHub to prove change management over financial calculation rules
- **S3 Object Lock policy configuration** to prove trace logs are unalterable
- A **sample execution token log** tracing a query from a human user prompt through to a Looker MCP tool call and back

---

## 10. LLMOps: Observability, Lineage, and Evaluation

An enterprise Knowledge Fabric running in production is a living system. Without deep observability, a multi-agent RAG architecture is effectively a black box. LLMOps — the operational discipline for AI systems — must be treated as a first-class architectural component alongside the data and knowledge layers.

**Key LLMOps capabilities:**

**Distributed Tracing** — every tool invocation, routing decision, retrieval query, and generation step must emit OpenTelemetry spans. These spans stitch together into a unified trace for every user session, enabling end-to-end debuggability. Tools: Jaeger, Grafana Tempo, AWS X-Ray, Arize AX.

**Data Lineage** — every AI response must be traceable back to the exact document chunk, database row, or graph node that contributed to it. Platforms like Arize Phoenix and Langfuse record chunk attribution alongside latency metrics.

**Evaluation Metrics** — continuous evaluation against the RAG Triad:
- **Context Relevance** — are the retrieved chunks actually relevant to the query?
- **Groundedness (Faithfulness)** — is the generated answer fully supported by the retrieved chunks?
- **Answer Relevance** — does the final answer address the user's actual question?

Frameworks like **Ragas** and **TruLens** automate this evaluation in production. [14]

**Semantic Drift Detection** — monitor the cosine similarity distribution of model outputs over time to detect when the model's behavior begins to drift from its approved baseline — a signal that either the underlying model, the system prompt, or the knowledge base has been altered.

**CI/CD for Knowledge** — treat knowledge updates like software releases. Every change to an OKF bundle, a LookML definition, or a RAG ingestion pipeline should go through a pull request workflow with automated evaluation gates before promotion to production.

---

## 11. Conclusion and Architectural Recommendations

Building a production-grade Knowledge Fabric for enterprise GenAI is not a single-technology problem. It is an integration challenge spanning data engineering, knowledge engineering, AI infrastructure, and compliance governance. The following synthesizes the key architectural recommendations from this article.

### Recommendation 1: Adopt a Layered Knowledge Architecture

Do not conflate the Data Fabric (the infrastructure of connected data pipelines) with the Knowledge Fabric (the semantic intelligence layer above it). Build both explicitly. The Knowledge Fabric adds the glossary, taxonomy, ontology, lineage, and provenance metadata that transforms raw data into something an AI agent can reason about with confidence.

### Recommendation 2: Choose Modular RAG, Not Monolithic RAG

Implement a **three-layer Modular RAG architecture** — a Routing Layer (Adaptive RAG), an Execution Layer (RAG-Agency / ComoRAG), and a Verification Layer (CRAG + Self-RAG). Do not build eight isolated RAG pipelines. These techniques become microservices within a single framework, turned on or off per workload. [7, 13]

### Recommendation 3: Never Allow Agents Direct Database Access

The single most important guardrail in a financial AI system is a **semantic layer MCP server** (built on Looker, dbt, or equivalent) that prevents agents from writing raw Text-to-SQL. All financial metrics must be retrieved through pre-audited, version-controlled LookML definitions. This is both a hallucination prevention measure and a SOX compliance requirement.

### Recommendation 4: Implement OKF for the Unstructured Knowledge Layer

Adopt the **Open Knowledge Format** as the standard for authoring and distributing your enterprise's unstructured context. Build an OKF enrichment pipeline (e.g., using Google's reference BigQuery enrichment agent pattern) that auto-generates concept documents for tables, metrics, runbooks, and APIs. Commit these bundles alongside the code they describe and govern changes through pull requests.

### Recommendation 5: Treat SOX Controls as Ontological Rules

Rather than bolting SOX compliance onto the architecture after the fact, encode compliance constraints directly into your financial ontology. Use the COSO Principle 13 framework to restrict agents to pre-audited data sources. Implement HITL state machine interruptions for all transactional workflows. Log Dual-Identity traces (human user + agent version) to a WORM S3 bucket with Object Lock for every execution.

### Recommendation 6: Build LLMOps from Day One

Observability, lineage, and continuous evaluation are not afterthoughts — they are requirements for an auditable, production-grade AI system. Integrate OpenTelemetry distributed tracing, automated RAG Triad evaluation with Ragas/TruLens, and CI/CD pipelines for knowledge updates from the beginning of the project.

### Recommendation 7: Implement Semantic Caching at Every Layer

Three-tier semantic caching (prompt cache, KV-cache, graph cache) is essential for controlling cloud infrastructure costs at enterprise scale. Without it, the cost of LLM API calls and vector search operations will grow proportionally with user adoption, making the platform economically unsustainable.

---

### Closing Thought

The enterprise AI landscape of 2026 has moved decisively past the proof-of-concept stage. The organizations now extracting measurable value from GenAI are those that have invested in the unglamorous but essential work: standardizing their metadata semantics, governing their data provenance, and building the compliance guardrails that allow autonomous agents to operate within institutional trust boundaries. The architecture described in this article is not a future vision — it is a deployable blueprint, built on open standards and production-proven technologies, for any enterprise ready to make that investment.

---

## Citations

All citations are preserved inline throughout the article as `[n]` reference markers. The original citation sources from the research material are:

- [1]–[9]: Dimensions and Metrics in Analytics — nicolalazzari.ai, piwik.pro, graduateschool.edu, metabase.com, learnbi.online, altitudemarketing.com, funnel.io, developers.google.com, youtube.com
- [11]–[17]: Corrective RAG (CRAG) — cobusgreyling.medium.com, lancedb.com, arxiv.org
- [14]–[17]: Self-RAG — arxiv.org, selfrag.github.io
- [18]–[22]: Adaptive RAG — arxiv.org, meilisearch.com, geeksforgeeks.org, dzone.com
- [25]–[27]: Lean RAG — arxiv.org, ojs.aaai.org
- [28]–[32]: ComoRAG — ojs.aaai.org, github.com
- [33]–[36]: REX RAG — arxiv.org, openreview.net
- [37]–[42]: RAG 3.0 / Agentic RAG — ibm.com, weaviate.io, medium.com, linkedin.com
- Knowledge Fabric use-case and combination sources: atlan.com, ibm.com, medium.com, arxiv.org, sdh.global
- Enterprise Knowledge Fabric architecture: cisco.com, apache.org, neo4j.com, aws.amazon.com
- Semantic Layer: strategy.com, atlan.com, cube.dev, timbr.ai, atscale.com
- LLM-Wiki alternatives: dev.to, google.com (NotebookLM), github.com (Khoj)
- Amazon Strands: aws.amazon.com, strandsagents.com, dev.to
- Glossary/Taxonomy/Ontology: erstudio.com, topquadrant.com, datahub.com, neo4j.com, atlan.com
- Financial Semantic Layer: medium.com, wandb.ai, atlan.com, atscale.com, tellius.com
- Data Lineage: atlan.com, dataversity.net, decube.io, ovaledge.com, collibra.com
- Data Provenance: yext.com, snowflake.com, arxiv.org, cacm.acm.org
- SOX Compliance: nodeloom.io, safepaas.com, grantthornton.com, coso.org, isaca.org, eisneramper.com
- OKF: cloud.google.com, github.com/GoogleCloudPlatform/knowledge-catalog, marktechpost.com
- Google Knowledge Catalog: cloud.google.com, docs.cloud.google.com/dataplex, infoworld.com
- Data Contracts: bitol-io.github.io, datacontract.com, medium.com
