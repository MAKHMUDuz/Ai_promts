# 🔵 Kimi K2.5 (Moonshot AI) — Ultimate Prompt Engineering Guide

> **Models covered:** Kimi K2.5 · Moonshot V2  
> **Official docs:** [platform.moonshot.ai/docs/guide/prompt-best-practice](https://platform.moonshot.ai/docs/guide/prompt-best-practice)

---

## 🧬 Why Kimi Is Different

Kimi is built by Moonshot AI and is uniquely optimized for **bilingual (English + Chinese) workflows**, making it exceptional for international research, cross-language document analysis, and safe enterprise deployments. Kimi's signature strength is handling **extremely long documents** (up to 1M tokens in some configurations) with a recursive chunk-and-summarize methodology recommended directly in its official docs. It prioritizes safety and accuracy over verbosity.

---

## 🔑 Core Principles (Official + Research-Backed)

### 1. 👤 System Prompt Role Assignment Is Essential
Always open with a clear, specific role definition in the system message. Kimi's behavior is strongly steered by role context.

```
System: You are Kimi, a precise bilingual technical editor fluent in both
English and Mandarin with 15 years of academic research editing experience.
```

### 2. 🔲 Use Delimiters to Prevent Context Bleed
Kimi handles mixed-content prompts better when sections are clearly delimited. Use:
- XML tags: `<article>...</article>`, `<instructions>...</instructions>`
- Triple quotes: `"""..."""`
- Triple dashes: `---..---`

### 3. 📋 Step-by-Step Sequential Instructions
Kimi performs best when tasks are decomposed into ordered, numbered steps — not vague top-level goals.

**Instead of:** `"Analyze this document"`  
**Use:** `"1. Summarize in 1 sentence. 2. List 3 key risks. 3. Suggest 2 improvements."`

### 4. 🔄 Chunk-and-Recursively-Summarize for Long Documents
This is Kimi's **officially recommended technique** for long document processing:

```
Step 1: Divide the document into [N] roughly equal sections.
Step 2: Summarize each section in 3 sentences.
Step 3: Combine the section summaries.
Step 4: Synthesize a final coherent summary from the combined summaries.
```

This prevents the model from losing important early information in very long texts.

### 5. 🌍 Bilingual Switching
Kimi can seamlessly switch languages within a single response. You can instruct it to reason in one language and deliver output in another:

```
"Reason in Chinese, but provide your final answer in English."
"Summarize in English, then translate your summary to Chinese below."
```

### 6. 📌 Show Example Output
Kimi generalizes very well from a single high-quality output example. Always include one if precision is important.

---

## 💡 Format Control Techniques

| Goal | Instruction |
|------|-------------|
| Bilingual summary | `"Write summary in English first, then Chinese translation below."` |
| Risk analysis | `"List exactly 3 risks. Format: Risk: [Name] — Impact: [High/Medium/Low] — Mitigation: [1 sentence]."` |
| Safe answer | `"If unsure, say 'I cannot confirm this with high confidence' rather than guessing."` |
| Document chunking | `"Process the document in sections of ~2000 words each. Summarize each before combining."` |

---

## 🚀 Production-Ready Prompt Templates

### 📌 Template 1: Bilingual Research Synthesis

```
System: You are Kimi, a precise bilingual research analyst. You produce
structured intelligence reports in both English and Chinese.
Your outputs are always factually grounded — never speculate without flagging uncertainty.

User:
<instructions>
Analyze the article below and produce a structured research report.
Follow these steps exactly:
1. Write a 1-sentence executive summary in English
2. Identify 5 key findings — for each, rate confidence (High/Medium/Low)
3. List 3 potential risks or limitations of the findings
4. Provide your complete analysis in Chinese below the English section
</instructions>

<article>
"""
[PASTE YOUR ARTICLE OR DOCUMENT HERE]
"""
</article>
```

---

### 📌 Template 2: Long Document Recursive Summarization

```
System: You are an expert document analyst specializing in recursive summarization
of technical and academic documents. You preserve nuance while compressing length.

User:
<instructions>
I will provide a long document. Apply the recursive summarization technique:

Step 1: Mentally divide the document into 5 equal sections
Step 2: Summarize each section in exactly 3 sentences (label as Section 1–5)
Step 3: From your 5 section summaries, write a 2-paragraph synthesis
Step 4: From the synthesis, produce a single executive summary sentence

Output format:
## Section Summaries
[Section 1–5 summaries]

## Synthesis
[2 paragraphs]

## Executive Summary
[1 sentence]
</instructions>

<document>
"""
[PASTE LONG DOCUMENT HERE — up to 1M tokens]
"""
</document>
```

---

### 📌 Template 3: Technical Document Editing & Risk Analysis

```
System: You are Kimi, a world-class senior technical editor with expertise in
software architecture, system design, and academic writing. You identify both
linguistic and technical issues with equal precision.

User:
<instructions>
Review the technical document below and produce a structured editorial report.

1. Overall Quality Score: [0–10] with justification
2. Technical Issues (list each): Name → Severity [Critical/High/Medium/Low] → Fix suggestion
3. Clarity Issues (list each): Line reference → Issue → Improved version
4. Missing Information: What critical information is absent?
5. Final recommendation in 2 sentences

Be direct. Do not soften criticism.
</instructions>

<document>
---
[PASTE TECHNICAL DOCUMENT]
---
</document>
```

---

### 📌 Template 4: Cross-Language Translation with Cultural Adaptation

```
System: You are Kimi, a professional translator and cultural adaptation specialist.
You translate with precision AND adapt content for cultural appropriateness in the
target market — not just word-for-word translation.

User:
<instructions>
Translate the following content from [SOURCE LANGUAGE] to [TARGET LANGUAGE].

Translation requirements:
1. Preserve all technical terms — do not simplify
2. Adapt idioms and cultural references for natural fluency in [TARGET LANGUAGE]
3. Flag any untranslatable concepts with [NOTE: explanation]
4. After translation, provide a "Translation Notes" section listing
   every significant adaptation decision and why you made it

Length of translation must match the original ± 15%.
</instructions>

<source_text>
"""
[PASTE TEXT TO TRANSLATE]
"""
</source_text>
```

---

### 📌 Template 5: Safe & Accurate Q&A Over Documents

```
System: You are Kimi, a precise document Q&A assistant. Your answers are
grounded strictly in the provided document. You never speculate or draw from
outside knowledge unless explicitly asked.

Uncertainty protocol:
- If the answer is clearly in the document: answer directly with paragraph reference
- If the answer is partially there: answer what you can, flag what's missing
- If the answer is not in the document: say "This document does not contain
  information about [topic]. I cannot answer without additional sources."

User:
<document>
"""
[PASTE YOUR DOCUMENT]
"""
</document>

Question: [YOUR QUESTION]
```

---

## 🌐 Bilingual Prompt Strategies

Kimi's bilingual capability is genuinely native — not translation-based. Use it for:

| Use Case | Prompt Pattern |
|----------|----------------|
| International market research | `"Analyze sentiment for Chinese market, report in English"` |
| Dual-language content creation | `"Write the article in English, then localize for Chinese readers"` |
| Cross-language fact checking | `"Verify these English claims against Chinese sources"` |
| Academic translation | `"Translate, preserve technical terms, add Chinese academic conventions"` |

---

## ⚠️ Common Mistakes to Avoid

- ❌ **Long wall-of-text prompts without delimiters** → Kimi may lose section boundaries; always use `"""`, `---`, or XML tags
- ❌ **Asking for 10+ items at once** → Break complex tasks into sequential steps (1, 2, 3...)
- ❌ **Not providing an output example** → For precision tasks, one example is worth 500 words of instruction
- ❌ **Expecting Kimi to speculate** → Kimi is safety-oriented; for speculative/creative tasks, explicitly say "It's okay to hypothesize"
- ❌ **Assuming it favors either language** → Always explicitly specify if you want EN or ZH output

---

## 🔗 Official Resources

- 📖 [Kimi Prompt Best Practices](https://platform.moonshot.ai/docs/guide/prompt-best-practice)
- 🚀 [Moonshot AI Platform](https://platform.moonshot.ai)
- 🌐 [Kimi Web Interface](https://kimi.ai)
