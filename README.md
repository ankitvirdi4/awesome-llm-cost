# Awesome LLM Cost [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of tools, libraries, papers, and patterns for **LLM cost engineering**. Tracking, routing, caching, quantization, budgeting, and reducing the cost of running large language models in production.

> 🚧 **Early stage.** This list is being actively built. The first comprehensive version (~150 entries across 10 categories) is targeted for **3 May 2026**. Star the repo to follow along, or [open an issue](https://github.com/ankitvirdi4/awesome-llm-cost/issues) to suggest entries.

## Why this list

LLM cost is the single most discussed pain point for teams shipping AI in 2026, but the tools and patterns for managing it are scattered across observability platforms, routing libraries, caching layers, quantization research, provider specific docs, and pricing calculators. There is no single map.

This list aims to be that map.

## Scope

In scope:
- Cost tracking, observability, and attribution tools
- Routing libraries that optimize for cost
- Caching strategies and libraries (semantic, exact, prefix, prompt caching)
- Quantization, pruning, and distillation for cost reduction
- Batching APIs and async cost reduction patterns
- Pricing data sources and calculators
- Cost-aware serving research
- Provider specific cost optimization techniques
- Budget enforcement and circuit breaker patterns
- Cost benchmarks and leaderboards

Out of scope:
- General LLM observability (see [awesome-llm-observability](#), TBD)
- Pure inference speed work (see [awesome-mobile-llm](https://github.com/stevelaskaridis/awesome-mobile-llm) and similar)
- Prompt engineering for quality (this list is about cost, not quality)

## Contents

Categories will be filled in as entries are added. Planned structure:

- Cost Tracking, Observability & Budgets
- Routing & Model Selection
- Caching
- Quantization & Compression
- Batching & Async Patterns
- Pricing Data
- Calculators & Estimators
- Cost-Aware Serving (Research)
- Benchmarks & Leaderboards

## Contributing

PRs welcome once the v0.1 structure lands. In the meantime, the fastest way to help is to [open an issue](https://github.com/ankitvirdi4/awesome-llm-cost/issues) suggesting tools, papers, or patterns that should be included.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the criteria each entry must meet.

## Related lists

- [awesome-mobile-llm](https://github.com/stevelaskaridis/awesome-mobile-llm). On device and mobile LLMs.
- [awesome-on-device-AI-systems](https://github.com/jeho-lee/Awesome-On-Device-AI-Systems). On device AI systems research.

## License

[CC0](LICENSE), public domain. Take it, fork it, do whatever you want.

---

Maintained by [Ankit Virdi](https://github.com/ankitvirdi4). Also building [react-native-llm-meter](https://github.com/ankitvirdi4/react-native-llm-meter).
