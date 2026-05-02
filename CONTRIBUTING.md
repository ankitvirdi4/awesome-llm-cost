# Contributing

Thanks for considering a contribution. This list lives or dies by the quality of its entries, so the bar is real.

## Entry criteria

Every entry must meet all of the following:

1. **Actively maintained.** Last commit, release, or substantive update within the past 12 months. Dead projects move out, even if they were once great.
2. **Solves a clearly defined cost problem.** Tracking, routing, caching, quantization, batching, budgeting, pricing data, or another item under the Scope section in the README. If you cannot summarize the cost problem it solves in one sentence, it does not belong here.
3. **Has a working link.** GitHub repo, project page, or paper URL that resolves. Broken links are removed on sight.
4. **Accurate one line description.** Says what the project does, not what its marketing site says. Plain English. No buzzwords. No "world's first" or "next generation" or "revolutionary."
5. **Fits one of the listed categories.** If your entry does not fit, open an issue first to discuss whether the category list should be extended.

## Format

Entries follow this exact format:

```
- [Project Name](url) — One line description in present tense.
```

Examples of the tone we want:

```
- [Helicone](https://helicone.ai) — Open source proxy that logs OpenAI and Anthropic calls and rolls up cost per request.
- [LMCache](https://github.com/LMCache/LMCache) — Disaggregated KV cache for LLM serving that reduces cost on repeated prefixes.
```

Examples of what we reject:

```
- [TotallyAI](https://example.com) — The leading AI platform for enterprises  ← marketing copy
- [SomeProject](https://example.com) — does stuff with tokens                  ← vague
```

## PR process

1. **One entry per PR.** Easier to review, easier to merge, easier to revert if a project later goes bad.
2. **Open an issue first if you are unsure** the entry qualifies. A 2 line issue saves both of us the time of a closed PR.
3. **Place the entry alphabetically within its category** unless the category has its own ordering rule.
4. **Update the table of contents** if you are adding a new section header.

## Rejection is not personal

Entries get rejected for: dead projects, low quality docs, vendor marketing copy disguised as a tool, duplicate entries under a new name, or simply not fitting the scope. None of that is a comment on the contributor or the project. If your entry is rejected, the maintainer will say why, and a polished resubmission is welcome.

## Maintainer response time

I aim to respond to issues and PRs within 48 hours. If 7 days pass with no response, ping the issue or PR and I will follow up.
