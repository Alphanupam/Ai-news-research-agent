# AI-Powered News Research & Ranking Agent

An end-to-end **n8n automation** that collects AI and technology news, ranks articles, generates AI summaries locally using **Ollama**, and emails a daily HTML newsletter.

## Features

- Aggregates articles from **TechCrunch**, **MIT Technology Review**, and **MarkTechPost**.
- Extracts links and article text using **Jina Reader**.
- Scores articles with custom **JavaScript** based on recency, source authority, impact, topic interests, and penalties. Minimum qualification score: **54**.
- Selects **up to 25** qualified articles for the newsletter.
- Cleans extracted text and checks readiness before AI summarization.
- Uses **Ollama / Llama 3.2** for summaries, key takeaways, and why each article matters.
- Preserves original article titles, source names, dates, and URLs.
- Generates a styled **HTML email newsletter** with links to full articles.
- Supports scheduled delivery through **Gmail**.

## Architecture

```text
Schedule Trigger
  -> Initialize News Sources & Run IDs
  -> Jina AI Reader
  -> Filtration — Extract article URLs
  -> Score all articles V4 (threshold 54)
  -> Top 25 articles qualified
  -> Jina AI extraction
  -> Cleaning
  -> If (READY_FOR_LLM)
  -> OLLAMA Prompt ready
  -> Message a model (llama3.2:latest)
  -> Build AI Newsletter
  -> Gmail — Send a message
```

## Tech stack

| Technology | Role |
| --- | --- |
| n8n | Workflow automation and scheduling |
| JavaScript | Filtering, ranking, cleaning, and HTML generation |
| Jina Reader | News extraction |
| Ollama + Llama 3.2 | Local AI summarization |
| Gmail | Newsletter delivery |

## Getting started

1. Install or access **n8n** and ensure it can reach an **Ollama** server.
2. Install Ollama and run `ollama pull llama3.2`.
3. Download and import [workflow.json](workflow.json) into n8n.
4. Configure an **Ollama** credential in the `Message a model` node.
5. Configure a **Gmail OAuth2** credential in `Send a message` and replace `YOUR_EMAIL@example.com` with your email address.
6. Verify workflow timezone **Asia/Kolkata** and the Schedule Trigger (export currently sets **9 AM IST**).
7. Test manually, verify the newsletter, then publish/activate the workflow.

**This repository is an importable workflow, not a hosted web app.** A running n8n instance, reachable Ollama model, and configured Gmail account are required.

## Important implementation notes

- Recency scoring uses article URL dates relative to the newest article in the current batch, rather than enforcing a strict 24-hour news window.
- Fewer than 25 articles may be delivered if too few pass ranking or extraction checks.
- The exported IF node routes the false branch back to Cleaning; review this loop before production deployment.
- Newsletter summaries are matched to upstream article metadata by item order; keep the item ordering consistent.
- Long newsletters may be clipped by Gmail.
- AI-generated summaries should be verified against the original linked sources.

## Security

The public export removes Gmail recipient details, n8n credential references, webhook identifiers, instance metadata, and stored execution data. Configure credentials **inside your own n8n instance**. Never commit API keys, OAuth tokens, passwords, or real `.env` files.

## Resume description

**AI-Powered News Research & Ranking Agent | n8n, JavaScript, Ollama, Jina AI, Gmail**

Built a scheduled AI news automation workflow that collects articles from three publications, ranks and selects up to 25 using custom JavaScript scoring, generates local LLM summaries, and delivers a formatted HTML email newsletter.
