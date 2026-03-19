# 🟠 MiniMax M2 — Ultimate Prompt Engineering Guide

> **Models covered:** MiniMax M2 · MiniMax M2.5  
> **Official API docs:** [platform.minimax.io/docs](https://platform.minimax.io/docs)

---

## 🧬 Why MiniMax Is Different

MiniMax M2.5 is a frontier model built for **agentic coding and production workflows** at extreme scale. Its standout feature is a **1M+ token context window** — the largest among mainstream models — combined with native tool-use architecture. Unlike other models, MiniMax benefits from a **"diff-first"** prompting philosophy: always ask for the minimal patch, never a full rewrite. This makes it exceptionally efficient for real-world code review, production bug fixes, and long-horizon agent tasks.

---

## 🔑 Core Principles (Official + Research-Backed)

### 1. 🩹 Diff-First Approach (The #1 MiniMax Technique)
Never ask MiniMax for a full code rewrite when you can ask for a minimal patch. This:
- Reduces token consumption dramatically
- Produces cleaner, reviewable changes
- Aligns with how production codebases actually evolve

```
❌ "Rewrite this entire authentication module"
✅ "Provide a minimal diff to add JWT refresh token support to auth.py"
```

### 2. 🎯 Short & Task-Focused Prompts
Paradoxically, even though MiniMax handles 1M tokens, its prompts should be **short and sharp**. The context window is for your *input data* (code, documents, logs), not for verbose instructions. Keep system prompts under 200 words; let the context data be long.

### 3. 🔧 Tool-Native Agent Mode
MiniMax is built for multi-step agent workflows. Define tools clearly and let it orchestrate:
- Searching codebases
- Running test suites
- Writing files
- Querying APIs

### 4. 📋 Role + Intent Pattern
Always give both **who you want it to be** AND **why this task matters**. The "why" (intent) improves output alignment significantly.

```
"You are a senior security engineer. The intent is to find and fix all
SQL injection vulnerabilities before a production deployment tomorrow."
```

### 5. 🗂️ Leverage the 1M Token Window Strategically
Use the massive context window for:
- Entire codebase analysis (paste full repos)
- Complete log file debugging
- Multi-document contract review
- Long conversation history retention

---

## 💡 Format Control Techniques

| Goal | Instruction |
|------|-------------|
| Minimal change | `"Provide a minimal diff. Do not refactor unrelated code."` |
| Patch format | `"Output as unified diff format (--- a/ +++ b/)"` |
| Risk classification | `"Tag each issue: [CRITICAL], [HIGH], [MEDIUM], [LOW]"` |
| No over-engineering | `"Fix only what is asked. Do not add new features or patterns."` |

---

## 🚀 Production-Ready Prompt Templates

### 📌 Template 1: Surgical Code Review (Diff-First)

```
System: You are a principal-level code reviewer focused on correctness, security,
and performance. Your intent is to prepare this code for production deployment.
Be surgical — identify only real issues, not stylistic preferences.

User:
Review the following code diff and produce a structured report:

<diff>
[PASTE YOUR GIT DIFF OR CODE CHANGES HERE]
</diff>

Report format:
## Correctness Risks
For each issue: [SEVERITY] Line X: [Issue] → [Recommended fix as code snippet]

## Performance Hotspots
For each: [Function/Block] — [Current O(n)?] → [Recommended approach]

## Security Review
For each finding: [CWE-XXX if applicable] — [Issue] — [Remediation]

## 3-Line Patch Suggestions
For the top 3 issues, provide the minimal possible fix in ≤ 3 lines each.

## Overall Verdict
[SHIP / SHIP WITH MINOR FIXES / DO NOT SHIP] + 2-sentence justification
```

---

### 📌 Template 2: Whole-Codebase Architecture Analysis (1M Context)

```
System: You are a world-class software architect. Your intent is to evaluate
this codebase for a major refactoring decision.
Focus on: coupling, cohesion, scalability bottlenecks, and technical debt.

User:
Analyze the entire codebase pasted below. It represents our current monolith.
We are considering migrating to microservices. Give me a decision framework.

<codebase>
[PASTE ENTIRE REPO CONTENTS — MiniMax handles 1M+ tokens]
</codebase>

Deliver:
1. Dependency graph summary (which modules depend on which)
2. Top 5 coupling violations (most dangerous for microservices migration)
3. 3 candidate microservice boundaries with rationale
4. Migration risk score [1-10] with breakdown
5. Recommended first extraction + minimal diff sketch
```

---

### 📌 Template 3: Production Debugging with Full Log Context

```
System: You are a senior site reliability engineer (SRE). Your intent is to
find the root cause of a production incident as quickly as possible.
Be direct, skip pleasantries, go straight to diagnosis.

User:
We have an active production incident. Here are the full logs:

<logs>
[PASTE FULL LOG OUTPUT — thousands of lines are fine]
</logs>

<error_report>
Service: [NAME]
Impact: [WHAT IS BROKEN]
Started: [TIMESTAMP]
Error message: [EXACT ERROR]
</error_report>

Produce:
1. Root cause hypothesis (ranked by probability, top 3)
2. For each hypothesis: evidence from the logs that supports/contradicts it
3. Immediate mitigation steps (can be applied in < 5 minutes)
4. Permanent fix as a minimal diff
5. Monitoring/alerting gaps that let this happen
```

---

### 📌 Template 4: Agentic Feature Implementation

```
System: You are an autonomous senior engineer. You have access to file system
tools, code execution, and search. Your intent is to implement the requested
feature end-to-end in production-quality code.

Agent protocol:
Step 1: Read relevant files to understand the codebase pattern (use tools in parallel)
Step 2: Design the implementation — describe your approach before writing code
Step 3: Implement with minimal changes to existing code (diff-first principle)
Step 4: Write tests covering normal path + edge cases + error cases
Step 5: Self-review: check correctness, performance, security before finishing

User:
Implement the following feature: [FEATURE DESCRIPTION]

Constraints:
- Match the existing code style exactly
- Provide only the changes needed — no refactoring of unrelated code
- All new code must have ≥ 80% test coverage
- No new dependencies unless absolutely necessary
```

---

### 📌 Template 5: Multi-Document Contract / Legal Review

```
System: You are a senior contract attorney with expertise in software licensing,
SaaS agreements, and data processing terms. Your intent is to identify all
material risks before contract signature.

User:
Review all documents below for the following:

<documents>
  <document id="1" label="Master Service Agreement">
  [PASTE MSA]
  </document>
  <document id="2" label="Data Processing Agreement">
  [PASTE DPA]
  </document>
  <document id="3" label="Order Form">
  [PASTE ORDER FORM]
  </document>
</documents>

Deliver:
1. Critical risks [must negotiate before signing]
2. Standard risks [negotiate if possible]
3. Acceptable clauses [no action needed]
4. Internal conflicts between documents (Doc 1 vs Doc 2 on topic X)
5. Missing standard clauses (what's absent that should be there)
6. Negotiation priority list (top 5 items, ranked)
```

---

## 🧩 MiniMax Agent Architecture

MiniMax excels when operating as an orchestrated agent. Recommended tool stack:

```json
{
  "tools": [
    {
      "name": "read_file",
      "description": "Read file contents from the repository"
    },
    {
      "name": "write_file",
      "description": "Write or overwrite a file"
    },
    {
      "name": "run_tests",
      "description": "Execute the test suite and return results"
    },
    {
      "name": "search_codebase",
      "description": "Semantic search over the entire codebase"
    }
  ],
  "agent_instructions": "Use diff-first approach. Read before writing. Run tests after every change. Never modify files outside the designated scope."
}
```

---

## ⚠️ Common Mistakes to Avoid

- ❌ **Asking for full rewrites** → Always ask for minimal diffs; MiniMax produces cleaner code this way
- ❌ **Long, verbose system prompts** → Keep system prompts short; put long data in context
- ❌ **Not specifying intent** → Always say *why* the task matters (e.g., "production deploy tomorrow")
- ❌ **Ignoring the 1M context** → Don't chunk your data unnecessarily; paste entire codebases/logs
- ❌ **No output format constraints** → MiniMax can be verbose; specify exact report format

---

## 🔗 Official Resources

- 📖 [MiniMax Platform Docs](https://platform.minimax.io/docs)
- 🚀 [MiniMax API Reference](https://platform.minimax.io/docs/api)
- 🎙️ [Speech Prompt Guide](https://platform.minimax.io/docs/guides/prompt/speech-prompt)
