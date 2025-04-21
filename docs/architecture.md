# SOUL Architecture Deep Dive

## 1. Overview

This document provides a more detailed exploration of the **SOUL (Specific and Objective Understanding Logic)** Framework's architecture. For a high-level introduction, key concepts, and the overall vision, please refer to the main `README.md`.

The core philosophy is a **modular, pluggable framework** designed to orchestrate sophisticated AI agency (**Soulmades**) by **guiding powerful, pre-trained Large Language Models (LLMs)** through dynamic internal states and targeted background prompting.

## 2. Architectural Diagram (Reiteration for Context)

```mermaid
graph LR
    subgraph Host System
        Host["Host System/Application <br/>(Chatbot, Game, Robot)"]
    end

    subgraph External Systems
        LLM["Pluggable LLM <br/>(GPT-4o, etc.)"]
        KS["External Knowledge Source <br/>(MCG, DB, Files, API)"]
    end

    subgraph SOUL Framework Core
        direction TB

        subgraph Agent Core
            SOUL_Agent["SOUL Agent Orchestrator<br/>(Main Loop)"]
        end

        subgraph Cognitive Components
            MF["Motivation Framework <br/>(Vector, Updaters)"]
            BPE["Background Prompting Engine <br/>(Generator, Templates)"]
            Gov["(Governance Hooks Interface)"]
        end

        subgraph Interfaces & Context
            PAI["Perception/Action Interfaces"]
            KI["Knowledge Interface"]
            LLMI["LLM Interface"]
            CtxA["Context Assembler"]
            Eval["Evaluation/Learning Hooks"]
        end

        %% Connections from SOUL Agent %%
        SOUL_Agent -->|Input/Output| PAI
        SOUL_Agent -->|Triggers Cognitive Cycle| MF
        SOUL_Agent -->|Receives Action Plan| BPE
        SOUL_Agent -->|Sends Context Requests| CtxA
        SOUL_Agent -->|Applies Moderation?| Gov

        %% Internal Cognitive Connections %%
        MF --->|Guides| BPE
        CtxA -->|Assembled Context| BPE
        BPE --->|Generates Prompt| LLMI
        BPE --->|Receives LLM Result Analysis| Eval
        MF --->|Current State (Vector)| CtxA %% State used by Context Assembler %%
        MF --->|Receives Updates| Eval

        %% Interface Connections %%
        CtxA -->|Queries| KI
        KI --->|Accesses| KS
        LLMI --->|Requests| LLM
        LLM --->|Returns Response| LLMI
        LLMI --->|Provides Result| BPE
        KS --->|Provides Data| KI
        KI --->|Provides Knowledge| CtxA

        %% Governance Connections %%
        Gov -->|Moderates?| MF
        Gov -->|Moderates?| BPE
        Gov -->|Moderates?| SOUL_Agent # Action Vetting
    end

    %% System Boundary Connections %%
    Host <---> PAI

    %% Styling %%
    style Host fill:#68a,stroke:#DDD,stroke-width:2px,color:#FFF
    style External Systems fill:#eec,stroke:#333,stroke-width:2px,color:#000
    style SOUL_Agent fill:#446,stroke:#DDD,stroke-width:2px,color:#FFF
    style MF fill:#558,stroke:#DDD,stroke-width:1px,color:#FFF
    style BPE fill:#558,stroke:#DDD,stroke-width:1px,color:#FFF
    style Gov fill:#666,stroke:#AAA,stroke-width:1px,stroke-dasharray: 5 5,color:#FFF
    style PAI fill:#474,stroke:#DDD,stroke-width:1px,color:#FFF
    style KI fill:#474,stroke:#DDD,stroke-width:1px,color:#FFF
    style LLMI fill:#474,stroke:#DDD,stroke-width:1px,color:#FFF
    style CtxA fill:#777,stroke:#AAA,stroke-width:1px,color:#FFF
    style Eval fill:#777,stroke:#AAA,stroke-width:1px,color:#FFF
```

## 3. Core Component Details (`src/soul/`)

### 3.1. Agent Orchestrator (`agent.py`)

*   **Responsibility:** Manages the main operational loop of the Soulmade.
*   **Functionality:**
    *   Receives perceptual input via the Perception Interface.
    *   Initiates the cognitive cycle by triggering context assembly and motivation updates.
    *   Coordinates the interaction between the Motivation Framework, Context Assembler, and Background Prompting Engine.
    *   Receives the final proposed action/output from the BPE (potentially after LLM interaction).
    *   Optionally passes proposed actions through Governance Hooks for vetting.
    *   Sends the final action/output to the host system via the Action Interface.
    *   Manages overall agent lifecycle and state transitions.

### 3.2. Interfaces (`interfaces/`)

*   **Purpose:** Define the contracts for pluggability. They use Python's Abstract Base Classes (ABCs).
*   **Key Interfaces:**
    *   `PerceptionInterface`: Methods for receiving input (e.g., `on_message(text)`, `on_sensor_update(data)`). Implemented by the host system adapter.
    *   `ActionInterface`: Methods for executing outputs (e.g., `send_message(text)`, `perform_action(action_spec)`). Implemented by the host system adapter.
    *   `LLMInterface`: Defines methods for interacting with a backend LLM (e.g., `generate(prompt, config) -> LLMResponse`). Implemented by adapters in `src/soul/adapters/llm/`.
    *   `KnowledgeInterface`: Defines methods for querying external knowledge (e.g., `search(query)`, `retrieve(id)`). Implemented by adapters in `src/soul/adapters/knowledge/`.
    *   `MotivationModel`, `MotivationUpdater`: Define how motivational states are represented and updated. Implemented in `src/soul/components/motivation/`.
    *   `GovernanceHook`: Defines methods for moderation points (e.g., `vet_action(action) -> bool`, `moderate_motivation(vector) -> vector`). Implemented by the host system or specific governance modules.

### 3.3. Motivation Framework (`components/motivation/`)

*   **Responsibility:** Representing and updating the Soulmade's internal state.
*   **`MotivationVector` (`core/state.py`):** A data structure (e.g., numpy array, dictionary) holding values for different motivational dimensions. Dimensions are configurable.
*   **`Instincts` (`instincts.py`):** Provides default configurations and seed values for the Motivation Vector, defining initial personality/biases. Loaded from `config/`.
*   **`Manager` (`manager.py`):** Orchestrates the update process. Receives triggers (from perception, evaluation) and applies relevant `Updater` logic.
*   **`Updaters` (Defined abstractly in `interfaces/motivation.py`, implemented here):** Contain the specific logic for how dimensions of the Motivation Vector change based on different inputs (e.g., decay over time, boost on success, shift based on user sentiment). This is a key area for implementing different motivational theories (human-like or alien).

### 3.4. Background Prompting Engine (BPE) (`components/prompting/`)

*   **Responsibility:** Generating effective, targeted prompts for the backend LLM based on current state and context.
*   **`Engine` (`engine.py`):** Coordinates the prompt generation process. Takes the current Motivation Vector and assembled context as input.
*   **`Templates` (`templates.py` & `prompt_library/`):** Loads and manages `.md` prompt templates. Templates contain placeholders for dynamic injection of context, rules, and motivational directives.
*   **`Generator` (`generator.py`):** Selects appropriate template(s) based on the Motivation Vector and task type. Instantiates templates by filling placeholders with data from the Context Assembler and injecting motivation-specific instructions (e.g., adding "Be objective" or "Prioritize perspective X" based on the vector).
*   **`Refiner` (`refiner.py`):** (Optional/Advanced) Handles multi-turn interactions with the LLM, analyzing initial responses and generating follow-up prompts for clarification or correction.

### 3.5. Context Assembler (`components/context/`)

*   **Responsibility:** Gathering all necessary information required by the BPE to generate an effective background prompt.
*   **Functionality:**
    *   Retrieves conversational history relevant to the current interaction.
    *   Queries the Knowledge Interface (`KI`) for external facts or relevant stored information based on the current topic or agenda.
    *   Accesses the current Motivation Vector to potentially include relevant biases or goals as context for the LLM.
    *   Structures the assembled information into a format suitable for the BPE's template instantiation process.

### 3.6. Evaluation & Learning Hooks (`components/evaluation/`)

*   **Responsibility:** Assessing the effectiveness of the Soulmade's actions and enabling adaptation.
*   **`Tracker` (`tracker.py`):** Receives feedback (explicit user feedback, implicit signals like task success/failure, host system validation) related to the outcome of an action generated via a specific background prompt and motivation state.
*   **Integration:** Signals from the Tracker feed back into the Motivation Framework's `Manager` and potentially influence the BPE's `Generator` (e.g., updating probabilities for selecting certain prompt templates based on past effectiveness). This forms the basis for adaptive learning.

## 4. Data Flow Example (Simplified Chatbot Interaction)

1.  **Host -> PAI:** User sends a message "What's the latest on Project X?".
2.  **PAI -> Agent Orchestrator:** Message received.
3.  **Agent -> Context Assembler:** Request relevant context for "Project X".
4.  **Context Assembler -> KI:** Query knowledge source for "Project X status".
5.  **KS -> KI -> Context Assembler:** Returns latest status update data.
6.  **Context Assembler:** Assembles history + status update.
7.  **Agent -> Motivation Framework:** Trigger update cycle (e.g., increase "Information Seeking" drive).
8.  **Motivation Framework -> Agent:** Provides current Motivation Vector (e.g., high "Specificity", moderate "Objectivity", active perspective "Internal Team View").
9.  **Agent -> BPE:** Initiate prompt generation with assembled context and motivation vector.
10. **BPE (Generator):** Selects a "status update" template. Injects context. Adds directives based on motivation: "Provide a specific, factual update on Project X from an internal perspective. Be concise."
11. **BPE -> LLMI:** Sends the generated background prompt.
12. **LLMI -> LLM:** Forwards request.
13. **LLM -> LLMI:** Returns generated text: "Project X: Backend API integration 80% complete, frontend blocked on design mockups. Target completion next sprint."
14. **LLMI -> BPE:** Provides LLM response.
15. **BPE -> Agent Orchestrator:** Forwards proposed response.
16. **Agent -> Governance Hooks:** (Optional) Vet response for policy compliance. Assume OK.
17. **Agent -> PAI -> Host:** Sends the final message to the user.
18. **Agent -> Evaluation Hooks:** Log that this prompt/motivation combo led to a successful response (assuming no negative feedback).

This detailed flow illustrates the interplay between the modular components in processing input, leveraging internal state and external knowledge, guiding the LLM, and generating a purposeful output.
