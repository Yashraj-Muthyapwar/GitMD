<div align="center">
  
# GitMD

**Paste a GitHub URL. Get clean, structured markdown in seconds.**

No sign-ups. No configuration. Just works.

[![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=flat-square&logo=next.js&logoColor=white)](https://nextjs.org/)
[![Gemini AI](https://img.shields.io/badge/Gemini_AI-4285F4?style=flat-square&logo=google&logoColor=white)](https://deepmind.google/technologies/gemini/)
[![Upstash Redis](https://img.shields.io/badge/Upstash_Redis-DC382D?style=flat-square&logo=redis&logoColor=white)](https://upstash.com/)
[![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)](https://vercel.com/)
[![GitHub API](https://img.shields.io/badge/GitHub_API-181717?style=flat-square&logo=github&logoColor=white)](https://docs.github.com/en/rest)


<a href="https://gitmd.org" target="_blank" rel="noopener noreferrer">
  <img src="images/homepage.png" alt="GitMD Homepage" width="700" />
</a>

[![Try it live](https://img.shields.io/badge/Try_it_live-gitmd.org-b8f040?style=for-the-badge)](https://gitmd.org)

</div>



## What is this?

Most of the time when you land on a GitHub repo you've never seen, the README either doesn't exist or was written by someone who already knew the codebase and forgot to explain the obvious parts. So you spend 20 minutes just orienting yourself before you can do anything useful.

GitMD is a tool I built to fix that. You paste a GitHub URL, it reads the repo's file structure and picks the files that actually reveal how the project is put together, then writes a markdown doc that covers the architecture, the key dependencies, where to start reading, and how to contribute. Cold generation takes 10-15 seconds. If someone ran the same repo earlier today, it's instant.

## How it works

```
Paste a GitHub URL
        │
        ▼
  Redis cache check
        │
   ┌────┴──────────────────────┐
   │ HIT                       │ MISS
   ▼                           ▼
Instant response          GitHub API fetch
                          (tree, README, metadata)
                                │
                                ▼
                       Stage 1 — Smart Selector
                       Identifies the 8-12 files
                       that best explain the codebase
                                │
                                ▼
                       Stage 2 — Doc Writer
                       Writes the markdown doc
                       from the selected context
                                │
                                ▼
                       Cache for 24 hours
                       Stream to client
```

One big prompt over an entire repo produces shallow, generic output. Stage 1 runs a triage pass first — it looks at the file tree, detects the repo type (monorepo, CLI, ML library, platform running on Docker services, and so on), then picks the files worth feeding to Stage 2. The second stage writes from that focused set instead of from everything at once. On small repos the difference is subtle. On large ones it's obvious.

## What you get

The output is a single markdown file structured for four different readers at once: an engineer who needs architecture depth fast, a newcomer who needs plain-English orientation, a learner who wants to understand what patterns the codebase teaches, and an AI agent that needs structured metadata.

In practice that means:

- A one-liner on what the project does and who it's for
- The actual problem it was solving before it existed, not marketing copy
- The data flow traced through real filenames from the repo, not generic diagrams
- The two or three dependencies the whole thing falls apart without, and why
- Every top-level directory explained — what lives there, not just the folder name
- Separate file picks for engineers vs learners, pointing at actual source files
- Getting started instructions from the real config files
- YAML metadata block for AI tooling and automation

## Tested against hard repos

| Repo | What makes it hard | Result |
|---|---|---|
| `vercel/next.js` | 100k+ files, Rust + JS, large monorepo | Rust engine files surfaced alongside the JS framework source |
| `supabase/supabase` | Backend is external Docker services, not TypeScript source | `docker-compose.yml` used as the architecture document |
| `huggingface/transformers` | Docs outnumber source files by a wide margin | Engineers pointed at `.py` source, not `.md` docs about source |
| `langchain-ai/langchain` | Nested monorepo, all packages under one `libs/` parent | Coverage spread across `libs/core`, `libs/langchain`, `libs/community` |
| `fastapi/fastapi` | `docs_src/` tutorial files look like real source to a naive selector | Tutorial directories excluded from engineer entry points |
| `cli/cli` | Pure Go CLI, no frontend layer | Entry point, command router, and API client all picked correctly |

## Feedback

If a specific repo produces bad output, [open an issue](https://github.com/Yashraj-Muthyapwar/GitMD/issues/new) and include the URL. That's genuinely the most useful thing you can send.

<div align="center">
Built by <a href="https://github.com/Yashraj-Muthyapwar">Yashraj</a> &nbsp;·&nbsp; <a href="https://gitmd.org">gitmd.org</a>
</div>
