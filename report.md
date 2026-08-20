# AI Workstyle Self-Coaching Report

## Evaluation Standard & Origin

This report measures AI interaction quality against the **OpenAI Prompt Engineering Best Practices** (system instructions, explicit output schemas, zero/few-shot prompting) and **Anthropic’s Prompt Engineering Guidelines** (XML tag structuring, explicit context boundary setting, system role separation, and Chain-of-Thought enforcement). These guidelines have served as the benchmark for systematic prompt design from 2024 through 2026.

The evaluation assesses three main operational domains:
1. **Context Boundary Engineering**: Supplying structural constraints and raw schemas upfront to prevent hallucinated assumptions.
2. **Deterministic Data Grounding**: Recognizing when text inputs are insufficient for quantitative accuracy and pivoting to primary source documents.
3. **Guardrail Navigation & State Management**: Diagnosing policy blocks, isolating trigger parameters, and managing model session memory during refusals.

---

## Coaching Report: Domain Breakdown

### Sub-Topic 1: Workflow Automation & Code Generation (Chat 1: Job Scraper)

* **What You Do Well**:
  * **System Priming**: Effectively established strict persona boundaries ("professional automation bot") to force non-conversational, executable JSON outputs for n8n.
  * **Complete Requirements Contracting**: Explicitly mapped data destinations (Google Sheets) and defined target schema fields (Role, Company, Description, Location, Link, Date, Source) prior to workflow generation.
* **What Is Holding You Back**:
  * **Conversational Overhead**: Prompt 1 was spent entirely on persona setup without task context, needlessly adding an uninformative turn to the context window.
  * **Deferred Schema Payloads**: Mentioned that Apify actors have varying key fields without providing sample JSON outputs, forcing the model to generate fallback JavaScript logic (`item.json.title || item.json.role || ''`) rather than exact field mappings.
* **What to Change Next Week**:
  * **Single-Turn Task Contracts**: Combine persona, architecture specs, and raw sample JSON payloads into a single initial prompt to generate production-ready code in one turn.

### Sub-Topic 2: Quantitative Data & Multimodal Logic (Chat 2: CGPA Calculator)

* **What You Do Well**:
  * **Multimodal Pivot**: Correctly recognized that text-based summaries were yielding inaccurate calculations and pivoted to uploading the raw official transcript image (`transcript.jpg`).
  * **Requirement Verification**: Used the model to verify an explicit external benchmark (last 60 credits admission threshold) rather than accepting simple aggregate averages.
* **What Is Holding You Back**:
  * **Garbage In, Garbage Out**: Prompts 1 and 2 provided loose letter grades without course credit counts or Pokhara University's official grade scale. This led the model to assume equal 20-credit semester weights, producing a dangerous false positive (**3.10 CGPA calculated vs. 2.88 actual**).
  * **Redundant Prompting**: Prompt 2 duplicated Prompt 1 without adding missing parameters (credits or course weightings), wasting an iteration without improving calculation accuracy.
* **What to Change Next Week**:
  * **Primary Source First**: For any quantitative or rule-bound calculation, provide the primary document or explicit mathematical scale in Prompt 1 before asking for computation.

### Sub-Topic 3: Guardrail Navigation & Refusal Recovery (Chat 3: Image Generation Refusal)

* **What You Do Well**:
  * **Diagnostic Meta-Prompting**: Asked the model directly to explain the refusal ("tell me which part of prompt is against guidelines"), uncovering trademark/copyright IP triggers (Tobey Maguire, Spider-Man, X-Men) and prompt complexity limits.
  * **Prompt Sanitization Strategy**: Successfully prompted the model to generate IP-generalized alternatives ("iconic, detailed web-swinging hero") to preserve artistic intent while respecting safety filters.
* **What Is Holding You Back**:
  * **Context Contamination**: After receiving sanitized prompt options, you re-pasted the original trademark-heavy prompt, re-triggering the refusal flag.
  * **Session Memory Persistence**: Sent the sanitized prompt into the same chat thread where a refusal state was already active. Automated safety guardrails often persist across turns in a flagged session, causing sanitized prompts to fail when they would succeed in a clean window.
* **What to Change Next Week**:
  * **Hard Reset on Refusal**: Once a prompt strategy is sanitized, immediately open a clean chat thread to execute it without lingering safety state flags.

---

## One More Thing: The False Signal of AI Skill

**Metric to deliberately NOT score**: **High Prompt Interaction Volume / Turn Count per Task**

**Rationale**: High interaction frequency is frequently misidentified as deep AI adoption or high engagement. In reality, a high number of back-and-forth prompts often indicates **prompt chaos**—a trial-and-error workflow where the user relies on the LLM to guess requirements through attrition. A highly skilled practitioner crafts structured, context-rich prompts that achieve deterministic outputs in 1 to 2 turns. Scoring volume incentivizes unfocused chatter over deliberate prompt engineering.

---

*Assistant and model used for this task: Gemini 3.1 Pro[cite: 1]. Error caught: During the initial synthesis of Chat 2, the assistant attempted to average the SGPAs directly without accounting for the varying credit hours per semester (17, 17, and 16 credits) shown on the transcript image[cite: 1].*