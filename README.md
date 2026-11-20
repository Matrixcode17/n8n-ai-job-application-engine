# Automated AI Job Scraping & Resume Tailoring Pipeline (n8n)

An end-to-end, event-driven automation workflow built in **n8n** that scrapes targeted LinkedIn job postings, evaluates role compatibility using LLMs, generates tailored resumes in Google Docs, and logs all opportunities into a centralized Google Sheets tracker.

## System Architecture & Data Flow

1. **Automated Scraping:** Ingests a parameterized LinkedIn search URL and triggers an **Apify** scraper actor to extract job descriptions, company metadata, and application links.
2. **Batch Processing:** Uses n8n's `Loop Over Items` node (batch size: 5) to throttle execution and prevent downstream API rate-limiting.
3. **LLM Relevance Filtering:** Feeds the candidate's master resume (fetched via Google Docs API) and the scraped job description into **OpenAI** with enforced JSON output (`verdict: boolean`), filtering out unqualified or location-mismatched roles.
4. **Dynamic Resume Tailoring:** Prompts an LLM to rewrite and align resume bullet points in structured HTML, automatically generating a dedicated Google Doc per approved role.
5. **Centralized Tracking:** Appends the role metadata, direct application URL, and newly generated resume link into a **Google Sheets** tracking database.

## Tech Stack
* **Orchestration:** n8n (Self-Hosted / Cloud)
* **Web Scraping:** Apify LinkedIn Jobs Scraper
* **AI / LLM:** OpenAI API (Structured JSON Outputs & HTML Generation)
* **Storage & Output:** Google Docs API, Google Sheets API

## Setup & Installation
1. Clone this repository and import `workflow.json` into your n8n instance.
2. Configure credentials in n8n for **Apify**, **OpenAI**, **Google Docs OAuth2**, and **Google Sheets OAuth2** (see `.env.example`).
3. Update the `Set URL` node with your target LinkedIn Jobs search URL and input your master resume's Document ID in the `Get Resume` node.
4. Execute the workflow.
