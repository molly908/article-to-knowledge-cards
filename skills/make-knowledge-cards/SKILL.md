---
name: make-knowledge-cards
description: Turn a pasted article or a local Markdown/TXT file into 5-8 knowledge cards, each with a title, core idea, plain explanation, example, and self-test question. Use when the user asks to make knowledge cards, 知识卡片, study cards, or flashcards from an article, notes, or a .md or .txt file. Do not use for web pages or URLs, PDF files, Anki decks, or graphical interfaces.
---

# Make Knowledge Cards

Convert one article (pasted text, or a local `.md`/`.txt` file) into study cards. Each card teaches exactly one knowledge point and contains five fields: title, core idea, plain explanation, example, self-test question.

## Input

- **Pasted text** — the article is in the user's message.
- **Local file** — a path ending in `.md` or `.txt`. Read it as UTF-8. If the path does not exist or cannot be read, ask the user instead of guessing.
- **Not supported** — web pages and URLs, PDF, Anki, graphical interfaces. If the user asks for these, say so briefly and offer to process pasted text or a `.md`/`.txt` file instead.

## Workflow

1. **Read the whole source** before picking anything. Note what the article spends the most words on, defines explicitly, or keeps returning to.
2. **List candidate knowledge points** — the concepts, mechanisms, definitions, procedures, and key claims that carry the article's value. Facts mentioned only in passing are weak candidates.
3. **Merge duplicates.** Several paragraphs that explain the same thing become one card; keep the clearest formulation and fold in supporting details.
4. **Rank by importance and pick the top 5-8 points.** Prefer points the rest of the article depends on. Split a compound point only when each part is independently worth remembering; otherwise keep just the main idea. If fewer than 5 points qualify, output fewer — the never-pad rule below wins over the 5-8 target.
5. **Write one card per point** using the template below, strictly grounded in the source.
6. **Output the cards in your reply** as Markdown, and always also save them to a file: for a file input write `<file-name>-cards.md` next to the source file; for pasted text write a kebab-case `<article-topic>-cards.md` in the current working directory. Mention the saved path in the reply.

## Card template

Write card content in the source language. Use Chinese field labels for a Chinese article, English labels otherwise:

```markdown
## 1. 标题

- **核心知识** — one sentence stating the point exactly as the article supports it.
- **简明解释** — 2-4 sentences in plain words; no jargon the article did not introduce.
- **例子** — the article's own example. If the article gives none, build a tiny illustration strictly from the article's concepts and mark it（演绎示例）. Either way it must add a detail or scene the 核心知识 sentence does not already state.
- **自测** — 1-2 questions answerable from this card alone.
```

## Quality rules

- **Ground every card in the source.** Never add facts, numbers, names, statistics, or conclusions the article does not contain. If unsure whether the source says it, leave it out.
- **One card, one knowledge point.** If a card title needs "和/and" to describe itself, it is probably two cards.
- **No duplicates.** Cards must not restate the same point from a different angle. A closing paragraph that only recaps earlier sections is a summary, not a new point — fold it into the existing cards instead of making a new one.
- **Never pad.** If the article supports fewer than 5 solid points, output fewer cards — even 1 — and tell the user why. Do not invent filler, promote trivia, or lower the bar to reach 5-8.
- **Self-test questions must be answerable from the card**, not from outside knowledge.
