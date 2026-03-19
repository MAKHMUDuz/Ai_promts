# 🧠 Ultimate AI Prompt Engineering Collection

> **The most powerful, research-backed prompt templates for every major AI model — copy, paste, and win.**

A curated GitHub repository of production-grade prompt engineering guides for the world's most powerful AI systems. Each guide is built from official documentation, real-world testing, and advanced techniques that go far beyond basic prompting.

---

## 🗂️ Navigate by AI Model

| AI Model | Best For | Guide |
|----------|----------|-------|
| 🟣 **Claude** (Anthropic) | Long-form analysis, coding, structured reasoning | [→ claude/README.md](./claude/README.md) |
| 🟢 **ChatGPT / GPT-5** (OpenAI) | General, creative, agentic workflows | [→ chatgpt/README.md](./chatgpt/README.md) |
| 🔵 **Kimi K2.5** (Moonshot AI) | Bilingual research, safe/long-doc tasks | [→ kimi/README.md](./kimi/README.md) |
| 🟠 **MiniMax M2** | Agentic coding, 1M+ token context | [→ minimax/README.md](./minimax/README.md) |
| ⚡ **Grok** (xAI) | Fast iteration, live coding, pair programming | [→ grok/README.md](./grok/README.md) |
| 🔴 **Gemini 2.5 Pro** (Google) | Multimodal, planning, self-critique workflows | [→ gemini/README.md](./gemini/README.md) |

---

## 🚀 Why This Repository?

Most prompt guides are shallow. This one is different:

- ✅ **Every technique is sourced from official documentation** (not Reddit opinions)
- ✅ **Model-specific** — what works in Claude can break GPT, and vice versa
- ✅ **Production-ready** — all prompts are formatted for immediate copy-paste use
- ✅ **Continuously updated** as models evolve

---

## ⚡ Quick Comparison: Which AI for What?

```
🟣 Claude      → Writing, deep coding, 200K+ token documents, structured XML reasoning
🟢 ChatGPT     → Versatile tasks, creative work, agent chains, JSON/tool output
🔵 Kimi        → Bilingual (EN+ZH) research, safe summaries, recursive long-doc analysis
🟠 MiniMax     → Agentic coding with massive context, diff-patch, production workflows
⚡ Grok        → Fast code iteration, pair programming, real-time file-context coding
🔴 Gemini      → Images, video, multimodal planning, self-critique reasoning loops
```

---

## 📐 Universal Prompt Engineering Principles

These principles apply across **all** AI models:

1. **Give a role** — "You are a senior X" outperforms no role in every model tested
2. **Use delimiters** — XML tags, `"""`, `---` prevent context bleed
3. **Show examples** — Few-shot (2–5 examples) dramatically improves output quality
4. **Specify output format** — Tell the model *exactly* how you want the response structured
5. **Constrain verbosity** — Set explicit limits: "≤ 5 bullets", "max 3 sentences"
6. **Think out loud** — "Think step-by-step before answering" improves reasoning across all models

---

## 📁 Repository Structure

```
Ai_promts/
├── README.md               ← You are here (master index)
├── claude/
│   └── README.md           ← Claude Opus/Sonnet/Haiku prompt guide
├── chatgpt/
│   └── README.md           ← GPT-5 / GPT-4.1 prompt guide
├── kimi/
│   └── README.md           ← Kimi K2.5 prompt guide
├── minimax/
│   └── README.md           ← MiniMax M2 prompt guide
├── grok/
│   └── README.md           ← Grok grok-code-fast-1 prompt guide
└── gemini/
    └── README.md           ← Gemini 2.5 Pro prompt guide
```

---

## 🤝 Contributing

Found a better technique? Open a PR! Please include:
- The source (official docs, paper, or reproducible experiment)
- The model version it was tested on
- A before/after example

---

## 📜 License

MIT — free to use, share, and build upon.

---

> ⭐ **Star this repo** to stay updated as models and best practices evolve!
