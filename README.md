<div align="center">

# Ultimate AI Prompt Engineering Collection

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

> **The most powerful, research-backed prompt templates for every major AI model — copy, paste, and win.**

A curated GitHub repository of production-grade prompt engineering guides for the world's most powerful AI systems. Each guide is built from official documentation, real-world testing, and advanced techniques that go far beyond basic prompting.

</div>

<br/>

## Navigate by AI Model

| AI Model | Best For | Guide |
|----------|----------|-------|
| ![Claude](https://img.shields.io/badge/Claude-D97757?style=for-the-badge&logo=anthropic&logoColor=white) | Long-form analysis, coding, structured reasoning | [→ claude/README.md](./claude/README.md) |
| ![ChatGPT](https://img.shields.io/badge/ChatGPT-74aa9c?style=for-the-badge&logo=openai&logoColor=white) | General, creative, agentic workflows | [→ chatgpt/README.md](./chatgpt/README.md) |
| ![Kimi](https://img.shields.io/badge/Kimi_K2.5-4384F4?style=for-the-badge&logoColor=white) | Bilingual research, safe/long-doc tasks | [→ kimi/README.md](./kimi/README.md) |
| ![MiniMax](https://img.shields.io/badge/MiniMax_M2-FF4B4B?style=for-the-badge&logoColor=white) | Agentic coding, 1M+ token context | [→ minimax/README.md](./minimax/README.md) |
| ![Grok](https://img.shields.io/badge/Grok-000000?style=for-the-badge&logo=x&logoColor=white) | Fast iteration, live coding, pair programming | [→ grok/README.md](./grok/README.md) |
| ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white) | Multimodal, planning, self-critique workflows | [→ gemini/README.md](./gemini/README.md) |

<br/>

## Why This Repository?

Most prompt guides are shallow. This one is different:

- **Sourced from official documentation**: No guesswork, relying only on what the creators publish.
- **Model-specific**: What works in Claude can break GPT, and vice versa. 
- **Production-ready**: All prompts are formatted for immediate copy-paste use.
- **Continuously updated**: Refined constantly as models evolve.

<br/>

## Quick Comparison: Which AI for What?

| Model | Primary Superpower |
|-------|-----------------------|
| ![Claude](https://img.shields.io/badge/Claude-D97757?style=flat-square&logo=anthropic&logoColor=white) | Writing, deep coding, 200K+ token documents, structured XML reasoning |
| ![ChatGPT](https://img.shields.io/badge/ChatGPT-74aa9c?style=flat-square&logo=openai&logoColor=white)| Versatile tasks, creative work, agent chains, JSON/tool output |
| ![Kimi](https://img.shields.io/badge/Kimi_K2.5-4384F4?style=flat-square&logoColor=white) | Bilingual (EN+ZH) research, safe summaries, recursive long-doc analysis |
| ![MiniMax](https://img.shields.io/badge/MiniMax_M2-FF4B4B?style=flat-square&logoColor=white)| Agentic coding with massive context, diff-patch, production workflows |
| ![Grok](https://img.shields.io/badge/Grok-000000?style=flat-square&logo=x&logoColor=white) | Fast code iteration, pair programming, real-time file-context coding |
| ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white) | Images, video, multimodal planning, self-critique reasoning loops |

<br/>

## Universal Prompt Engineering Principles

These principles apply across **all** AI models:

1. **Give a Role** — "You are a senior X" outperforms no role in every model tested.
2. **Use Delimiters** — XML tags, `"""`, `---` prevent context bleed.
3. **Show Examples** — Few-shot (2–5 examples) dramatically improves output quality.
4. **Specify Output Format** — Tell the model *exactly* how you want the response structured.
5. **Constrain Verbosity** — Set explicit limits: "≤ 5 bullets", "max 3 sentences".
6. **Think Out Loud** — "Think step-by-step before answering" improves reasoning across all models.

<br/>

## Repository Structure

```text
Ai_promts/
├── README.md               ← You are here (master index)
├── claude/
│   └── README.md           ← Claude prompt guide
├── chatgpt/
│   └── README.md           ← ChatGPT prompt guide
├── kimi/
│   └── README.md           ← Kimi K2.5 prompt guide
├── minimax/
│   └── README.md           ← MiniMax M2 prompt guide
├── grok/
│   └── README.md           ← Grok prompt guide
└── gemini/
    └── README.md           ← Gemini prompt guide
```

<br/>

## Contributing

Found a better technique? **Open a PR!** Please include:
- The source (official docs, paper, or reproducible experiment)
- The model version it was tested on
- A before/after example

<br/>

## License

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](https://opensource.org/licenses/MIT)

Free to use, share, and build upon.

<br/>

<div align="center">

**Star this repo** to stay updated as models and best practices evolve!

[![GitHub stars](https://img.shields.io/github/stars/user/repo.svg?style=social&label=Star)](https://github.com/user/repo)

</div>
