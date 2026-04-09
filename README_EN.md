# Content Digestion Skill

> **Digest any long content (articles, podcasts, video transcripts) into structured notes. Two modes: Knowledge / Publishing.**

A [Claude Skill](https://www.anthropic.com/news/skills). Zero dependencies. Single markdown file.

中文 · [English](./README_EN.md)

---

## What this Skill solves

You constantly run into long content worth digesting: a 2-hour podcast, an 8000-word essay, a 3-hour interview. You probably do one of three things:

1. **Save it to Notion / Obsidian / read-later**, telling yourself you'll come back — and never do
2. **Force yourself to consume the whole thing**, then remember nothing
3. **Want to take notes**, but have no idea what the notes should look like or where they should live

This Skill makes it simple: hand Claude a piece of content, tell it whether you want to **archive** or **publish**, get back a structured note in seconds.

## Two modes

### 1. Knowledge Mode

**Use it when**: you want to file the content into your knowledge base for later retrieval.

**Output**: a markdown note with YAML frontmatter (including `summary_for_index` and `concepts_to_create` fields that map directly to Karpathy's LLM Wiki workflow), `[[wikilinks]]` on key entities and concepts in the body, plus a "Suggested Wiki Updates" section, a recommended filename, and a ready-to-use LLM agent prompt for Karpathy-style workflows.

**Drops directly into**: Obsidian, Notion, Apple Notes, a local markdown folder, the `raw/` directory of a Karpathy-style LLM Wiki, or as context fed to an LLM.

→ Full example (English): [`examples/knowledge-mode-example-en.md`](./examples/knowledge-mode-example-en.md) · Chinese version: [`examples/knowledge-mode-example.md`](./examples/knowledge-mode-example.md)

### 2. Publishing Mode

**Use it when**: you want to turn long content (a 2–3 hour podcast, a long article) into a magazine-style note with structure, logic, and human warmth.

**Two ways to use it**:
- **For yourself**: read the essence of a 2-hour podcast in 10 minutes
- **For an audience**: post directly to your social platforms or share with friends

Both uses share the same output format: a magazine-style long-form note, organized by theme (not by timeline), preserving high-impact quotes and human details. **Output language automatically follows your conversation language with Claude** — talk to it in English, get English notes; talk to it in Chinese, get Chinese notes.

→ Full example (English): [`examples/publishing-mode-example-en.md`](./examples/publishing-mode-example-en.md) · Chinese version: [`examples/publishing-mode-example.md`](./examples/publishing-mode-example.md)

## How to use it

### 1. Install

#### If you use Claude.ai (web / desktop app)

> ⚠️ Note: Skills in Claude.ai are **not** uploaded into a Project's Files or Instructions. They're a **global feature** — install once and they work across all your conversations and Projects.

**Step by step:**

1. **Make sure Code Execution is enabled**
   - Go to `Settings → Capabilities`
   - Find "Code execution and file creation" and ensure it's toggled on
   - This is a prerequisite — without it, Skills won't work

2. **Package the Skill folder as a ZIP**
   - Take the entire `content-digestion/` folder (the one you downloaded) and zip it up — don't just zip the SKILL.md file alone
   - Mac: right-click the folder → Compress
   - Windows: right-click the folder → Send to → Compressed (zipped) folder
   - Important: when unzipped, the top-level should be the `content-digestion/` folder itself, with SKILL.md inside it. If unzipping shows files directly without a parent folder, you've zipped it wrong

3. **Upload to Claude.ai**
   - Go to `Settings → Customize → Skills` (or `Customize → Skills`, depending on your plan)
   - Click the "+" button → choose "Create skill" or "Upload skill"
   - Upload your ZIP file
   - Once uploaded, the Skill will appear in your Skills list. Make sure it's **toggled on**

4. **Verify the install**
   - In any conversation, paste a piece of text and say "help me digest this"
   - If Claude asks whether you want "Knowledge mode" or "Publishing mode / read-it-fast", the Skill is working
   - If Claude just summarizes generically, the Skill didn't kick in — go back and check the steps above

#### If you use Claude Code CLI

```bash
mkdir -p ~/.claude/skills/content-digestion
cp SKILL.md ~/.claude/skills/content-digestion/
```

Claude Code will auto-discover all skills under `~/.claude/skills/` — no restart needed.

### 2. Hand Claude some content

Any of these work:
- Paste a transcript
- Upload a PDF / TXT / SRT file
- Send an article URL (Claude will fetch it)
- Send a YouTube link (Claude will try to find a third-party transcript)

### 3. Tell it which mode

If you don't say, Claude will briefly ask. If you do, any of these phrasings will trigger the right mode:

**Trigger Knowledge Mode** (file it for later):

> - "Digest this in Knowledge Mode"
> - "Process this and put it in my knowledge base"
> - "Turn this into structured notes I can search later"
> - "File this away for later reference"
> - "Make this into Obsidian-ready notes"

**Trigger Publishing Mode** (read it now or share it):

> - "Turn this into a publishable long-form note"
> - "I want to read this in 10 minutes"
> - "Summarize this as a long-form article I can post"
> - "Make this into something I can share on social"
> - "Give me a magazine-style read of this"

**Both modes**:

> - "Do both modes"
> - "First archive it, then give me a publishable version"

Done.

## A real example

The same source — David Sinclair's 2-hour interview on The Diary of a CEO about reversing aging — produces very different notes in the two modes:

| | Knowledge Mode | Publishing Mode |
|---|---|---|
| Form | YAML frontmatter + body + Wiki Updates section | Magazine-style long form |
| Length | ~3500 chars | ~5000 chars |
| Focus | Structured, retrievable, precision on key data | Narrative flow, human warmth, quote tension |
| Audience | Future-you / AI / your KB system | Now-you / your readers |
| Destination | Obsidian / Notion / Apple Notes / fed to AI | Published or read on the spot |

Both full examples are in [`examples/`](./examples/).

## Design principles

A few choices that make this Skill different from generic AI summarizers — and that **directly affect what your notes look like**.

### 1. Preserve by default, not compress by default

**What it means for you**: the notes you get back will be longer than you expect. A 2-hour podcast won't give you a 200-word summary — it'll give you a 3000–5000 word note.

**Why**: most AI summarizers default to compression — squeezing 2 hours into 5 bullet points. The information survives, but **everything that made the content worth listening to is gone**: the story that made you stop and think, the line you wanted to screenshot, the number that gave you an aha moment. Those details are *the actual reason* you spent 2 hours on it.

> **A note you'll re-read in 3 months is worth more than a summary you'll glance at once.**

### 2. Quotes wrapped in a "sandwich", not dropped raw

**What it means for you**: in Publishing Mode, every quote shows up wrapped in two editorial sentences. Before: a one-sentence lead-in. The quote itself. After: a one-sentence interpretation telling you why it matters.

**Why**: ever read articles that stitch quotes together? They read like subtitles — line after line, no one guiding you. You forget them immediately because **there's no one with judgment helping you understand**.

A good long-form note reads like a friend sitting next to you, first telling you "pay attention here," then letting you hear the line, then telling you "this is the key." That's the sandwich.

This is why the Skill works especially well on conversational content — podcasts, interviews, video transcripts.

### 3. Knowledge Mode notes are designed to be findable later

**What it means for you**: Knowledge Mode notes start with `---`-wrapped metadata, and certain words in the body are wrapped in `[[wikilinks]]`. **These aren't decoration — they're how you'll find this note 3 months from now.**

**Why**: most people's knowledge-base pain isn't *capturing*, it's *retrieving*. You save 100 Notion docs. Three months later: "where was that note about aging reversal?" You're stuck searching by keywords you've already forgotten.

The frontmatter auto-tags each note with author, date, topics, and concepts — so Notion / Obsidian / Dataview can organize your notes like a library catalog. The `[[wikilinks]]` mark recurring concepts as clickable links — so when your *next* note also mentions the same concept, **the two notes automatically connect**. Over time you get a knowledge graph, not 100 isolated files.

> **Knowledge Mode doesn't give you a pretty note — it gives you a note that grows together with your future notes.** With one note it looks like overkill. By the tenth, you'll understand.

## The "raw → markdown" step of Karpathy's LLM Wiki

> **If you don't know who Andrej Karpathy is, or you've never heard of "LLM Wiki," feel free to skip this section — this Skill doesn't depend on you understanding any of it. This section is for people who are already using (or want to use) LLMs to maintain a personal knowledge base.**

If you've read Andrej Karpathy's [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — he describes a complete workflow for using LLMs to incrementally "compile" a personal knowledge base:

1. **Data ingest** — index source documents into `raw/`, let the LLM compile a wiki
2. **IDE** — use Obsidian as the frontend
3. **Q&A** — ask questions against the wiki, LLM auto-retrieves and synthesizes
4. **Output** — render markdown / slides / charts, file them back into the wiki
5. **Linting** — LLM health checks, gap-filling, connection discovery
6. **Extra tools** — search engines and helpers

It's a complete, self-growing knowledge base system. Karpathy himself notes: "there is room here for an incredible new product instead of a hacky collection of scripts."

**This Skill addresses the most mundane step in that flow: turning a single piece of raw content (an article, a podcast, a video transcript) into a markdown file that can be dropped into a Karpathy-style `raw/` directory and immediately understood and compiled by an LLM agent.**

It does *not* maintain the wiki, build backlinks, or perform compilation — those are jobs for a larger system (or for your LLM agent working directly in Obsidian / Claude Code). What it does:

- The content you drop into `raw/` is **no longer a raw PDF / SRT file** — it's a structured markdown with frontmatter metadata and a concept-candidate list
- The `summary_for_index` field in the frontmatter feeds directly into the wiki index file in Karpathy's workflow
- The `concepts_to_create` field maps directly to the "writes articles for them" step in Karpathy's flow
- The "🧠 If you're using Karpathy's LLM Wiki workflow" section at the end of every Knowledge Mode output contains **a ready-to-use prompt** telling your LLM agent how to compile this output into the wiki

In short: **Karpathy describes 6 components. This Skill handles the most-overlooked-but-most-frequent transformation in component 1 — getting from raw source to compilable markdown. The other 5 components are out of scope — they're handled by your own LLM agent (Claude Code / Cursor / etc.) working directly in Obsidian.**

**If you're not using Karpathy's pattern**: this Skill still works for you — treat it as "produces high-quality notes that load into any markdown system."

**If you are or want to use Karpathy's pattern**: this Skill reduces the entry friction of your workflow to near zero.

## Roadmap

**v1.0.2 (current)**

- Knowledge Mode + Publishing Mode
- frontmatter (with `summary_for_index` / `concepts_to_create` fields that map to Karpathy's LLM Wiki workflow)
- wikilinks / Suggested Wiki Updates / recommended filename / ready-to-use LLM agent prompt for Karpathy-style workflows
- SRT / PDF / web / YouTube (via third-party transcripts) input
- Bilingual README

**v1.1 (planned)**

- Refine prompts and error handling based on early user feedback
- More input sources (e.g., end-to-end audio file → transcription → digestion)
- Knowledge Mode support for "user-provided wiki index" → precise integration suggestions

## What this Skill is good for

You have a valuable, long piece of content. You want to archive it, build research material, or turn it into notes you can publish. That's the core use case.

Specifically:

- **Talks, keynotes, long-form course videos** — a 90-minute hardcore talk, archived into your knowledge base or turned into a long-form post
- **High-value podcasts** — a 2-3 hour interview, turned into searchable source notes or a publishable long-read
- **Investment / industry research reports** — a 10,000-word report, the essence in 10 minutes, key data intact
- **Long articles / deep journalism / academic papers** — a piece worth reading three times, turned into notes you'll come back to
- **Cross-language**: digest in one language, output in another — non-native readers are 3-5x slower in a foreign language, so great long-form content gets perpetually procrastinated. Let the Skill produce notes in your native language and get the essence in 1/3 the time

> Get the "digest" step right — save time, but never compromise on accuracy, quality, or information density.

## What this Skill is *not*

To avoid wrong expectations:

- **Not a 200-word summarizer**. If you just want one paragraph, use a different tool
- **Not a literal translator**. If you need word-for-word bilingual translation, use a dedicated translation tool
- **Not a content curator**. This Skill assumes you've already decided the content is worth digesting
- **Not an automatic knowledge base maintainer**. The "Suggested Wiki Updates" section gives suggestions, not automation. True automated maintenance requires a larger system (like the full wiki compilation flow in Karpathy's pattern), which is out of scope for this Skill

## License

[MIT](./LICENSE). Free to use, modify, distribute, including commercially. Just keep the copyright notice.

## About

Built by [Vinson Liang](https://github.com/VinsonLiangyc).

If this Skill helps you, a star is appreciated. I'd especially love to see notes you produce with it on content I never would have thought of.

Bug reports and suggestions: open an issue.
