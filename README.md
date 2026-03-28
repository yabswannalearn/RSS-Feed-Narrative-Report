# AI News Digest — n8n Workflow

Automatically pulls the latest AI news from two RSS feeds, summarizes the top stories using Google Gemini, and publishes a daily digest to Notion.

---

## What It Does

1. **Fetches RSS feeds** from two sources simultaneously:
   - [TechCrunch AI](https://techcrunch.com/category/artificial-intelligence/feed/)
   - [MIT Technology Review](https://www.technologyreview.com/feed/)

2. **Merges & sorts** all articles by publication date (newest first), then **limits** to the top 3.

3. **Aggregates** the titles and content snippets of those 3 articles into a single payload.

4. **Creates a dated Notion page** (e.g. "March 28, 2025") inside a parent page to house the digest.

5. **AI Agent** (powered by Google Gemini) does two things:
   - Writes a cohesive, plain-text narrative report summarizing all 3 stories (under 1,700 characters, no Markdown).
   - Uses a Notion tool to create individual database entries for each article, each with a **Name**, **2-sentence Summary**, and **Category** (Legal, Insurance, Real Estate, or General Automation).

6. **Appends the report** as a block to the dated Notion page created in step 4.

---

## Nodes Overview

| Node | Purpose |
|---|---|
| Manual Trigger | Starts the workflow on demand |
| RSS Read / RSS Read1 | Fetches feeds from TechCrunch and MIT Tech Review |
| Merge | Combines both feed outputs |
| Sort | Orders articles by `pubDate` descending |
| Limit | Keeps only the top 3 articles |
| Aggregate | Bundles titles + snippets into one item |
| Create a page | Creates a new dated page in Notion |
| AI Agent | Generates the report and populates the Notion database |
| Google Gemini Chat Model | LLM powering the AI Agent (`gemini-3.1-flash-lite-preview`) |
| Simple Memory | Maintains session context for the agent (key: `AgentGenius_123`) |
| Create a database page in Notion | Tool used by the agent to log individual articles |
| Append a block | Writes the final report into the Notion page |

---

## Requirements

- **n8n** (self-hosted or cloud)
- **Google Gemini API** credential (`googlePalmApi`)
- **Notion API** credential with access to:
  - A parent page (ID: `3317102bc84e80ec9bd7f0130c517d80`)
  - A database (ID: `3317102b-c84e-800f-8b2e-d5fad3773242`) with `Name`, `Summary`, and `Category` properties

---

## Trigger

This workflow is triggered **manually**. To automate it, swap the Manual Trigger for a Schedule Trigger (e.g. daily at 8 AM).
