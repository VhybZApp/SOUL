# SOUL Motivation Framework: Implementation Plan

This document provides a comprehensive, step-by-step plan for implementing the SOUL Motivation Framework. It is designed to be self-contained: a competent coder should be able to proceed without ambiguity. The plan covers requirements, mathematical foundations (with explanations), architecture, and recommended open source libraries.

---

## 1. Requirements

### Core Objectives
- Equip each agent ("Soulmade") with a dynamic internal state—called the Motivation Vector—so it acts with a consistent agenda, expresses individual biases, and behaves purposefully.
- Enable micro-agent "societies of minds," plug-in knowledge sources, and feedback-driven learning hooks.
- Ensure the Motivation Vector is hidden from the LLM, evolves over time, and drives context-aware actions and interventions.
- Support auto-discovery of subgoals when progress stalls, enabling adaptive exploration.
- Allow for silent observation and learning: agents do not intervene in every interaction, but instead listen and build up a knowledge base (meta-graph) of patterns, rules, and axioms from user–LLM conversations.
- Introduce a thresholded decision mechanism: agents only "nudge" (suggest prompts, actions, or tool calls) when their internal confidence—based on the meta-graph and motivation vector—surpasses a calibrated threshold.
- When nudging, harvest and inject symbolic axioms/rules into the LLM context, relying on the LLM to naturalize or act upon these instructions.
- Continuously adapt using signals of progress, novelty, and stability, refining both the Motivation Vector and the meta-graph.
- Prepare for future genetic mixing of agent policies and sharing of meta-graphs among agent societies.

### Functional Requirements
- Maintain a hidden Motivation Vector \(\mathbf{s} = [s_c, s_u, s_h]\) that is updated after every interaction based on competence, novelty, and homeostasis signals.
- Passively observe user–LLM interactions and record patterns, responses, and context, storing them in a symbolic meta-graph (e.g., using MeTTa language).
- Auto-generate subgoals and generalize patterns using novelty detection and pattern compression.
- Only intervene ("nudge") when the agent's internal confidence—derived from meta-graph matches and motivation vector state—exceeds a dynamic threshold. Otherwise, remain silent and continue learning.
- When nudging, harvest relevant axioms/rules from the meta-graph and inject them into the LLM prompt in symbolic form, relying on the LLM (with a pre-prompt) to interpret and naturalize the intervention.
- Evolve rules and meta-rules using reward-driven local edits, optimal transport updates, and meta-learning, to improve the agent's ability to recognize and act on meaningful patterns.
- Interface with continuous predictive coding (PC) networks for perception and action, exchanging features and actions between neural and symbolic layers.
- Store long-term context and embeddings for memory, enabling retrieval and adaptation over time.

---

## 2. Adopted Mathematics (with Explanations)

### 2.1. Competence-Progress Core
```tex
\Delta_c = p_g(t) - p_g(t-1)
\quad
s_c \leftarrow \mathrm{clip}(s_c + \alpha\,\Delta_c, 0, 1)
```
**Explanation:**
- \(p_g(t)\) is the agent's measured performance (competence) at time \(t\).
- \(\Delta_c\) is the change in competence since the last step.
- \(s_c\) is the competence component of the Motivation Vector; it is updated by adding the scaled change in competence, then clipped to stay between 0 and 1.

### 2.2. Novelty/Surprise Seeding
```tex
\mathrm{novel}(t) = 1 - \frac{\mathbf{e}(t) \cdot \mu_{t-1}}{\|\mathbf{e}(t)\|\,\|\mu_{t-1}\|}
\quad
s_u \leftarrow \mathrm{clip}(s_u + \alpha\,\mathrm{novel}(t), 0, 1)
```
**Explanation:**
- \(\mathbf{e}(t)\) is the embedding of the current context; \(\mu_{t-1}\) is the mean embedding of past contexts.
- This computes the cosine similarity between current and past contexts, subtracts from 1, so higher values mean more novelty.
- \(s_u\) is the novelty/surprise component; it is updated by adding the scaled novelty score, clipped to [0,1].

### 2.3. Homeostatic Decay
```tex
s_h \leftarrow (1 - \delta) s_h + \delta
```
**Explanation:**
- \(s_h\) is the homeostatic drive, which gently decays toward a baseline (e.g., 1) at rate \(\delta\).
- This ensures the agent doesn't get stuck at extremes.

### 2.4. Discrete Generative Core (Rule Metagraph)
- Instincts and policies are encoded as rewrite rules in a directed graph (metagraph).
- Each turn, the agent predicts a distribution over rules, compares it to observed outcomes, and computes error using KL divergence:
```tex
e_t = D_{KL}(q_t \| p_t)
```
**Explanation:**
- \(p_t\) is the predicted distribution over rules; \(q_t\) is the observed distribution.
- \(D_{KL}\) is the Kullback-Leibler divergence, a measure of how one probability distribution diverges from another.
- \(e_t\) is the error signal used for learning and adaptation.

### 2.5. Reward Signals
```tex
r^{\rm int}_t = -e_t
\qquad
r^{\rm ep}_t \propto \sum_m q_t(m) \log \frac{1}{p_t(m)}
```
**Explanation:**
- Internal reward is negative error (the agent is rewarded for reducing surprise).
- Episodic reward is proportional to the negative log-likelihood of predictions.

### 2.6. Meta-Rule Self-Modification
```tex
m_i \leftarrow \arg\min_{m' \in \mathcal{N}(m_i)} e_t(R, \{M \setminus m_i\} \cup \{m'\})
```
**Explanation:**
- The agent searches for a local change (neighbor \(m'\)) to a rule \(m_i\) that minimizes the error \(e_t\).
- Meta-rules are rules that can rewrite the rule graph itself.

### 2.7. Wasserstein Natural Gradient
```tex
\xi_{k+1} = \xi_k - h G(\xi_k)^{-1} \nabla_{\xi} F(p(\xi_k))
```
**Explanation:**
- \(\xi\) are the parameters of the rule distribution.
- \(G(\xi_k)\) is the Laplacian (a kind of matrix) over the rule graph, encoding its geometry.
- This is a gradient descent step that respects the structure of the rule space ("natural gradient").

### 2.8. Neural–Symbolic Hybrid (Predictive Coding Nets)
- Two neural networks (vision and motor) run beneath the discrete core, exchanging symbolic features and actions.
- These networks implement "predictive coding": they try to predict their own inputs, and the difference (prediction error) is used to update both the neural and symbolic layers.

### 2.9. Genetic Tuning (Optional)
- Hyperparameters (e.g., \(\alpha, \delta, \tau_c, \tau_u\)) are encoded as chromosomes and can be evolved using genetic algorithms for optimal performance.

### 2.10. Vector Stores and Long-Term Memory
- Use vector databases to store past context embeddings for retrieval and long-term adaptation.

---

## 3. System Architecture

### 3.1. High-Level Overview
- **Motivation Vector:** Maintains a hidden, continuously evolving state \([s_c, s_u, s_h]\) that encodes the agent's current competence, novelty, and homeostasis.
- **Rule Metagraph:** Encodes instincts, policies, and meta-rules as a symbolic graph (meta-graph), which is initially empty and populated over time by mining patterns from observed user–LLM interactions.
- **Perception/Action:** Continuous neural nets (predictive coding) extract features and generate actions, providing a bridge between low-level perception and high-level symbolic reasoning.
- **Thresholded Nudge Selection:** Instead of nudging every turn, the agent evaluates its internal confidence (based on meta-graph matches and motivation vector state). Only when the threshold is surpassed does it harvest and inject a nudge (in symbolic form) into the LLM context.
- **LLM Naturalization:** Injected nudges are expressed as symbolic axioms/rules (e.g., MeTTa), and the LLM is pre-prompted to interpret and translate these into natural language or tool actions.
- **Memory:** A vector store is used for past context embeddings, enabling long-term adaptation and retrieval.
- **Learning:** The agent updates its rules, motivation vector, and neural nets using error and reward signals, and evolves its meta-graph over time.

### 3.2. Main Loop (Per Turn)
1. **Perceive:** Use predictive coding nets to process input and extract features from the current environment or conversation.
2. **Update Motivation Vector:**
    - Compute competence progress, novelty, and homeostasis using the mathematical formulas above. The Motivation Vector reflects the agent's current internal state.
3. **Observe and Record:**
    - Passively listen to the user–LLM interaction. Record prompts, responses, and relevant context. Extract patterns and store them in the meta-graph in symbolic form (e.g., MeTTa expressions).
4. **Meta-Graph Growth and Compression:**
    - Generalize and compress observed patterns, forming new rules or merging similar ones to keep the meta-graph efficient and expressive.
5. **Threshold Evaluation:**
    - Calculate a confidence score based on meta-graph matches and the Motivation Vector. Only proceed if the score surpasses a dynamic threshold; otherwise, remain silent and continue learning.
6. **Harvest and Inject Nudge (when threshold is reached):**
    - Identify relevant axioms/rules from the meta-graph that justify intervention. Inject these (in symbolic form) into the LLM prompt, relying on the LLM to interpret and naturalize them.
7. **Evaluate:**
    - Compare predicted vs actual outcomes, compute error, and update rewards. Use this feedback to refine the Motivation Vector and meta-graph.
8. **Update Rules:**
    - Adjust rule weights using Wasserstein natural gradient. Optionally mutate rules/meta-rules for local improvement.
9. **Memory:**
    - Store context/embedding in the vector store for future retrieval and adaptation.
10. **(Optional) Genetic Tuning:**
    - Periodically evolve hyperparameters using genetic algorithms to optimize agent performance.

### 3.3. Data Structures
- **Motivation Vector:** Python `dataclass` with fields for \(s_c, s_u, s_h\).
- **Rule Graph:** `networkx.DiGraph` with nodes for rules/meta-rules.
- **Neural Nets:** `torch.nn.Module` or `jax` models for predictive coding.
- **Memory:** `faiss` or `chromadb` vector store.
- **Hyperparameters:** Numpy array or genetic algorithm chromosome.

---

## 4. Open Source Python Libraries

| Component                | Library/Tool         | Purpose                        |
|--------------------------|----------------------|---------------------------------|
| Vector/Matrix Ops        | `numpy`              | All mathematical operations     |
| Rule Graph/Metagraph     | `networkx`           | Discrete rules & meta-rules     |
| KL Divergence/Error      | `scipy.stats.entropy`| Divergence, error calculation   |
| Optimal Transport        | `pot`                | Wasserstein natural gradient    |
| Neural Nets (PC)         | `torch`, `jax`       | Predictive coding networks      |
| Vector Store             | `faiss`, `chromadb`  | Long-term memory                |
| Genetic Tuning           | `DEAP`, `pygad`      | Evolutionary optimization       |

---

## 5. Implementation Notes and Best Practices
- Modularize each component (motivation vector, rule graph, neural nets, memory, genetic tuner) for independent testing and development.
- Use clear interfaces for neural-symbolic communication (e.g., feature extraction, symbolic action selection).
- Log all state updates and decisions for interpretability and debugging.
- Start with a minimal working prototype (motivation vector, rule graph, and competence/novelty updates), then incrementally add complexity.
- Regularly checkpoint agent state and rule graphs for reproducibility.

---

## 6. References
- See the LaTeX guide for original formulas and further reading.
- Key libraries: [numpy](https://numpy.org/), [networkx](https://networkx.org/), [scipy](https://scipy.org/), [pot](https://pythonot.github.io/), [torch](https://pytorch.org/), [jax](https://jax.readthedocs.io/), [faiss](https://github.com/facebookresearch/faiss), [chromadb](https://www.trychroma.com/), [DEAP](https://deap.readthedocs.io/en/master/), [pygad](https://pygad.readthedocs.io/en/latest/)

---

This plan is designed to be actionable and complete. If you need code scaffolds, pseudocode, or further breakdowns for any section, request them as needed.

---

## 7. Meta-Graph Bootstrapping, Silent Observation, and LLM-Driven Naturalization

### 7.1. Overview

The SOUL Motivation Framework is designed to avoid premature or ill-informed interventions. Instead, it adopts a "silent observer" approach at startup, bootstrapping its meta-graph (rule/axiom space) by listening passively to user–LLM interactions. Only when it accumulates enough evidence and confidence does it begin to "nudge" the LLM, using harvested axioms and pre-prompted translation mechanisms.

### 7.2. Step-by-Step Process

1. **Silent Observation Phase**
   - On initialization, the agent's meta-graph (atomspace) is empty.
   - For each user–LLM interaction, the agent:
     - Records the exchanged prompts, responses, and any relevant context.
     - Extracts and compresses patterns, phrases, or actions from these interactions.
     - Stores them as symbolic atoms/rules in the meta-graph (using a language like MeTTa).
   - No nudges or interventions are issued at this stage.

2. **Meta-Graph Growth and Compression**
   - The agent incrementally builds up the meta-graph, seeking to represent observed conversational patterns, user goals, and LLM behaviors as compactly as possible.
   - Example MeTTa rule (mined from a repeated LLM suggestion):
     ```metta
     (suggest-alternative (context low-novelty) (action "Propose an alternative approach."))
     ```
   - The agent may compress similar patterns into more general rules or axioms.

3. **Thresholded Opinion Mechanism**
   - The agent remains silent until a confidence threshold is reached. This threshold is based on:
     - The density or frequency of matching patterns in the meta-graph.
     - The agent's internal motivation vector (e.g., high novelty drive, competence progress).
     - A match score between the current context and the compressed meta-graph.
   - Only when the threshold is crossed does the agent form a "strong opinion" and prepare to intervene.

4. **Harvesting and Expressing Axioms**
   - When ready to act, the agent:
     - Identifies the relevant axioms/rules from the meta-graph that justify its intervention.
     - Extracts these in their native symbolic (MeTTa) form.
     - Example harvested axiom:
       ```metta
       (should-nudge (context (low-novelty user-query)) (action suggest-alternative))
       ```
   - These axioms are injected into the LLM input, not shown to the user directly.

5. **LLM Pre-Prompting and Naturalization**
   - The LLM is pre-prompted (system message) to interpret the injected meta-graph axioms as instructions or context.
   - Example system pre-prompt:
     > "You may receive special meta-graph instructions in MeTTa syntax. If present, interpret them and translate their intent into natural language or appropriate actions."
   - Example LLM input:
     ```
     [SYSTEM]: You may receive special meta-graph instructions in MeTTa syntax. If present, interpret them and translate their intent into natural language or appropriate actions.
     [USER]: <conversation history>
     (should-nudge (context (low-novelty user-query)) (action suggest-alternative))
     ```
   - The LLM then generates a natural nudge, e.g., "Can you propose an alternative approach that you haven’t tried before?"

### 7.3. MeTTa Code Examples

- **Recording a Pattern:**
  ```metta
  (recorded-pattern (user-query "How do I solve X?") (llm-response "Have you tried breaking it down?"))
  ```
- **Generalizing to a Rule:**
  ```metta
  (suggest-breakdown (context (problem-solving)) (action "Suggest breaking down the problem."))
  ```
- **Threshold Check:**
  ```metta
  (opinion-threshold (match-score 0.85) (threshold 0.8) (ready-to-nudge true))
  ```
- **Harvested Axiom for LLM:**
  ```metta
  (should-nudge (context (low-novelty user-query)) (action suggest-alternative))
  ```

### 7.4. Considerations and Variations

- **Meta-Graph Language:**
  - Use MeTTa or a similar symbolic language for expressing rules and axioms.
  - Ensure the LLM is robustly pre-prompted to interpret these instructions.
- **Compression Quality:**
  - The agent's ability to generalize and compress patterns affects the quality and relevance of its nudges.
  - Consider using clustering or similarity metrics on observed patterns before rule formation.
- **Threshold Calibration:**
  - Adjust the threshold dynamically based on agent confidence, novelty drive, or user feedback.
- **Transparency:**
  - Log all harvested axioms and interventions for debugging and analysis.
- **Extensibility:**
  - The agent can be extended to learn new meta-graph languages or adapt to different LLMs by updating the pre-prompt.
- **Fallbacks:**
  - If the LLM cannot interpret a meta-graph axiom, the agent can default to template-based nudges or remain silent.

### 7.5. Possible Paths and Extensions

- **Active Learning:**
  - Allow the agent to query the LLM for clarification on ambiguous patterns, improving its meta-graph.
- **Society of Agents:**
  - Multiple "soulmades" can share or merge their meta-graphs, leading to richer, more diverse interventions.
- **Hybrid Approaches:**
  - Combine template-based nudging with meta-graph-driven nudging for greater flexibility.
- **Human-in-the-Loop:**
  - Allow users to review, edit, or approve harvested axioms before they are injected into the LLM.

---

This extended approach ensures that the SOUL Motivation Framework remains grounded, adaptive, and interpretable, leveraging both symbolic meta-graph reasoning and LLM generativity for effective, context-aware nudging.
