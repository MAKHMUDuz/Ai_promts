# 🟢 ChatGPT / GPT-5 (OpenAI) — Ultimate Prompt Engineering Guide

> **Models covered:** GPT-5 · GPT-4.1 · GPT-4o · o3 / o4-mini  
> **Official docs:** [platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering)  
> **Cookbook:** [cookbook.openai.com](https://cookbook.openai.com)

---

## 🧬 Why GPT Is Different

GPT-5 is the **most literal-following** model — if you say it, it does it exactly. This is a superpower and a trap: vague prompts produce mediocre results, but hyper-specific prompts unlock world-class output. GPT excels at **agentic chains, JSON structured output, tool/function calling**, and creative tasks. Unlike Claude, it responds well to Markdown-structured system prompts with clear headers.

---

## 🔑 Core Principles (Official + Research-Backed)

### 1. 👥 Master the Message Role Hierarchy
GPT uses three message roles, each with different authority:

| Role | Purpose | When to Use |
|------|----------|-------------|
| `system` / `developer` | Sets persistent behavior, identity, constraints | Always — define persona here |
| `user` | The actual task or question | Per-turn input |
| `assistant` | Previous AI responses (for conversation history) | Multi-turn context |

📌 **Key insight:** The `developer` role (formerly `system` in GPT-4) has the highest authority in GPT-5. Always put critical constraints here.

### 2. 🎯 Few-Shot Examples Are Non-Negotiable for Precision Tasks
Provide 2–5 high-quality input/output examples inside the system message. GPT-5 generalizes remarkably well from just 3 examples.

### 3. 📍 Context Goes LAST
Unlike Claude, GPT attends to **recent tokens strongly**. Put your primary task and instructions **after** any pasted context/documents.

**Order:** `system` (role + constraints) → `user` (context/documents) → `user` (actual task/question)

### 4. 📊 Structured Output (JSON Mode)
GPT-5 supports guaranteed JSON output mode. Use it for any data extraction or API-integration task:

```python
response = client.chat.completions.create(
    model="gpt-5",
    response_format={"type": "json_object"},
    messages=[
        {"role": "system", "content": "You are a data extraction engine. Always respond with valid JSON."},
        {"role": "user", "content": "Extract key info from: [TEXT]"}
    ]
)
```

### 5. 🔧 Function/Tool Calling for Agent Workflows
GPT-5 natively supports function calling. Define tools explicitly and let GPT decide when to invoke them:

```python
tools = [{
    "type": "function",
    "function": {
        "name": "search_database",
        "description": "Search the product database for items matching a query",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Search term"},
                "limit": {"type": "integer", "description": "Max results (1-50)"}
            },
            "required": ["query"]
        }
    }
}]
```

### 6. 📏 Verbosity Control Is Critical
GPT-5 tends to over-explain. Always add explicit output constraints:

```
"Respond in ≤ 5 bullet points."
"Maximum 3 sentences per section."
"No preamble. No restatement of the question. Go directly to the answer."
```

---

## 💡 Format Control Techniques

| Goal | Instruction |
|------|-------------|
| Concise bullet list | `"Respond with exactly 5 bullets, each under 15 words."` |
| Numbered steps | `"Format as numbered steps. No prose between steps."` |
| Table output | `"Format your answer as a markdown table with columns: X, Y, Z."` |
| No filler text | `"Do not start with phrases like 'Great question!' or 'Certainly!'."` |
| JSON only | `"Return only valid JSON. No markdown. No extra text."` |

---

## 🚀 Production-Ready Prompt Templates

### 📌 Template 1: World-Class Product Strategist

```
[SYSTEM / DEVELOPER ROLE]
You are a world-class product strategist with 20 years of experience scaling
SaaS products from 0 to 10M users. You have worked at Stripe, Notion, and Linear.

# Identity
Expert in: product-market fit, growth loops, retention mechanics, B2B SaaS pricing.

# Behavior
- Always think from first principles before answering
- Provide data-driven reasoning, not opinions
- Challenge assumptions — if a premise is flawed, say so explicitly

# Output Format
1. Restate the core problem in one sentence
2. Break into 3 sub-problems
3. For each sub-problem: provide 2 solutions with pros/cons
4. Final recommendation with confidence level (High / Medium / Low)
Maximum length: 600 words. No filler. No preamble.

# Examples
User: "How do we grow to 10k users?"
Response:
Core problem: Acquiring 10k quality users without burning CAC budget.
Sub-problem 1: Acquisition channel — Solution A: SEO content (slow, compounding)
vs Solution B: paid social (fast, expensive)...
[continue with your pattern]

[USER MESSAGE]
# Task
[YOUR ACTUAL QUESTION]
```

---

### 📌 Template 2: Expert Code Review (GPT-5 Literal Mode)

```
[SYSTEM]
You are a principal-level code reviewer at a top-tier tech company (Google, Meta level).
Your reviews are precise, actionable, and educational. You never leave vague feedback.

# Review Criteria (check EVERY item)
1. Correctness: Logic bugs, off-by-one errors, race conditions
2. Performance: O(n²) traps, unnecessary DB queries, memory leaks
3. Security: SQL injection, XSS, unvalidated inputs, secrets in code
4. Readability: Naming, function length, single responsibility principle
5. Test coverage: Missing edge cases, untested error paths

# Output Format
For EACH issue found:
- Severity: [CRITICAL / HIGH / MEDIUM / LOW]
- Line reference: [line number or function name]
- Issue: [one sentence description]
- Fix: [exact corrected code snippet, ≤ 10 lines]

End with: Overall Score [1-10] and one sentence recommendation.

[USER]
Review this code:

```[language]
[PASTE YOUR CODE]
```
```

---

### 📌 Template 3: Agentic Research Chain

```
[SYSTEM / DEVELOPER]
You are an autonomous research agent. You have access to web search and code
execution tools. Complete the task below using a structured approach:

# Agent Protocol
Step 1 — PLAN: List every sub-question you need to answer
Step 2 — RESEARCH: Use search tools for each sub-question (run parallel searches)
Step 3 — SYNTHESIZE: Combine findings into a coherent answer
Step 4 — VERIFY: Identify gaps or conflicting sources
Step 5 — DELIVER: Write the final report

# Constraints
- Cite every factual claim with [Source: URL]
- If uncertain, say "Confidence: Low" rather than guessing
- Maximum report length: 800 words

[USER]
Research task: [YOUR RESEARCH QUESTION]
Target audience: [WHO WILL READ THIS]
Required depth: [executive summary / technical deep-dive / literature review]
```

---

### 📌 Template 4: Creative Writing with Style Control

```
[SYSTEM]
You are a world-class fiction writer in the style of [AUTHOR NAME / STYLE].
You write with [describe voice: sparse prose / lush description / sharp dialogue].

# Style Rules
- Show, don't tell — no explaining emotions, demonstrate them through action
- Sentence variation: mix short punches with longer, flowing sentences
- No adverbs as substitutes for strong verbs
- Every line must earn its place — delete anything that doesn't advance plot or character

# Output Format
Write exactly [N] words. No title. No author note. Just the fiction.

[USER]
Write a scene where [DESCRIBE SCENE SETUP].
Character: [NAME], [brief description]
Setting: [WHERE/WHEN]
Mood: [tense / melancholic / hopeful / etc.]
```

---

### 📌 Template 5: Structured Data Extraction (JSON Mode)

```
[SYSTEM]
You are a precise data extraction engine. Extract structured information from
the provided text and return ONLY valid JSON matching the schema below.
No markdown. No explanation. Pure JSON.

# Output Schema
{
  "entities": [
    {
      "name": "string",
      "type": "person|company|location|product",
      "mentions": "integer",
      "sentiment": "positive|neutral|negative",
      "key_facts": ["string"]
    }
  ],
  "main_topic": "string",
  "summary": "string (max 2 sentences)",
  "confidence": "high|medium|low"
}

[USER]
Extract from the following text:

"""
[PASTE TEXT HERE]
"""
```

---

### 📌 Template 6: o3 / o4-mini Reasoning Mode (Chain-of-Thought)

```
[SYSTEM]
You are an expert problem solver. Before answering, reason through the problem
carefully. Use the following thinking structure:

## Thinking Process
1. Understand: Restate the problem in your own words
2. Decompose: Break into smallest solvable units
3. Solve each unit
4. Integrate solutions
5. Verify: Check your answer against the original problem
6. Simplify: Can you express the answer more clearly?

[USER]
Problem: [YOUR HARD REASONING PROBLEM]
Show all reasoning steps before giving your final answer.
Final answer must be boxed: **[ANSWER]**
```

---

## 🎯 Model Selection Guide

| Task | Best Model |
|------|-----------|
| Complex reasoning, math, science | o3 or o4-mini (reasoning models) |
| Coding, agentic tasks, tools | GPT-5 or GPT-4.1 |
| Fast, cheap responses | GPT-4o mini |
| Multimodal (images + text) | GPT-4o |

---

## ⚠️ Common Mistakes to Avoid

- ❌ **Vague system prompts** — GPT-5 needs explicit, literal instructions; "be helpful" is useless
- ❌ **Putting context before the task** — GPT loses focus on early context; task goes last
- ❌ **Not constraining output length** — GPT-5 will over-explain without word/bullet limits
- ❌ **Skipping few-shot examples** — for precision formatting tasks, examples are essential
- ❌ **Asking for opinion without a framework** — give GPT a decision framework, not just a question

---

## 🔗 Official Resources

- 📖 [Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- 🍳 [OpenAI Cookbook](https://cookbook.openai.com)
- 🛠️ [Function Calling](https://platform.openai.com/docs/guides/function-calling)
- 📊 [Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
