<div align="center">

# ✦ GitMD ✦

### Paste a GitHub URL. Get clean, structured markdown in seconds.

*No sign-ups. No configuration. No waiting.*

<a href="https://gitmd.org">
  <img src="https://img.shields.io/badge/Try%20it%20Live-%E2%86%92-b8f040?style=for-the-badge&labelColor=0d0d0d" alt="Try it live" />
</a>
<br/>
<br/>

<a href="https://gitmd.org">
  <img src="images/homepage.png" alt="GitMD — paste a URL, get markdown" width="720" />
</a>

<br/>
<br/>

<img src="https://img.shields.io/badge/Next.js_16-000000?style=flat-square&logo=next.js&logoColor=white" />
<img src="https://img.shields.io/badge/Gemini_AI-4285F4?style=flat-square&logo=google&logoColor=white" />
<img src="https://img.shields.io/badge/Upstash_Redis-DC382D?style=flat-square&logo=redis&logoColor=white" />
<img src="https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white" />
<img src="https://img.shields.io/badge/GitHub_API-181717?style=flat-square&logo=github&logoColor=white" />

</div>
<br>

<details>
  <summary><strong>📽️ CLICK FOR DEMOs</strong></summary>

  ## DEMO 1:
  ![GitMD demo](images/demo.gif)

  ## DEMO 2:
  ![GitMD demo](images/demo2.gif)
</details>


## What is this?

Most of the time when you land on a GitHub repo you've never seen, the README either doesn't exist or was written by someone who already knew the codebase and forgot to explain the obvious parts. So you spend 20 minutes just orienting yourself before you can do anything useful.

GitMD is a tool I built to fix that. You paste a GitHub URL, it reads the repo's file structure and picks the files that actually reveal how the project is put together, then writes a markdown doc that covers the architecture, the key dependencies, where to start reading, and how to contribute.

Cold generation takes **15–20 seconds**. If someone ran the same repo earlier today, it's **instant**.

## How it works

<details>
<summary><b>See the full data flow ↓</b></summary>

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
                       Identifies the 8–12 files
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

</details>

One big prompt over an entire repo produces shallow, generic output. Stage 1 runs a triage pass first — it looks at the file tree, detects the repo type (monorepo, CLI, ML library, platform running on Docker services, and so on), then picks the files worth feeding to Stage 2. The second stage writes from that focused set instead of from everything at once.

On small repos the difference is subtle. On large ones it's obvious.

## What you get

Every generated doc is structured for four readers at once:

| Reader | What they need | What they get |
|---|---|---|
| 🧑‍💻 Engineer | Architecture depth, fast | Data flow, key abstractions, exact file paths |
| 🌱 Newcomer | Plain-English orientation | What it does, why it exists, how to run it |
| 📚 Learner | Patterns worth studying | What the codebase teaches, where to start |
| 🤖 AI Agent | Structured metadata | YAML block with repo facts, entry points, deps |

In practice, every output includes:

- What the project does and who it's for — in one sentence, not a paragraph
- The actual problem it was solving before it existed, not marketing copy
- Data flow traced through real filenames from the repo
- The two or three dependencies the whole thing falls apart without, and why
- Every top-level directory explained — what lives there, not just the folder name
- Separate reading paths for engineers and learners, pointing at real source files
- Setup steps from the actual config files, not guessed
- Machine-readable YAML for AI tooling

## Tested against hard repos

These are the repos that break naive documentation tools. GitMD handles all of them.

| Repo | Why it's tricky | Result |
|---|---|---|
| `vercel/next.js` | 100k+ files, Rust + JS, large monorepo | Rust engine files surfaced alongside JS framework source |
| `supabase/supabase` | Backend is external Docker services, not TypeScript | `docker-compose.yml` treated as the architecture document |
| `huggingface/transformers` | Docs outnumber source files by a wide margin | Engineers pointed at `.py` source, not `.md` docs |
| `langchain-ai/langchain` | Nested monorepo, all packages under one `libs/` parent | Coverage spread across `libs/core`, `libs/langchain`, `libs/community` |
| `fastapi/fastapi` | `docs_src/` tutorials look like real source to a naive selector | Tutorial directories excluded from engineer entry points |
| `cli/cli` | Pure Go CLI, no frontend layer | Entry point, command router, and API client all picked correctly |

## Feedback

Found a repo where the output is wrong or clearly off?

**[→ Open an issue](https://github.com/Yashraj-Muthyapwar/GitMD/issues/new)** with the repo URL. That's the most useful thing you can send.

<p align="center">
  <em>
    Built by <a href="https://github.com/Yashraj-Muthyapwar"><b>Yashraj</b></a> ·
    <a href="https://gitmd.org">gitmd.org</a> ·
    <a href="https://gitmd.org/privacy">Privacy</a> ·
    <a href="https://gitmd.org/terms">Terms</a>
  </em>
</p>

<p align="center">
  <em>If GitMD saved you time, a ⭐ on this repo goes a long way.</em>
</p>


</div>
