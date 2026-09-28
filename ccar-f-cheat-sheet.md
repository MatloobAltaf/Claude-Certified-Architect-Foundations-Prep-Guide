# Claude Certified Architect – Foundations Cheat Sheet

*Prepared by: Matloob Altaf*

## 1. The exam at a glance

| | |
|---|---|
| **Format** | 60 scenario-based questions · 120 min · multiple choice + multiple response (count stated) |
| **Pass mark** | 720 on a 100–1,000 scale (~72%) |
| **Delivery** | Pearson VUE, proctored |
| **Scenarios** | 4 drawn from a bank of 6:<br>• Customer support agent<br>• Code generation with Claude Code<br>• Multi-agent research<br>• Developer productivity tools<br>• Claude Code in CI/CD<br>• Structured data extraction |
| **Domains** | • D1 Agentic Architecture 27%<br>• D2 Tool Design & MCP 18%<br>• D3 Claude Code 20%<br>• D4 Prompt Eng & Structured Output 20%<br>• D5 Context & Reliability 15% |
| **Validity** | 12 months |
| **Before booking** | **Your registered name must exactly match your government ID, or you will be turned away.** |

## 2. Study plan (in this order)

1. **Official exam guide** — read end to end, including the sample rationales.
2. **Anthropic Academy courses** — Claude 101, AI Fluency, Building with the Claude API, Claude Code in Action, Introduction to MCP. ***Skip Amazon Bedrock and Google Vertex (no exam weight).***
3. **Build things.** Most answers come from hands-on use of Claude Code, the Agent SDK, MCP servers and the Claude API (see section 7).
4. **2–3 free mocks.** For every miss, write down which principle it broke.

**Free mocks:**

- <https://claudecertificationguide.com/> (medium, has Drill Mode)
- [github.com/paullarionov/claude-certified-architect](https://github.com/paullarionov/claude-certified-architect) (medium)
- <https://certsafari.com/anthropic> (large bank)
- [github.com/hong-chu/claude-certified-architect-foundations-llm-wiki](https://github.com/hong-chu/claude-certified-architect-foundations-llm-wiki) (have Claude quiz you from it)
- My own Mocks

> **Caution:** third-party banks are for recall only. Some teach reasoning that **contradicts Anthropic's guidance**. Learn principles from official material.

## 3. The one rule

> **Pick the cheapest mechanism that reliably handles the failure at its actual severity.**

Every question gives a broken system and four plausible fixes. Typically one is too soft (a stronger prompt), one too hard (new machinery), one too late (catch it afterwards) and one proportional.

## 4. Failure → fix

| If the problem is… | The answer is… | Common trap |
|---|---|---|
| A rule that must always hold (money, compliance, irreversible) | Hook, programmatic gate or settings permission | Stronger prompt, routing classifier, temperature 0 |
| A recoverable best practice (e.g. backup before refactor, repo in git) | CLAUDE.md / prompt instruction; ~90% compliance is fine | "Convert everything to hooks for consistency" |
| Code must always be formatted / linted / tested after edits | PostToolUse hook | Asking the model to remember |
| Vague rule applied inconsistently | Explicit criteria | More emphasis in the prompt |
| Inconsistent style or judgment (tone, severity labels, error-handling style) | Few-shot examples with reasoning, across varied cases | Lint gate, rewrite agent, temperature 0 |
| Must return structured output / must call a specific tool | Tool use + JSON schema + `tool_choice: {"type":"tool","name":"…"}` | `"any"` (can pick the wrong tool), few-shot, "reply only in JSON" |
| Output must start a fixed way, no preamble | Prefill the assistant turn | Instructions, regex cleanup |
| Model invents values for missing fields | Optional / nullable fields, enums with "other"/"unknown" | "Don't hallucinate" in the prompt |
| Output is valid but wrong | Semantic validation | Schema validation only |
| Step X sometimes errs and step Y consumes it | Validate at the boundary; divert failures to review | More few-shot upstream, merging steps, majority voting |
| Wrong tool chosen between similar tools | Expand tool descriptions: use cases, input formats, "use this, not X, when…". Rename only if the names themselves collide | Priority rules in the system prompt |
| Deciding which extractions a human reviews | Route by confidence, document characteristics and field-level ambiguity; use stratified sampling to calibrate confidence | Random sampling |
| Tool call fails | Structured error: type, retryable or not, suggested next action | Generic "failed", or returning an empty result |
| Customer asks for a human | Escalate immediately with a context summary | Solving it first because it's quick; sentiment analysis |
| Agent loop ends unexpectedly | Orchestration safeguard: every session ends in resolution or human escalation | Letting it end silently |
| Handing off between agents or to a human | Structured handoff: context, findings, authorization state | Passing the raw transcript |

## 5. Claude Code quick rules

| Situation | Use |
|---|---|
| Requirements unknown (e.g. unfamiliar compliance rules) | **Interview pattern**: Claude asks clarifying questions first |
| Requirements clear, approach unclear, or low reversibility / needs review | **Plan mode** |
| Clearly scoped change | **Direct execution** |
| Plan mode found the fix | Switch to direct execution **in the same session**. Not a new session (loses context), not more planning |
| MCP server for the whole team | **Project scope**: `.mcp.json` in the repo; secrets as `${ENV_VAR}`; confirm tools appear with `/mcp` |
| MCP server just for you / experimental | **User scope** (or local scope for one project) |
| Slash command for the team vs. for you | `.claude/commands/` in the repo vs. `~/.claude/commands/` |
| Guidance that applies only to some files | `.claude/rules/` with glob patterns |
| Must never be violated | Hooks or settings permissions, not CLAUDE.md |
| Reusable project context vs. one-off | CLAUDE.md vs. `@` file reference vs. inline description |
| Exploring a large codebase | Glob → Grep → Read only targeted files; subagents and scratchpad files to protect context |
| Resuming a session | Re-analyze only changed files; inject prior findings as context |

## 6. API quick rules

- The API is stateless: send the full conversation history with every request.
- Agentic loop: continue while `stop_reason` is `tool_use`; stop and return on `end_turn`.
- Message Batches API: cheaper, results within 24 h. Use it only when nothing blocks on the result; use the synchronous Messages API when a user or workflow is waiting.
- Structured output reliability: tool use with JSON schema > prefill > prompt-based formatting.
- Sequence multi-tool workflows so prerequisite data is fetched before dependent tools are called.

## 7. Hands-on checklist (do these before the exam)

- [ ] Write a PreToolUse hook that blocks writes outside the project directory
- [ ] Add a PostToolUse hook that runs the formatter after every edit
- [ ] Build a one-tool MCP server and add it at project scope with an env-var secret
- [ ] Create a slash command in both the project and user directories
- [ ] Build a classify → extract pipeline with the Agent SDK, then add boundary validation
- [ ] Force a tool call by name and compare against `"any"`
- [ ] Design an extraction schema with nullable fields and an "unknown" enum
- [ ] Use plan mode on a real bug, then switch to direct execution for the fix

## 8. Pre-answer checklist (before reading the options)

1. What's the consequence of failure? Irreversible → enforce. Recoverable → teach or instruct.
2. Does the error cascade across steps? → Validate at the boundary.
3. Did the user or policy say something explicitly? → That wins.
4. Does my answer add a new component? → Check if something existing covers it.
5. Is this the root cause or another layer?
6. What's unknown? Requirements → interview. Approach → plan. Nothing → execute.
7. Who needs this config? Team → project scope. Just me → user scope.

## 9. Areas being tested (official objectives)

*From the official score report. Grouped by topic; the report doesn't label domains. Use it as a tick-list.*

### Agentic architecture & orchestration

- [ ] Design orchestration-layer safeguards ensuring every agent session ends with a completed resolution or human escalation, regardless of how the agentic loop terminates.
- [ ] Design structured handoff packages that preserve accumulated context, findings, and authorization state when transferring control between agent steps or to a human operator.
- [ ] Apply session resumption techniques—including targeted re-analysis of changed files and context injection—to restore agent state accurately without repeating prior work.
- [ ] Decompose complex tasks into dynamically generated subtasks that adapt as new information is discovered, rather than executing a fixed sequence regardless of intermediate findings.
- [ ] Apply PreToolUse and PostToolUse hook patterns to enforce business rules and policy constraints not delegated to model discretion.
- [ ] Explain how the agentic loop uses model responses and `stop_reason` signals to decide whether to continue tool execution or terminate and return a final response.

### Claude Code configuration & workflows

- [ ] Create and deploy custom slash commands in the correct project or user directory so they are available to intended users and invoked on demand for task-specific workflows.
- [ ] Structure iterative refinement workflows by providing concrete input-output examples, targeted feedback on specific failures, and batched issue descriptions for consolidated evaluation.
- [ ] Determine when to use plan mode versus direct execution based on task scope, reversibility, architectural uncertainty, and the need for stakeholder review before implementation.
- [ ] Distinguish instructions that must be enforced through settings permissions or hooks from those appropriately placed in CLAUDE.md, and restructure configurations accordingly.
- [ ] Select the correct Claude Code configuration mechanism—CLAUDE.md, `.claude/rules/` with glob patterns, Skills, hooks, or settings permissions—based on guidance type and when it should apply.
- [ ] Implement PostToolUse hooks that automatically enforce code quality constraints—such as formatting, linting, or test execution—after every file edit, independent of model instruction-following.
- [ ] Apply systematic codebase exploration strategies using Grep, Glob, and Read tools that build incremental understanding while managing context window constraints.
- [ ] Configure MCP servers and project settings at the correct scope—project-level for shared team tooling and user-level for personal or experimental configurations.
- [ ] Apply context management strategies—subagent isolation, scratchpad files, and targeted file reading—to sustain coherent codebase exploration across sessions exceeding context limits.
- [ ] Select the appropriate method for providing project context to Claude Code—`@` references, CLAUDE.md, or inline description—based on reusability, specificity, and cross-session need.

### Prompt engineering & structured output

- [ ] Select the appropriate API processing mode—synchronous Messages API or asynchronous Message Batches API—based on latency requirements, workflow blocking behavior, and acceptable processing windows.
- [ ] Select and implement the most reliable structured output method—tool use with JSON schema, prompt-based formatting, or prefilled responses—based on required schema compliance strictness.
- [ ] Design extraction schemas with optional fields, nullable values, and appropriate enum definitions that allow the model to accurately represent missing or ambiguous information without fabricating values.
- [ ] Implement tool use with defined JSON schemas to enforce structured output compliance, and configure `tool_choice` to guarantee tool invocation when conversational responses would cause downstream failures.
- [ ] Apply extraction accuracy patterns—structured schemas with optional fields, format normalization instructions, and few-shot examples—to reduce hallucination and improve consistency across varied document formats.
- [ ] Design feedback loop mechanisms that capture structured metadata about model errors and use those patterns to improve prompts, schemas, or few-shot examples in future iterations.

### Tool design & MCP

- [ ] Implement MCP tool error handling that surfaces structured, type-specific error information to the agent—including recoverability status and suggested next actions—rather than generic failure messages.
- [ ] Improve tool selection reliability by expanding tool descriptions with use-case examples, input format specifications, and explicit disambiguation guidance for semantically similar tools.
- [ ] Integrate MCP servers into Claude Code and agent applications by selecting the correct server scope, configuring authentication via environment variable expansion, and verifying tool discovery.
- [ ] Configure the `tool_choice` parameter to guarantee tool invocation when structured output is required, and sequence multi-tool workflows so prerequisite data is obtained before dependent tools are called.

### Context management & reliability

- [ ] Explain why conversation history must be explicitly included in each API request and identify the correct mechanism for maintaining state across multiple turns in a stateless API.
- [ ] Apply escalation decision criteria to determine when an agent should immediately honor a human escalation request versus attempt autonomous resolution using available tools.
- [ ] Design human review routing strategies that direct extractions to reviewers based on confidence scores, document characteristics, and field-level ambiguity rather than random sampling.
