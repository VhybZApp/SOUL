# SOUL Knowledge Interface

## 1. Purpose

The Knowledge Interface (KI) defines the **abstract mechanism** through which a Soulmade, orchestrated by the SOUL framework, accesses information beyond its immediate perception inputs and the internal knowledge of the backend LLM. Its primary roles are:

*   **Contextual Enrichment:** Providing relevant external data to the Context Assembler and Background Prompting Engine to ground responses and inform reasoning.
*   **Fact Retrieval:** Enabling the Soulmade to look up specific pieces of information needed to fulfill its agenda or answer user queries accurately.
*   **Decoupling:** Separating the core SOUL logic from the specifics of any particular knowledge storage or retrieval system.

**Design Rationale:** AI agents require access to diverse information sources. Hardcoding access to specific databases, APIs, or file systems within the core SOUL logic would make the framework brittle and difficult to integrate into different environments. An abstract interface ensures **pluggability** and **modularity**, allowing SOUL to connect to various knowledge backends.

## 2. Abstraction Benefits

Using an abstract interface (`src/soul/interfaces/knowledge.py`) provides:

*   **Flexibility:** SOUL can be deployed in systems using different knowledge storage technologies (e.g., vector databases, graph databases like OpenCog Hyperon's Atomspace, traditional SQL databases, file systems, proprietary APIs, VhybZ's conceptual MCG) by simply implementing a corresponding adapter.
*   **Maintainability:** Changes to the underlying knowledge source implementation do not require modifications to the core SOUL motivation or prompting logic, as long as the adapter adheres to the interface contract.
*   **Testability:** Allows for mocking knowledge sources during testing of the core SOUL components.

## 3. Conceptual Interface Operations

While the specific methods depend on the implementation, the KI conceptually supports operations such as:

*   `search(query: str, type: SearchType) -> List[KnowledgeFragment]`: Perform semantic or keyword searches.
*   `retrieve(identifier: str) -> KnowledgeFragment`: Fetch a specific piece of knowledge by its ID or URI.
*   `query_structured(query: StructuredQuery) -> QueryResult`: Execute queries against structured data sources.
*   *(Potentially)* `store(fragment: KnowledgeFragment)`: Allow the Soulmade to contribute learned information back to the knowledge source (requires careful governance).

*Note: `KnowledgeFragment`, `SearchType`, `StructuredQuery`, `QueryResult` are conceptual types defined within SOUL's core data structures.*

## 4. Interaction with SOUL Components

*   **Context Assembler:** Uses the KI to fetch relevant background information based on the current interaction context before invoking the Background Prompting Engine.
*   **Background Prompting Engine:** May generate prompts instructing the backend LLM on how to interpret or incorporate information retrieved via the KI. It might also directly trigger KI queries if the LLM identifies a need for specific external facts during its reasoning process (if the architecture supports tool use by the LLM).

## 5. Implementation Adapters

Concrete implementations of this interface reside in `src/soul/adapters/knowledge/`. These adapters translate the abstract KI calls into specific operations for the target knowledge source (e.g., generating Cypher queries for Neo4j, SQL queries for PostgreSQL, API calls for a specific service, or potentially MeTTa queries for Hyperon's Atomspace). The framework can be configured to use one or more adapters depending on the deployment environment.
