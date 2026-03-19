# 🔴 Gemini 2.5 Pro (Google) — Ultimate Prompt Engineering Guide

> **Models covered:** Gemini 2.5 Pro · Gemini 2.0 Flash · Gemini 2.5 Flash-Lite  
> **Official docs:** [ai.google.dev/gemini-api/docs/prompting-strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies)  
> **Prompt gallery:** [ai.google.dev/gemini-api/prompts](https://ai.google.dev/gemini-api/prompts)

---

## 🧬 Why Gemini Is Different

Gemini 2.5 Pro is Google's **natively multimodal** flagship model, built from the ground up to process text, images, video, audio, and code in a single context window. Its 1M token context is second only to MiniMax and optimized for long-document grounding. Gemini uniquely supports **planning + self-critique loops** (the model can evaluate its own output) and Google Search grounding via the API. For any task involving images, video analysis, or complex multi-step planning, Gemini is the strongest choice.

---

## 🔑 Core Principles (Official + Research-Backed)

### 1. 📸 Multimodal: Always Reference Every Input Explicitly
When passing images, video, audio, or documents alongside text, you MUST explicitly reference each input in your prompt. Gemini doesn't "notice" attachments automatically.

```
❌ "What do you see in this image?"
✅ "Analyze the architecture diagram attached as image_1.png.
    Identify: 1) all system components, 2) data flow direction, 3) potential SPOFs."
```

### 2. 📋 Few-Shot Is Mandatory for Precision Tasks
Gemini's official documentation explicitly states that few-shot examples are one of the most reliable ways to steer output. For any structured or formatted output, provide 2–3 examples.

### 3. 🧭 Planning + Self-Critique Loop
Gemini's most powerful pattern — ask it to plan, execute, then critique its own output:

```
"First, create a plan. Execute the plan. Then review your output against these criteria:
[criteria list]. If any criterion is not met, revise until all are satisfied."
```

### 4. 🏗️ XML + Markdown Hybrid Structure
Gemini handles both XML tags (like Claude) and Markdown headers (like ChatGPT). Use this hybrid for maximum clarity:

```xml
# Identity
You are a senior solution architect.

# Constraints
- No external libraries
- Python 3.11+ only

<task>
[YOUR TASK]
</task>

# Output Format
Return a single fenced code block, then 2-sentence explanation.
```

### 5. 🔍 Google Search Grounding
Unique to Gemini: you can enable Google Search grounding to anchor responses to real-time, verified web data:

```python
from google.generativeai import GenerativeModel
model = GenerativeModel('gemini-2.5-pro')
response = model.generate_content(
    prompt,
    tools=[{"google_search": {}}]  # Enable grounding
)
```

### 6. 💭 Thinking Mode (Gemini 2.5 Pro)
Gemini 2.5 Pro has an experimental thinking mode for complex reasoning:

```python
response = client.models.generate_content(
    model='gemini-2.5-pro',
    contents='[COMPLEX TASK]',
    config=types.GenerateContentConfig(
        thinking_config=types.ThinkingConfig(thinking_budget=10000)
    )
)
```

---

## 💡 Format Control Techniques

| Goal | Instruction |
|------|-------------|
| Image-first analysis | `"Analyze [image description] attached as [filename]. Focus specifically on [aspect]."` |
| Self-correction | `"After completing, re-read your output and fix any inconsistencies before submitting."` |
| Planning mode | `"Before writing any content, produce a numbered outline. I will approve before you continue."` |
| Grounded response | `"Only include claims you can verify with factual data. Flag any speculation with [ESTIMATE]."` |

---

## 🚀 Production-Ready Prompt Templates

### 📌 Template 1: Multimodal Architecture Review (Images + Text)

```
# Identity
You are a world-class cloud architect with expertise in AWS, GCP, and Azure.
You review architecture diagrams with the same rigor as a principal architect at Google.

# Task
Analyze the system architecture diagram provided [IMAGE ATTACHED].

Deliver a structured architecture review:

## Component Inventory
List every component visible in the diagram with its role.

## Data Flow Analysis
Trace the primary data flows from entry to storage.

## Single Points of Failure (SPOFs)
Identify every SPOF with severity [CRITICAL / HIGH / MEDIUM].

## Missing Best Practices
What production-readiness requirements are absent?
(Load balancing, circuit breakers, backup strategies, etc.)

## Recommended Changes
Top 3 highest-priority improvements, each as:
- Change: [What to add/remove/modify]
- Reason: [Why this matters]
- Effort: [Days/Weeks/Months]

## Overall Maturity Score
Rate the architecture [1-10] with one-paragraph justification.
```

---

### 📌 Template 2: Planning + Self-Critique Loop

```
# Identity
You are a world-class [DOMAIN] expert. You use a structured plan-execute-critique loop.

# Process (follow this EXACTLY)

## Phase 1: Plan
Before writing anything, produce a numbered execution plan.
List every step you'll take and what the deliverable will be.

## Phase 2: Execute
Follow your plan step-by-step. Show your work.

## Phase 3: Self-Critique
Evaluate your output against these criteria:
- Criterion 1: [SPECIFIC MEASURABLE GOAL]
- Criterion 2: [SPECIFIC MEASURABLE GOAL]
- Criterion 3: [SPECIFIC MEASURABLE GOAL]

## Phase 4: Revise
For any criterion that is not fully met, revise that section.
Show what you changed and why.

# Task
[YOUR TASK HERE]
```

---

### 📌 Template 3: Long Document Grounded Research (1M Token Context)

```
# Identity
You are a research analyst trained at McKinsey. You produce rigorous,
evidence-based research reports with no speculation.

# Ground Rules
- Every factual claim must cite the document section (page/paragraph if available)
- If a question cannot be answered from the provided documents, say so explicitly
- Never extrapolate beyond the evidence

# Documents Provided
[PASTE UP TO 1M TOKENS OF DOCUMENTS BELOW]

---
[FULL DOCUMENT TEXT]
---

# Research Questions
Answer each question below with:
1. Direct answer (1–2 sentences)
2. Supporting evidence (direct quote + section reference)
3. Confidence level [HIGH / MEDIUM / LOW] with justification

Questions:
1. [QUESTION 1]
2. [QUESTION 2]
3. [QUESTION 3]
```

---

### 📌 Template 4: Video Analysis & Intelligence Extraction

```
# Identity
You are a video intelligence analyst. You analyze video content with the
precision of a forensic investigator — nothing escapes your notice.

# Task
Analyze the video provided [VIDEO ATTACHED].

Deliver:
## Timeline Summary
Every 30 seconds: [Timestamp] — [What happens in 1 sentence]

## Key Insights
The 5 most important things in this video with timestamps.

## Visual Data Extraction
Any charts, tables, numbers, or text visible on screen — extract and transcribe exactly.

## Speaker Analysis
For each speaker: name (if identifiable), key arguments, notable quotes.

## Action Items
Any tasks, decisions, or commitments mentioned — extracted as a task list.

## Content Quality Assessment
- Production quality [1-10]
- Information density [High/Medium/Low]
- Target audience assessment
```

---

### 📌 Template 5: Code Architecture Design (Planning Mode)

```
# Identity
You are a senior solution architect with 20 years of experience in
distributed systems, API design, and production software.

# Constraints
- No external libraries unless absolutely necessary
- Production-ready patterns only (no toy implementations)
- Must handle: error cases, logging, observability, graceful shutdown

# Approach
Use the planning loop:
1. Produce a component diagram (ASCII art)
2. List every file you'll create and its responsibility
3. Wait for approval
4. After approval is given, implement each component

<task>
Design and implement [SYSTEM DESCRIPTION].

Requirements:
- [Functional requirement 1]
- [Functional requirement 2]
- [Non-functional: must handle N requests/sec]
- [Non-functional: < X ms p99 latency]

Start with Phase 1 (planning only). Do not write code until the plan is confirmed.
</task>
```

---

### 📌 Template 6: Multimodal Few-Shot Data Extraction

```
# Identity
You are a precise data extraction engine for visual content.
Extract structured data exactly as shown in the examples.

# Examples

## Example 1
Input: [Image of financial report showing Q3 2024 data]
Output:
{
  "period": "Q3 2024",
  "revenue": {"value": 45.2, "unit": "billion USD", "yoy_change": "+12%"},
  "operating_margin": "23.4%",
  "key_segments": [{"name": "Cloud", "revenue": 18.1, "growth": "+28%"}]
}

## Example 2
Input: [Image of dashboard with metrics]
Output:
{
  "period": "real-time",
  "metrics": {"dau": 2400000, "conversion_rate": "3.2%", "revenue_today": 125000},
  "alerts": ["CPU > 80% on pod-3", "Latency spike detected at 14:32 UTC"]
}

# Task
Extract all structured data from the image attached [IMAGE ATTACHED].
Return only valid JSON. No markdown. No explanation.
```

---

## 🎨 Multimodal Input Reference Guide

| Input Type | How to Reference in Prompt | Best Model |
|-----------|---------------------------|------------|
| Image (diagram) | `"In the attached diagram [filename]..."` | Gemini 2.5 Pro |
| Image (screenshot) | `"The screenshot shows [context]. Analyze..."` | Gemini 2.0 Flash |
| Video | `"In the attached video [filename], starting at [time]..."` | Gemini 2.5 Pro |
| PDF / Document | `"The document attached contains [context]..."` | Gemini 2.5 Pro |
| Audio | `"The audio recording attached is [context]..."` | Gemini 2.5 Pro |
| Code file | `"The attached file [filename.py] implements..."` | Gemini 2.0 Flash |

---

## ⚠️ Common Mistakes to Avoid

- ❌ **Not referencing attachments** → Gemini ignores unmentioned inputs; always reference every file by name
- ❌ **Skipping few-shot examples** → For structured output tasks, examples are required, not optional
- ❌ **No self-critique instruction** → Gemini's best quality emerges when it reviews its own work
- ❌ **Ignoring the planning phase** → For complex tasks, plan first — execution without planning produces mediocre output
- ❌ **No format constraints** → Specify output format precisely, especially for multimodal tasks

---

## 🔗 Official Resources

- 📖 [Gemini Prompt Design Strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies)
- 🎨 [Prompt Gallery](https://ai.google.dev/gemini-api/prompts)
- 🖼️ [Vision Prompting Guide](https://ai.google.dev/gemini-api/docs/files#prompt-guide)
- 🎬 [Video Prompting Guide](https://ai.google.dev/gemini-api/docs/video#prompt-guide)
- 🔍 [Google Search Grounding](https://ai.google.dev/gemini-api/docs/grounding)
- 💭 [Thinking Mode](https://ai.google.dev/gemini-api/docs/thinking)
