# AI Virtual Tutor (DataMind)

A fully autonomous, multi-agent AI tutor built for postgraduate data analytics students. Unlike a single-model chatbot, it dynamically selects from five specialised tools based on user intent, maintains session memory, and automatically emails a personalised progress report at the end of each session.

## What it does

A student chats with the tutor about anything related to their coursework — asking it to explain a concept, quiz them, research a topic, or generate an exam-style assessment. The agent reasons about what's being asked and routes the request to the right specialised tool rather than answering everything the same way.

When the student says "done for today," the workflow automatically summarises the session, formats it as HTML, and emails a progress report — no manual step required.

## Architecture

- **Reasoning & orchestration:** Claude Sonnet (Anthropic) acts as the decision-making layer — it interprets intent and chooses which tool to invoke.
- **Automation backbone:** n8n handles the workflow logic, routing, and tool orchestration.
- **Session memory:** Simple Memory node maintains continuity across a conversation.
- **Formatting:** A JavaScript/Markdown-to-HTML step converts agent output into clean, styled email content.
- **Delivery:** Gmail API sends the automated end-of-session report.

### The five tools

| Tool | Purpose | Backing API |
|---|---|---|
| Search | Live web search for current trends/tools, filtered for data-analytics relevance | SerpAPI → Google |
| Teacher | Structured explanations, definitions, worked examples adapted to MSc depth | OpenAI |
| Quiz | Scenario-based MCQ/short-answer questions mapped to Bloom's Taxonomy | OpenTDB + OpenAI |
| Research | Multi-source academic-level summaries for coursework/dissertations | OpenAI |
| Assessment | Formal, exam-style questions with marking criteria | OpenAI |

### Workflow steps

1. Student sends a message via the chat trigger
2. Claude Agent reasons about intent and selects a tool
3. An IF node checks whether the session is complete
4. Response is formatted from Markdown to HTML
5. A task router directs the query to the correct tool node
6. At end of session, Gmail sends a personalised daily progress report automatically

## Status

MVP complete and live — all five tool nodes, session memory, HTML formatting, and automated Gmail reporting are built, connected, and tested.

## How to import this workflow

1. Download `AI Virtual Tutor.json` from this repo.
2. In n8n, go to **Settings → Import from File** (or use the **⋯ menu → Import Workflow** on the canvas).
3. Select the downloaded JSON file.
4. Reconnect your own credentials for Claude (Anthropic), SerpAPI, OpenAI, and Gmail — credentials are not included in the export for security reasons.
5. Activate the workflow to generate a live Production URL for the chat trigger.

## Roadmap

- **Phase 1 (complete):** Core multi-tool agent for data analytics, powered by Claude Sonnet + n8n
- **Phase 2:** Multi-subject expansion — computing, business, healthcare, and law as domain plugin modules
- **Phase 3:** Institutional licensing — white-label university deployment, academic dashboards, student analytics portals
- **Long-term:** Global SaaS platform combining B2C subscriptions and B2B corporate training
