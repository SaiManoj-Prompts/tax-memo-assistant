# Tax Memo Assistant — A Governed Claude Skill Case Study

A reusable Claude Skill that turns client-supplied tax facts into a structured, six-section first-draft tax research memo — built, guardrailed, and QA-tested end to end.

**[tax-memo-assistant.netlify.app](#)** 

---

## The Problem

Tax associates at professional services firms lose real time formatting first-draft memos. Handing that task to an unguarded AI creates a bigger risk than the time it saves: an AI can invent a legal citation, invent a fact the client never gave, or state a conclusion as if it were settled law — all while sounding completely credible. In a regulated professional context, that's a liability, not a shortcut.

## The Solution

A governed Claude Skill that enforces a fixed six-section output, refuses to use any fact or citation the user didn't supply, always hedges its conclusions, and always attaches a disclaimer — turning a risky shortcut into a safe drafting accelerator.

## Fixed Memo Structure

Every memo follows the same six sections, in the same order, regardless of how the request is phrased:

1. **Issue** — a precise, neutral statement of the tax question presented
2. **Facts** — only facts the user supplied, with every gap explicitly flagged
3. **Applicable Law** — only authorities the user named, never invented
4. **Analysis** — hedged, exploratory reasoning applying law to facts
5. **Conclusion** — a preliminary view framed as a starting point, never final
6. **Disclaimer** — mandatory on every output, with zero exceptions

## Seven Non-Negotiable Guardrails

- No invented citations
- No invented facts
- No definitive conclusions
- Mandatory disclaimer, every time
- No client-ready polish
- No jurisdiction or entity assumptions
- Escalation over guessing

## How It Was Built

The build followed the same process a real AI Center of Excellence team uses to ship a governed internal tool:

1. **Wrote the Architectural Standard** — defined the fixed six-section output format, tone rules, and quality bar before writing a single instruction for the AI itself.
2. **Defined the guardrails** — set the seven non-negotiable rules above.
3. **Built the Claude Skill** — translated the standard into a real `SKILL.md` file that Claude auto-applies, including trigger conditions and a pre-delivery quality checklist.
4. **Built a tiered prompt library** — wrote 15 test scenarios across three tiers (complete facts, missing facts, ambiguous or multi-issue authority), then ran 10 of them in this QA pass.
5. **Ran structured QA** — executed 10 real test runs, scoring each against a fixed rubric: hallucination risk, missing-fact flagging, disclaimer presence, and conclusion hedging.
6. **Diagnosed and confirmed fixes** — two runs failed the same way; rerunning them with manual invocation resolved both, proving the failure was in trigger detection, not the skill's core logic.

## QA Results

10 test runs across 3 difficulty tiers. Two runs failed for the same root cause — the skill didn't auto-activate for direct factual questions. Manually invoking it resolved both, confirming the failure was in trigger detection, not the skill's core logic.

| Test | Scenario Tier | Result | Hallucination Risk | Disclaimer Present |
|---|---|---|---|---|
| 1 | Simple, complete facts | Pass | 1/5 | Yes |
| 2 | Missing facts | Pass | 1/5 | Yes |
| 3 | Ambiguous authority | Pass | 1/5 | Yes |
| 4 | Simple, complete facts | Pass | 1/5 | Yes |
| 5 | Missing facts (auto-trigger) | Fail | 4/5 | No |
| 5b | Same, manually triggered | Pass | 1/5 | Yes |
| 6 | Missing facts | Pass | 2/5 | Yes |
| 7 | Multi-issue | Pass | 2/5 | Yes |
| 8 | Ambiguous authority (auto-trigger) | Fail | 4/5 | No |
| 8b | Same, manually triggered | Pass | 1/5 | Yes |

## Known Limitations

**Auto-trigger detection is unreliable for direct lookup questions.** The skill reliably activates for explicit requests ("write this up," "draft a memo") but not for phrasing like "does this qualify for X credit?" In both confirmed failures, when the skill didn't trigger, Claude either recalled unverified training knowledge or performed a live web search and cited named third-party sources — both violating the no-invented-citation rule. Manually invoking the skill resolved the issue in both cases.

**A gray zone around procedural mechanics.** The skill sometimes explains well-known procedural steps (like standard election forms) without a citation, treating them as general professional knowledge. This is a deliberate judgment call worth reviewing, not a clear violation.

## Why This Matters

This project replicates, at small scale, the actual process a Prompt/Skills Engineer follows on an internal AI governance team: define an architectural standard, build a reusable skill against it, stress-test it with a structured prompt library, and document failures with root-cause analysis rather than a simple pass rate.

## Files

- `tax-memo-assistant-case-study.html` — the full interactive case study (self-contained, no build step required)
- `tax-memo-assistant.skill` — the real SKILL.md packaged as a downloadable skill file (embedded in the case study page)

## Tech

Single static HTML file — no build tools, no dependencies beyond CDN-loaded fonts. Open directly in a browser, or deploy via GitHub Pages.
