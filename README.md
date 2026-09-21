# Theyond MCP

Live jobs from employer career pages, as a remote MCP server.

The connector is [https://theyond.com/mcp](https://theyond.com/mcp). Free. No signup. No auth. This repo is the registry record, not the board. The board is [theyond.com](https://theyond.com).

Add that URL to Claude, Gemini, Grok, ChatGPT, Cursor, or any client that speaks streamable HTTP. `POST` is the JSON-RPC server. A browser `GET` is the human page. JSON `GET` is 405 on purpose.

Hit Apply on the theyond.com job page. That sends you to the employer. The tools never return an employer apply link. Always show the theyond URL.

## What you can ask

- “What's hiring right now?”
- “Find data scientist jobs in Toronto”
- “Staff software engineer roles in San Francisco”
- “Remote machine learning jobs”
- “Open the theyond page so I can hit Apply”

Search or `hiring_now` first. Then `get_job` for the one they picked.

## Tools

### `search_jobs`

Up to 10 live jobs. No pagination. Need at least one of:

| Argument | What it does |
| --- | --- |
| `q` | Keywords in the title, company, or description. Not a theyond.com/jobs URL. |
| `location` | City, region, or country. |
| `company` | Employer name. Case-insensitive. `nvidia` matches NVIDIA. |
| `remote` | `true` keeps jobs that say remote, or whose text was labeled remote. |
| `seniority` | `intern`, `entry`, `mid`, `senior`, `staff_lead`, or `director`. |
| `new` | `true` searches the same just-added set as `hiring_now`. Still max 10. Not `posted_at`. |

Each row has title, company, location, `slug`, theyond `url`, `posted_at`, `theyond_verified_at`, `remote`, `salary`, and `seniority` when we have them.

`posted_at` is when the employer listed it. `theyond_verified_at` is when Theyond last saw it on the employer’s board. If it is in the result, it is live.

`salary` is the wage from the employer feed, or the amount taken from the job text when the feed had none.

### `hiring_now`

No arguments. Use this when there is no keyword.

Same companies as the homepage Hiring now chips, plus every just-added job behind those chips. Not the 25-job homepage sample. Not `posted_at`. Company pages and theyond job URLs. Empty when nothing was just added.

### `get_job`

One live job. Pass the `url` from a search row, or the `slug`. Plain-text description, plus the same card. A gone listing comes back as not live. Do not use this to search. Do not pass an employer career URL.

## Limits

30 requests a minute and 400 a day, per IP. Job research, not a bulk export.

Machine docs: [theyond.com/api/docs](https://theyond.com/api/docs) and [theyond.com/llms.txt](https://theyond.com/llms.txt). Other use: hello@theyond.com.
