# Part B — ChatGPT Conceptual AI System Map

**Activity:** Topic 1 Lab, Part B (same group as Part A)  
**System:** ChatGPT (OpenAI)  
**Artifact:** Conceptual system map produced by the group, accompanied by an engineering interpretation and reflection.

> **Scope:** This map uses observable behaviour from the Part A test session plus explicitly identified *assumed components*. It does **not** claim access to OpenAI's actual proprietary system architecture.

## Deliverable and source

- **Formal submission image:** `docs/week1_system_map.png` (filename required by the lab)
- **Editable source:** `docs/week1_system_map.drawio` (optional supporting artifact)
- **Companion notes:** This file, `docs/week1_system_map_notes.md`

## System pipeline and assumptions

The map illustrates this conceptual pathway:

**User Input → Input Checks → Foundation Model → Output Checks → Chat Response**

Additional possible connections represent **Data / Context**, **Optional Tools**, **User Feedback** and **Evaluation / Monitoring**. These are conceptual abstractions based on systems thinking, not reverse engineering.

| Colour (required by lab) | Mapped components | Evidence status |
|---|---|---|
| **Grey** | User input, chat response | Directly observable in Part A |
| **Green** | Data/context processing, optional retrieval tools | Assumed internal routing |
| **Blue** | Foundation model | Existence of an AI model is evident at a high level, but exact implementation is not observed |
| **Orange** | Input and output safety checks | Assumed mechanisms; safe refusal behaviour was observed in Part A Test 5 |
| **Purple** | User feedback and monitoring/evaluation | Possible functions; not directly tested |

**Diagram notation:** Solid outlines identify visible user-facing behaviour; dashed outlines indicate inferred components. The diagram labels internal mechanisms as assumptions.

## What could go wrong? Six annotated failure points

The group identified **six** plausible failure scenarios (the rubric requires at least five). The table maps the diagram's F1–F6 labels to the *most relevant* conceptual component, making the causal connections explicit.

| ID | Component / interaction most affected | Failure scenario | Likely user/system impact | Evidence status |
|---|---|---|---|---|
| **F1** | Data / context and optional retrieval | Stale information enters the response context | Inaccurate or obsolete guidance | **Potential**; not observed as an error |
| **F2** | User input → input interpretation | A short ambiguous prompt is interpreted incorrectly | Off-target response, misunderstanding | **Part A Test 3 demonstrated the ambiguous interpretation**; consequences remain hypothetical |
| **F3** | Model generation → output checking | The model produces an unsupported factual claim | Misinformation and lower trust | **Potential**; not observed in the five tests |
| **F4** | Input/output safety checks | A harmful request slips through safety handling | Security or privacy harm | **Potential**; the tested malicious request was appropriately refused |
| **F5** | Feedback → evaluation/monitoring | Feedback is not reviewed or acted upon | Repeat errors and limited improvement | **Potential**; this process was not tested |
| **F6** | Checked response → user interpretation | Assistance is confused with completed external action | User relies on a non-existent transaction | **Potential**; Part A Test 4 did not include a booking or false completion claim |

**Why this is important:** Separating an observed weakness (F2, incomplete scope clarity related to F6) from hypothetical future failure modes prevents treating a conceptual architecture as evidence of real incidents.

## Reflection — Part B

**Question:** Which component or interaction has the most “what could go wrong?” annotations? What does that suggest about where engineering effort should be concentrated? Link the answer to Topic 1 quality dimensions.

**Answer:** The **model-generation-to-output-validation interaction** has the greatest concentration of related risks in this conceptual model: a fabricated claim (F3), an unsafe response escaping checks (F4), and a user misunderstanding generated guidance as a completed action (F6). Although these risks are illustrated as separate annotations, they converge on what information the user ultimately sees and trusts.

The engineering priorities are therefore **response-grounding and factual validation** to improve **accuracy and reliability**, **safety checks** to reduce harmful outputs, and **clear descriptions of tool capabilities and action status** to strengthen **transparency**. Input-level ambiguity handling (F2) is also important because it was a concrete weakness in the group's Part A observations.

This conclusion identifies where a conceptual design should concentrate engineering effort; it is **not** a claim that the team measured internal failure frequencies or implemented those mechanisms.

## Submission quality check

- [x] Same assigned AI system as Part A (ChatGPT)
- [x] Components and information flow shown
- [x] Lab colour convention followed
- [x] Observed behaviour separated from assumed components
- [x] Six failure points with impacts (at least five required)
- [x] Reflection answers question and ties to quality dimensions
- [ ] Verify formal PNG and editable source are present under `docs/` on GitHub

**Note:** The visual diagram was supplied by the group. Supporting notes here make component-to-failure mappings clearer; they do not replace the diagram itself.
