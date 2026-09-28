# CCAR-F Prep Kit

Free, unofficial study material for the **Claude Certified Architect – Foundations (CCAR-F)** exam: a one-page cheat sheet and three full-length practice exams that run in your browser.

> **Unofficial.** Not affiliated with or endorsed by Anthropic. All scenarios and questions are original and written from the public exam guide. Nothing here is recalled from the live exam.

## What's inside

| File | What it is |
|---|---|
| [`ccar-f-cheat-sheet.md`](ccar-f-cheat-sheet.md) | Exam facts, study plan, the failure → fix table with common traps, Claude Code and API quick rules, a hands-on checklist, and the tested objectives as a tick-list |
| [`exams/001-ccar-f-mock-exam.html`](exams/001-ccar-f-mock-exam.html) | **Practice Examination.** 60 questions, 4 scenarios × 15. Timed mode (120 min, auto-submit) or untimed study mode with instant grading. Progress auto-saves in your browser. |
| [`exams/002-ccar-f-mock-exam.html`](exams/002-ccar-f-mock-exam.html) | **Full Simulation.** 60 questions across five production systems, weighted to the blueprint and scaled to 1,000. Includes two "Select TWO" items. Progress lives in the tab only, so don't refresh. |
| [`exams/003-ccar-f-mock-exam.html`](exams/003-ccar-f-mock-exam.html) | **Hard Mock.** 60 harder questions with per-option rationales and a per-domain score report. Progress auto-saves in your browser. |

## How to use the exams

1. Download the `.html` file (or clone the repo).
2. Open it in any modern browser. No install, no server, no account.
3. Take it closed-book under exam conditions: 120 minutes, no notes, no AI.
4. Review every miss and write down **which principle** it broke. The pattern matters more than the score.

Rough readiness guide: about **48+/60** is on track, **42–47** is borderline, under 42 means keep studying. The pass mark is 720/1000 (~72%).

**Keyboard:** `1`–`4` or `A`–`D` to select · `←` `→` to move · `F` to flag (varies slightly per exam).

## Suggested study order

1. Read the **official exam guide** end to end, including the sample rationales.
2. Do the **Anthropic Academy** courses: Claude 101, AI Fluency, Building with the Claude API, Claude Code in Action, Introduction to MCP. Skip Bedrock and Vertex.
3. **Build things.** Work through the hands-on checklist in the cheat sheet (hooks, an MCP server, slash commands, a forced tool call, an Agent SDK pipeline).
4. Take the mocks here and elsewhere, then drill only the patterns you miss.
5. Take the **official practice exam last** as your readiness check.

## Exam at a glance

| | |
|---|---|
| Format | 60 scenario-based questions, 120 min, multiple choice + multiple response |
| Pass mark | 720 on a 100–1,000 scale |
| Scenarios | 4 drawn from a bank of 6 |
| Domains | D1 Agentic Architecture 27% · D2 Tool Design & MCP 18% · D3 Claude Code 20% · D4 Prompt Eng & Structured Output 20% · D5 Context & Reliability 15% |
| Delivery | Pearson VUE, proctored. **Your registered name must exactly match your government ID.** |

## Privacy

The exams are single, self-contained HTML files. They make no network requests except loading Google Fonts. Two of them save your answers in your own browser's `localStorage` so you can resume later. Nothing is sent anywhere.

## Contributing

Found a wrong answer, a weak rationale, or something that contradicts Anthropic's guidance? Open an issue or a pull request and cite the source (exam guide section or Anthropic docs). Please **don't** submit questions remembered from the real exam.

## License

Choose a license before publishing (e.g. [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) for the content).

---

Prepared by Matloob Altaf. Good luck.
