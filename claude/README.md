# 🟣 Claude (Anthropic) — Ultimate Prompt Engineering Guide

> **Models covered:** Claude Opus 4.6 · Claude Sonnet 4.6 · Claude Haiku 4.5  
> **Official docs:** [platform.claude.com/docs → Prompting Best Practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)

---

## 🧬 Why Claude Is Different

Claude is the most **structure-sensitive** major AI model. It natively parses XML with near-zero hallucination even at 200K+ token context windows. It prefers **flowing prose** over bullet points and rewards explicit, role-grounded system prompts. Mastering Claude means mastering XML and understanding its thinking architecture.

---

## 🔑 Core Principles (Official + Research-Backed)

### 1. 🏷️ XML Tags Are Mandatory
Claude was trained on XML-structured data. Wrapping content types in descriptive tags (`<instructions>`, `<context>`, `<task>`, `<examples>`, `<output_format>`) reduces ambiguity drastically — especially in long prompts.

**Best practices:**
- Use consistent, descriptive tag names across all your prompts
- Nest tags when content has a natural hierarchy
- Use `<documents>` with `<document index="n">` for multi-doc context

### 2. 👤 Always Assign a Role in the System Prompt
Even a single sentence makes a measurable difference:
```
"You are a senior Python engineer with 15 years of backend systems experience."
```
This focuses both Claude's tone and its knowledge retrieval strategy.

### 3. 📄 Long Context: Documents FIRST
When working with 100K+ token context, place all reference documents **before** your instructions. Claude's attention is front-weighted; instructions buried after massive documents lose fidelity.

**Order:** `<documents>` → `<instructions>` → `<task>`

### 4. 🧠 Thinking & Adaptive Reasoning
Claude Opus 4.6 uses **adaptive thinking** by default — it dynamically decides when and how much to think. Claude Sonnet 4.6 supports both adaptive and manual extended thinking.

```python
# Claude Opus 4.6 – Adaptive Thinking (recommended)
client.messages.create(
    model="claude-opus-4-6",
    thinking={"type": "adaptive"},
    output_config={"effort": "high"},  # Options: low, medium, high, max
    messages=[{"role": "user", "content": "..."}],
)
```

📌 **Prompt tip:** You can guide thinking behavior:
```
"After receiving tool results, carefully reflect on their quality
and determine optimal next steps before proceeding."
```

### 5. ⚡ Parallel Tool Calling
Claude's latest models excel at parallel tool execution. Add this to your system prompt to push parallel calling to ~100%:

```xml
<use_parallel_tool_calls>
If you intend to call multiple tools and there are no dependencies between them,
make all independent tool calls in parallel. Prioritize simultaneous calls whenever
possible to maximize speed. Never use placeholder values or guess missing parameters.
</use_parallel_tool_calls>
```

### 6. 📝 Prose Over Bullets
Claude performs better when asked to write in **flowing paragraphs** rather than bullet lists (especially for explanations). Explicitly request this in your output format tag.

---

## 💡 Verbosity & Format Control

| Goal | Instruction |
|------|-------------|
| Shorter output | `"Be concise. Maximum 3 sentences per point."` |
| No markdown | `"Respond in plain prose, no bullet points or headers."` |
| Code + explanation | `"Code first, then explain changes in 2–3 flowing sentences."` |
| Summary first | `"Begin with a 2-sentence executive summary before details."` |

---

## 🚀 Production-Ready Prompt Templates

### 📌 Template 1: Senior Code Refactoring (XML + Thinking)

```xml
<instructions>
You are a world-class senior software engineer with deep expertise in Python
performance optimization and production-grade error handling. Think step-by-step
before writing any code. Use parallel tool calls if you need to read multiple files.
</instructions>

<context>
This codebase is a high-traffic REST API serving 50,000 requests/minute.
Latency is critical. We use FastAPI + PostgreSQL + Redis.
</context>

<task>
Refactor the following code to:
1. Improve performance (identify and eliminate bottlenecks)
2. Add comprehensive error handling with proper HTTP status codes
3. Make database queries async-compatible
4. Add type annotations throughout
</task>

<code>
[PASTE YOUR CODE HERE]
</code>

<output_format>
Begin with a 2-sentence summary of the main issues found.
Then provide the full refactored code in a ```python block.
Finally, explain every change you made in flowing prose — no bullet points.
</output_format>
```

---

### 📌 Template 2: Deep Document Analysis (Long Context)

```xml
<documents>
  <document index="1">
    <title>[Document Title]</title>
    <content>
      [PASTE FULL DOCUMENT CONTENT — up to 200K tokens]
    </content>
  </document>
  <document index="2">
    <title>[Second Document]</title>
    <content>
      [PASTE CONTENT]
    </content>
  </document>
</documents>

<instructions>
You are a world-class research analyst. Your task is to synthesize the documents
above and produce a structured intelligence report.
</instructions>

<task>
1. Identify the 5 most critical insights across all documents
2. Find contradictions or tensions between sources
3. Rate confidence in each finding (High / Medium / Low) with justification
4. Produce a 3-sentence executive summary for a non-technical executive
</task>

<output_format>
Use clear section headers. Write each finding in 2–4 flowing sentences.
End with the executive summary.
</output_format>
```

---

### 📌 Template 3: Agentic System Builder

```xml
<instructions>
You are an autonomous software architect. You have access to tools: file reading,
web search, code execution, and file writing. Use parallel tool calls wherever
possible. After each tool result, reflect on what you learned before proceeding.
Think step-by-step through each decision.
</instructions>

<use_parallel_tool_calls>
When reading multiple files or performing independent searches, execute all calls
simultaneously. Never call tools sequentially when parallel execution is possible.
</use_parallel_tool_calls>

<task>
Build a complete [DESCRIBE YOUR SYSTEM] with the following requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Start by exploring the existing codebase, then implement the solution, then verify
your work with tests.
</task>

<constraints>
- Do not modify files outside the /src directory
- All new code must include type annotations
- Commit-ready: no TODOs or placeholder comments in final output
</constraints>
```

---

### 📌 Template 4: Technical Writing & Documentation

```xml
<instructions>
You are a world-class technical writer with experience at Google and Stripe.
You write documentation that is precise, developer-friendly, and scannable.
</instructions>

<examples>
  <example>
    <input>Document the `create_payment` function</input>
    <output>
## create_payment

Creates a new payment intent and returns a confirmation object.

**Parameters**
- `amount` (int): Payment amount in cents. Must be > 0.
- `currency` (str): ISO 4217 currency code (e.g., "usd").
- `customer_id` (str): Existing customer identifier.

**Returns** `PaymentIntent` — contains `id`, `status`, and `client_secret`.

**Raises** `ValidationError` if amount ≤ 0 or currency is unsupported.
    </output>
  </example>
</examples>

<task>
Write complete documentation for the following function:

[PASTE FUNCTION CODE]
</task>
```

---

### 📌 Template 5: Thinking Mode — Complex Reasoning

```xml
<instructions>
You are a world-class systems architect and strategic thinker.
Use extended thinking to reason through this problem before answering.
Show your reasoning in <thinking> tags, then provide your final answer after.
</instructions>

<task>
[COMPLEX PROBLEM — e.g., "Design a distributed rate limiter that handles
5M requests/second with < 1ms latency, geo-distributed across 3 regions,
with no single point of failure."]
</task>

<output_format>
<thinking>
[Your step-by-step reasoning process]
</thinking>

<answer>
Your final, polished recommendation in flowing prose.
Include architecture diagram in ASCII if helpful.
</answer>
</output_format>
```

---

### 📌 Template 6: Few-Shot Structured Extraction

```xml
<instructions>
You are a precise data extraction engine. Extract structured information
from unstructured text exactly as shown in the examples.
</instructions>

<examples>
  <example>
    <input>Apple reported Q3 revenue of $89.5B, up 8% YoY. iPhone sales grew 6%.</input>
    <output>{"company": "Apple", "period": "Q3", "revenue_b": 89.5, "yoy_growth": 8, "segments": [{"name": "iPhone", "growth": 6}]}</output>
  </example>
  <example>
    <input>Microsoft Q2 revenue hit $62.0B, a 15% increase. Azure grew 29%.</input>
    <output>{"company": "Microsoft", "period": "Q2", "revenue_b": 62.0, "yoy_growth": 15, "segments": [{"name": "Azure", "growth": 29}]}</output>
  </example>
</examples>

<task>
Extract structured data from the following text:

[PASTE TEXT]
</task>

<output_format>
Return only valid JSON. No explanation. No markdown fences.
</output_format>
```

---

## 🎯 Model Selection Guide

| Task | Best Claude Model |
|------|------------------|
| Complex reasoning, long documents | Claude Opus 4.6 (adaptive thinking) |
| Coding, agentic workflows | Claude Sonnet 4.6 (interleaved thinking) |
| Fast responses, simple tasks | Claude Haiku 4.5 |

---

## ⚠️ Common Mistakes to Avoid

- ❌ **Putting instructions after a 100K-token document** → Claude's attention degrades; put docs first
- ❌ **Asking for bullet points when you need nuanced analysis** → Request flowing prose explicitly
- ❌ **Vague tasks like "improve this code"** → Be specific: "Add type annotations, make DB calls async, add error handling for DB timeouts"
- ❌ **Omitting output format** → Claude will guess; always define structure
- ❌ **No role in system prompt** → Even one sentence of role context dramatically improves output quality

---

## 🔗 Official Resources

- 📖 [Claude Prompting Best Practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- 🧠 [Extended Thinking Guide](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)
- ⚡ [Adaptive Thinking](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)
- 🛠️ [Tool Use Documentation](https://platform.claude.com/docs/en/build-with-claude/tool-use)
