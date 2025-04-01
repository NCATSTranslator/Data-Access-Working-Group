# Data Tier Definitions

This document defines the data tiers within Dogpark from the perspective of Retriever requirements and interface guarantees.
It will not prescribe hosting technologies.

## Overall

The below items apply outside of any one specific Tier:

- Retriever shall make access to the Tiers available, but will not reason about which to use
  - It is the responsibility of the client, through some available query flag, to specify which Tier(s) to use
  - This is because each Tier caters to a different use-case, specified below.
- Retriever shall handle predicate symmetry for Tier 2. For Tiers 1 and 0, Retriever shall only query in canonical directions
- Biolink Class and Predicate hierarchy will be handled within Retriever for Tier 2, and pre-computed
  - Whether Tiers 1 and 2 bake-in these hierarchies, or the hierarchies are handled by Retriever, depends on future benchmarking and discussion.
- Handling of node and edge attributes ("properties") is pending further discussion.

## Tier 0

Tier 0 shall operate under the following requirements:

Modelling/content:

- Pre-conformed to BioLink model and node-normalized, with predicates in canonical direction
- Knowledge graph is not partitioned by source
- Lower overall coverage than Tier 1

Usage:

- Higher performance than Tier 1
- Supports multi-hop graph queries
- Interfaces with Retriever through a query language determined by its hosting technology, not TRAPI

Use case:

- The client wants fast multi-hop graph query performance, even at the cost of coverage
- Example: Quick-access subgraphs for secondary use (rule-mining, scoring, etc.)
- Example: Rule-based query expansion lookups
- Example: Rule-based pathfinding

## Tier 1

Tier 1 shall operate under the following requirements:

Modelling/content:

- Pre-conformed to BioLink model and node-normalized, with predicates in canonical direction
- KGs are independently managed and served, with intent to allow for more frequent updates
- Comprehensive: includes all Translator-ingested knowledge
- Acts as a staging ground for knowledge promotion to Tier 0

Usage:

- Supports 1-hop associations
- Potentially higher single-hop performance than Tier 0
- Interfaces with Retriever through a query language determined by its hosting technology, not TRAPI
- Individual KGs are stored accessed via different endpoints

Use case:

- The client wants fast single-hop performance
- Example: Advanced hop-by-hop reasoning
- The client wants greater coverage, even at the cost of multi-hop performance
- Example: Narrow template-based expansion lookups

## Tier 2

Tier 2 shall operate under the following requirements:

> [!NOTE]
> Tier 2 support within Retriever is not an implementation priority until at least after Tier 1 integration MVP

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
- Sub-queried in background by Retriever during regular query execution in order to cache knowledge

Use case:

- Testing ground for APIs where knowledge source dump is less accessible
- The client wants absolute maximum possible coverage, using Tier 2 in combination with Tier 1, at potentially extreme performance cost
