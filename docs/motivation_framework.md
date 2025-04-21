# SOUL Motivation Framework

## 1. Purpose

The Motivation Framework is the core component within the SOUL architecture responsible for defining and managing the **Internal State** of a Soulmade. It provides the foundation for:

*   **Purposeful Agency:** Driving the Soulmade's actions based on internal goals and drives rather than purely reactive responses.
*   **Distinct Perspective:** Encoding biases and viewpoints that influence how information is processed and responses are formulated.
*   **Dynamic Agenda:** Enabling the Soulmade to form and pursue context-dependent goals.

This aligns with conceptual models like Minsky's "Society of Minds," where complex behavior emerges from the interaction of simpler, competing drives or motivations. It also directly addresses the need for dynamic, state-based motivation systems suitable for diverse AGI architectures (as outlined in related research RFPs).

## 2. Core Component: The Motivation Vector

The Internal State is primarily represented by a **Motivation Vector**. This is conceived as:

*   **Multi-dimensional:** Each dimension represents a specific drive, goal, value, bias, or emotional analogue (e.g., Specificity, Objectivity, Curiosity, Novelty Seeking, User Goal Alignment, Resource Conservation, AdherenceToPerspectiveX).
*   **Dynamic:** The values along these dimensions change over time based on internal logic and external stimuli.
*   **Influential:** The current state of the vector directly influences other SOUL components, primarily the Background Prompting Engine.

**Design Rationale:** Using a vector provides a flexible, quantifiable way to represent complex internal states. It allows for nuanced interactions between different drives and facilitates computational updates.

## 3. Seed Instincts & Configuration

While the Motivation Vector is dynamic, Soulmades require an initial configuration or "personality." This is achieved through **Seed Instincts**:

*   **Definition:** Pre-defined heuristics, default values for vector dimensions, or initial background prompting strategies associated with specific motivations.
*   **Source:** These can be derived from psychological theories (e.g., Maslow, Self-Determination Theory), best practices observed in AI assistants (see `prompt_library/`), or designed to create specific "alien digital" personalities.
*   **Configuration:** Loaded via configuration files (`config/`) allowing different Soulmade types to be instantiated with distinct starting motivations and perspectives.

**Design Rationale:** Seeding provides essential bootstrapping, ensuring baseline competence and allowing designers to create agents with specific initial characteristics, while still leaving room for adaptation.

## 4. Update Mechanism & Adaptability

The true power lies in the Motivation Vector's dynamism. It is updated based on:

*   **Perception Input:** Direct cues from user interaction or environmental sensors.
*   **Internal Evaluation:** Assessing the success/failure of previous actions in achieving goals derived from the current agenda (tracked via Evaluation/Learning Hooks).
*   **Feedback:** Explicit or implicit feedback from the host system or user.
*   **Internal Logic:** Pre-defined rules or potentially learned associations governing how different motivational dimensions interact or decay over time.

**Design Rationale:** This dynamic updating allows Soulmades to adapt their priorities, perspectives, and agenda based on context, experience, and feedback, moving beyond static rule-based systems. This is crucial for agents operating in complex, unpredictable environments.

## 5. Relationship to Other Components

*   **Guides Background Prompting Engine:** The primary role of the Motivation Vector's current state is to bias the selection and generation of background prompts. See [Background Prompting Engine](./prompting_engine.md).
*   **Influences Action Selection:** May directly influence the final action chosen if multiple options are generated.
*   **Modulated by Governance:** Can be constrained or overridden by rules defined via [Governance Hooks](./governance_hooks.md).

## 6. Pluggability & Extensibility

The framework is designed with interfaces (`src/soul/interfaces/motivation.py`) allowing for different models of motivation or update mechanisms to be implemented and plugged in, accommodating future research and diverse agent types.
