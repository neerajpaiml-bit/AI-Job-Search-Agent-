# 🎯 AI Job Search Agent

An automated workflow that searches for jobs matching your resume, scores each one with AI, and logs the best matches straight into a Google Sheet — no manual scrolling through job boards required.

Built with **n8n**, **OpenAI**, **Adzuna API**, and **Google Sheets**.

---

## ✨ What it does

Every day, on a schedule, this workflow:

1. 🔍 **Searches** Adzuna's job listings for roles matching your target keywords and location
2. 🧠 **Scores each job** against your resume using AI — rating fit from 0–100 with a short reason
3. ✅ **Filters** out low-relevance matches below your chosen threshold
4. 🔁 **Checks for duplicates** so the same job never gets added twice
5. 📊 **Logs new matches** to a Google Sheet with company, role, link, score, and status — ready for you to track applications

---

## 🗺️ Workflow overview

```
Schedule Trigger
      │
      ▼
HTTP Request (Adzuna API)
      │
      ▼
Split Out (individual job listings)
      │
      ▼
AI Agent (scores fit vs. resume)
      │
      ▼
Edit Fields (parses score + reason)
      │
      ▼
IF (score above threshold?)
      │
      ▼
Get Row(s) in Sheet (duplicate check)
      │
      ▼
IF (not already logged?)
      │
      ▼
Append Row (Google Sheets)
```

---

## 🧰 Tech stack

| Component        | Tool                          |
|-------------------|-------------------------------|
| Automation engine | [n8n](https://n8n.io)         |
| Job listings      | [Adzuna API](https://developer.adzuna.com) |
| AI scoring        | OpenAI API                    |
| Output            | Google Sheets                 |

---

## ⚙️ Setup

### 1. Prerequisites
- An [n8n](https://n8n.io) instance (Cloud or self-hosted)
- An [Adzuna developer account](https://developer.adzuna.com) (free) — for `app_id` and `app_key`
- An [OpenAI API key](https://platform.openai.com) with billing enabled
- A Google account with Sheets access

### 2. Create your Google Sheet
Set up a sheet with these column headers:

| Company | Job Role | Job Posted | Application Link | Status |
|---------|----------|------------|-------------------|--------|

### 3. Import the workflow
1. Download `workflow.json` from this repo
2. In n8n, go to **Workflows → Import from File**
3. Select the downloaded file

### 4. Add your credentials
- **Adzuna**: add your `app_id` and `app_key` as query parameters in the HTTP Request node
- **OpenAI**: create a credential in the AI Agent's Chat Model node
- **Google Sheets**: authorize via OAuth in both Google Sheets nodes

### 5. Add your resume
Paste your resume text into the AI Agent's prompt field — this is used as the reference point for scoring every job.

> ⚠️ **Note:** the exported workflow may contain your resume text in plain form if you've filled it in before exporting. Review before sharing publicly.

### 6. Configure your search
In the HTTP Request node, set:
- **Country**: change `us`/`in`/etc. in the URL path to your target country code
- `what` — your target job title/keywords
- `where` — city or region (optional)
- `sort_by=date` and `max_days_old=3` — recommended, so each run surfaces genuinely new listings instead of the same static results

### 7. Activate
Toggle the workflow **Active** so it runs on your chosen schedule.

---

## 📌 Customization ideas

- Adjust the **score threshold** in the IF node to be stricter or more lenient
- Swap Adzuna for another job API (JSearch, RemoteOK, etc.)
- Add a **Slack/Email notification** node for high-scoring matches
- Track application **Status** manually in the sheet as you apply

---

## 📄 License

MIT — use freely, adapt as needed.
