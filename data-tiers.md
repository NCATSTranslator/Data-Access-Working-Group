# Data Tier Definitions

This document defines the data tiers within Dogpark from the perspective of Retriever requirements and interface guarantees.
It will not prescribe hosting technologies.

## Tier 0

Tier 0 shall operate under the following requirements:

Modelling/content:

- Pre-conformed to BioLink model and node-normalized
- Knowledge graph is not partitioned by source

Usage:

- Higher performance than Tier 1
- Sub-queried by Retriever in all normal cases of Shepherd queries
- Supports multi-hop graph queries
- Interfaces with Retriever through a query language determined by its hosting technology, not TRAPI

## Tier 1

Tier 1 shall operate under the following requirements:

Modelling/content:

- Pre-conformed to BioLink model and node-normalized
- Not necessarily a single unified knowledge graph
- Update pipeline is significantly faster than Tier 0
- Comprehensive: includes all Translator-ingested knowledge

Usage:

- Supports 1-hop associations
- Sub-queried by Retriever in all normal cases of Shepherd queries
  - Subject to Retriever reasoning about when to query Tier 1 vs. Tier 0
- Interfaces with Retriever through a query language determined by its hosting technology, not TRAPI

## Tier 2

Tier 2 shall operate under the following requirements:

Modelling/content:

- No guarantee of BioLink model compliance or node-normalization
  - Mapping to BioLink model is achieved by annotation, if necessary
  - Node normalization is done at query-time
- No specific hosting format; Tier 2 resources are not necessarily under Translator control
- Supplementary to knowledge ingested by Translator, preferrably non-overlapping

Usage:

- Assumed to only support 1-hop associations (or associations by entity-based lookup)
- Interfaces with Retriever through either TRAPI or bespoke REST model via x-bte annotation
- No guarantees about performance
- Sub-queried by Retriever in all normal cases of Shepherd queries
  - Unlike Tiers 1 & 0, however, these sub-queries are not awaited by query graph execution and must not contribute to Retriever query processing time
  - Retriever shall make available some flag by which a client may request that these sub-queries are awaited
