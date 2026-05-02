# Awesome LLM Cost [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of tools, libraries, papers, and patterns for **LLM cost engineering**. Tracking, routing, caching, quantization, budgeting, and reducing the cost of running large language models in production.

LLM cost is the most discussed pain point for teams shipping AI in 2026, but the tools and patterns for managing it are scattered across observability platforms, routing libraries, caching layers, quantization research, provider docs, and pricing calculators. There is no single map.

This list aims to be that map.

## Contents

- [Cost Tracking, Observability & Budgets](#cost-tracking-observability--budgets)
- [Routing & Model Selection](#routing--model-selection)
- [Caching](#caching)
- [Quantization & Compression](#quantization--compression)
- [Batching & Async Patterns](#batching--async-patterns)
- [Pricing Data](#pricing-data)
- [Calculators & Estimators](#calculators--estimators)
- [Cost-Aware Serving (Research)](#cost-aware-serving-research)
- [Benchmarks & Leaderboards](#benchmarks--leaderboards)
- [Contributing](#contributing)

## Scope

**In scope:** anything that helps you spend less on LLMs without giving up quality you actually need.

**Out of scope:** general LLM observability (use awesome-llm-observability, TBD), pure inference speed work (use [awesome-mobile-llm](https://github.com/stevelaskaridis/awesome-mobile-llm) and similar), prompt engineering for quality (this list is about cost, not quality).

Every entry was verified to be actively maintained and in real use as of May 2026.

---

## Cost Tracking, Observability & Budgets

Tools that show you where your tokens go and let you set limits before the bill arrives.

### Open source

- [Langfuse](https://github.com/langfuse/langfuse) — MIT. Full LLM tracing, prompt management, evaluation, datasets. The most adopted open source observability platform. Self hostable on Postgres + ClickHouse.
- [Helicone](https://github.com/Helicone/helicone) — Apache 2.0. Proxy first: change your base URL, get cost tracking, caching, and rate limits with no SDK install. Self hostable.
- [OpenLLMetry (Traceloop)](https://github.com/traceloop/openllmetry) — Apache 2.0. OpenTelemetry instrumentation for LLMs. Vendor neutral, exports to any OTel backend (Datadog, New Relic, SigNoz, Honeycomb).
- [Phoenix](https://github.com/Arize-ai/phoenix) — Elastic 2.0. OpenTelemetry native via OpenInference. Notebook friendly, strong for RAG observability and drift detection.
- [Opik](https://github.com/comet-ml/opik) — Apache 2.0. Tracing, evaluation, and prompt playground from Comet. Good fit if you also fine tune your own models.
- [Laminar](https://github.com/lmnr-ai/lmnr) — Apache 2.0. Agent debugging first observability. Transcript view, Signals for outcome tracking, agent rollout debugging. OTel compatible ingest.
- [Langtrace](https://github.com/Scale3-Labs/langtrace) — AGPL 3.0. OpenTelemetry based LLM tracing. Smaller adoption but a clean OTel implementation.
- [OpenObserve](https://github.com/openobserve/openobserve) — AGPL. Unified LLM and infrastructure observability in one platform. Significantly lower storage costs than Datadog. Pair with Langfuse for eval depth.
- [TruLens](https://github.com/truera/trulens) — MIT. Originally evaluation first, with observability bolted on. Strong for RAG.
- [PostHog LLM Observability](https://github.com/PostHog/posthog) — MIT. LLM observability inside the broader PostHog product analytics platform. Useful if you already use it.
- [SigNoz](https://github.com/SigNoz/signoz) — MIT. OpenTelemetry native APM with LLM observability features. Best if you already standardise on SigNoz.

### Commercial

- [LangSmith](https://smith.langchain.com) — Deepest integration with LangChain and LangGraph. Annotation queues, eval pipelines, prompt versioning. Free tier, then per seat. (closed source)
- [Datadog LLM Observability](https://www.datadoghq.com/product/llm-observability/) — Adds LLM monitoring to Datadog APM. Path of least resistance if you already use Datadog. (closed source)
- [Braintrust](https://www.braintrust.dev) — Eval first platform with tracing. Strong regression harness. (closed source)
- [Weights & Biases Weave](https://wandb.ai/site/weave) — Fits if your ML team already lives in W&B. (closed source)
- [Galileo](https://galileo.ai) — Eval first with quality alerting and drift detection. (closed source)

### Mobile / on device

- [react-native-llm-meter](https://github.com/ankitvirdi4/react-native-llm-meter) — MIT. Token usage, cost, and streaming TTFT for Claude / GPT / Gemini calls from React Native and Expo apps. On device storage, dev overlay, optional remote sink. Disclosure: maintained by this list's author.

### Budgets & circuit breakers

Most budget enforcement lives inside the tools above. The patterns worth knowing:

- [LiteLLM virtual keys + budgets](https://docs.litellm.ai/docs/proxy/users) — per user, per team, per key budgets enforced in the LiteLLM proxy. The most flexible self hostable budget layer.
- Helicone, Portkey, Langfuse, OpenRouter all expose budgets as a feature. See each tool's docs.
- [Finout](https://www.finout.io) — Broader FinOps platform with LLM cost as one of many tracked services. Useful at organisation scale. (closed source)

---

## Routing & Model Selection

Sit between your app and providers. Pick the right model for each request based on cost, quality, latency, or rules.

### Gateways and proxies

- [LiteLLM](https://github.com/BerriAI/litellm) — MIT. The reference open source LLM proxy. 100+ providers behind one OpenAI compatible API. Self hostable, with budgets, virtual keys, fallbacks, and spend logging. Also maintains the canonical [pricing JSON](#pricing-data) the rest of the ecosystem depends on.
- [Portkey](https://github.com/Portkey-AI/gateway) — MIT. AI gateway with semantic caching, guardrails, programmable routing, and observability. Self hostable or managed.
- [OpenRouter](https://openrouter.ai) — 300+ models behind one API key, marketplace pricing with a small markup. Cache aware routing and prompt caching. The simplest way to try many models fast. (closed source)
- [Helicone AI Gateway](https://github.com/Helicone/ai-gateway) — GPL 3.0. Rust based. Low overhead, built in caching and load balancing, drop in OpenAI compatibility.
- [Cloudflare AI Gateway](https://developers.cloudflare.com/ai-gateway) — Edge native, natural fit if your stack is on Cloudflare. Caching, rate limiting, analytics. Free tier. (closed source)
- [Vercel AI Gateway](https://vercel.com/docs/ai-gateway) — Default provider in the Vercel AI SDK, model strings work out of the box. (closed source)
- [TrueFoundry AI Gateway](https://www.truefoundry.com) — Enterprise gateway with RBAC, virtual models, deployment flexibility. (closed source)
- [Kong AI Gateway](https://konghq.com/products/kong-ai-gateway) — Kong's AI plugins. For teams already running Kong as their API layer. (closed source)
- [Bifrost](https://github.com/maximhq/bifrost) — Apache 2.0. Rust based router with microsecond scale overhead at high RPS. For latency critical production.

### Smart / cost-aware routers

- [RouteLLM](https://github.com/lm-sys/RouteLLM) — Apache 2.0, from LMSYS. Reference framework for serving and evaluating preference data driven LLM routers. ICLR 2025. Research project, less actively maintained but widely cited.
- [NotDiamond](https://www.notdiamond.ai) — Per query model routing with quality aware predictions. (closed source)

---

## Caching

Three independent layers. Each cuts a different slice of cost. You want all three.

### Provider prompt caching (KV cache reuse at the provider)

Cuts the cost of the cached portion of input tokens. Use for stable system prompts, RAG context, tool definitions.

- [Anthropic Prompt Caching](https://docs.claude.com/en/docs/build-with-claude/prompt-caching) — 90% off cached input. Explicit `cache_control` markers. 5 minute default TTL, 1 hour extended (at higher cost). The most aggressive provider discount available.
- [OpenAI Prompt Caching](https://platform.openai.com/docs/guides/prompt-caching) — 50% off cached input. Automatic for prompts ≥1024 tokens, no code changes required.
- [Google Gemini Context Caching](https://ai.google.dev/gemini-api/docs/caching) — implicit and explicit caching. Charged for cache *storage*, best for very long contexts that justify the storage cost.
- [AWS Bedrock Prompt Caching](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-caching.html) — Bedrock's wrapping of Anthropic style prompt caching, with `cachePoint` markers translated automatically by SDKs like LiteLLM.

### Semantic caching libraries (similarity match in your infra)

Eliminates the LLM call entirely on a hit. Hit rate ranges from 10% (open ended chat) to 70% (structured FAQ). Threshold tuning is the hardest part.

- [GPTCache](https://github.com/zilliztech/GPTCache) — MIT. The reference open source semantic cache. Multiple embedding backends, multiple storage backends, integrated with LangChain and llama_index.
- [Redis LangCache](https://redis.io/langcache/) — Redis's official semantic cache product. Production grade. (closed source)
- [Upstash Semantic Cache](https://upstash.com/docs/vector/sdks/ts/semantic-cache) — serverless semantic cache built on Upstash Vector. (closed source)
- [Portkey semantic cache](https://portkey.ai/features/semantic-cache) — feature within the Portkey gateway. Up to ~40% cost reduction in their reported workloads.

### Inference infrastructure (KV cache at serving time)

For self hosted inference. Reuses computed key value attention states across requests.

- [vLLM](https://github.com/vllm-project/vllm) — Apache 2.0. PagedAttention + continuous batching + automatic prefix caching. The default open source LLM serving framework.
- [SGLang](https://github.com/sgl-project/sglang) — Apache 2.0. Fast LLM serving with RadixAttention prefix caching. Particularly strong for prompts with shared structure.
- [LMCache](https://github.com/LMCache/LMCache) — Apache 2.0. Extends vLLM with GPU → CPU RAM → disk KV cache tiering. Significant latency reduction for cache hits.

---

## Quantization & Compression

Make models smaller and cheaper to run. Mostly relevant for self hosted inference, but knowing the trade offs matters even if you only call APIs.

- [bitsandbytes](https://github.com/bitsandbytes-foundation/bitsandbytes) — MIT. Runtime 4/8 bit quantization from Tim Dettmers. NF4, FP4, LLM.int8. The only option that supports QLoRA training on quantized base models.
- [ExLlamaV2](https://github.com/turboderp-org/exllamav2) — MIT. EXL2 quantization. Speed focused, supports K-quants, optimized for low latency inference.
- [llama.cpp / GGUF](https://github.com/ggml-org/llama.cpp) — MIT. The reference for CPU and Apple Silicon quantization. K-quants (Q4_K_M etc) retain ~92-95% of FP16 quality at ~25% of memory.
- [MLC LLM](https://github.com/mlc-ai/mlc-llm) — Apache 2.0. Universal compiler for deploying LLMs across hardware (mobile, web, edge). Cross platform quantization.
- [MLX](https://github.com/ml-explore/mlx) — MIT. Apple Silicon native ML framework with quantization tools. From Apple ML Research.
- [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM) — NVIDIA's optimized inference framework. INT8 / FP8 quantization, kernel fusion, multi GPU.

### Foundational papers

- [AWQ: Activation-aware Weight Quantization](https://arxiv.org/abs/2306.00978) — Lin et al. The AWQ paper.
- [GPTQ: Accurate Post-Training Quantization](https://arxiv.org/abs/2210.17323) — Frantar et al. The GPTQ paper.
- [SmoothQuant](https://arxiv.org/abs/2211.10438) — Xiao et al. Activation outlier handling.
- [BitNet b1.58](https://arxiv.org/abs/2402.17764) — Microsoft. 1.58 bit quantization research.

---

## Batching & Async Patterns

50% off, asynchronous. The single biggest discount most teams aren't using.

- [OpenAI Batch API](https://platform.openai.com/docs/guides/batch) — 50% off, 24 hour completion window, up to 50,000 requests per batch and 200 MB per file. Stacks with prompt caching for compounded discounts.
- [Anthropic Message Batches API](https://docs.claude.com/en/docs/build-with-claude/batch-processing) — 50% off, 24 hour window, up to 100,000 requests per batch. Supports up to 300K output tokens with the appropriate beta header.
- [Google Gemini Batch Mode](https://ai.google.dev/gemini-api/docs/batch-mode) — 50% off. Supports very large batches via JSONL upload. Available on both Gemini API and Vertex AI.
- [AWS Bedrock Batch Inference](https://docs.aws.amazon.com/bedrock/latest/userguide/batch-inference.html) — Bedrock specific batch jobs across supported models.
- [LiteLLM batch passthrough](https://docs.litellm.ai/docs/batches) — unified interface to OpenAI, Anthropic, and Vertex batch APIs.

---

## Pricing Data

The community maintained data sources that the rest of the ecosystem depends on.

- [model_prices_and_context_window.json](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.json) — the canonical community maintained pricing JSON, lives in LiteLLM. 100+ providers. Vendor or fetch from this if you're building anything cost aware.
- [OpenRouter Models API](https://openrouter.ai/api/v1/models) — JSON API with current pricing for 300+ models. Cached at the edge, designed for production integration.
- [llm-info](https://www.npmjs.com/package/llm-info) — community maintained model and pricing metadata for the JS ecosystem.

---

## Calculators & Estimators

### Tokenizers (count tokens before you send)

- [tiktoken](https://github.com/openai/tiktoken) — Python. The official OpenAI BPE tokenizer.
- [js-tiktoken / @dqbd/tiktoken](https://github.com/dqbd/tiktoken) — JS port of tiktoken with both pure JS and WASM builds. 3M+ weekly downloads. WASM is 3-6x faster than pure JS for large texts.
- [gpt-tokenizer](https://github.com/niieani/gpt-tokenizer) — pure JS tokenizer with the smallest bundle and fastest small text performance. Includes `encodeChat()` with proper special token handling.
- [tiktoken-go](https://github.com/pkoukk/tiktoken-go), [tiktoken-rs](https://github.com/zurawiki/tiktoken-rs), [SharpToken](https://github.com/dmitry-brazhenko/SharpToken), [jtokkit](https://github.com/knuddelsgmbh/jtokkit) — language ports of tiktoken (Go, Rust, C#, Java).
- [tokencost](https://github.com/AgentOps-AI/tokencost) — Python library that combines tokenization with up to date pricing for cost calculation.
- [Anthropic countTokens API](https://docs.claude.com/en/docs/build-with-claude/token-counting) — official, free, network based. Currently the only ground truth source for Claude models from the official SDK.
- [Gemini countTokens](https://ai.google.dev/gemini-api/docs/tokens) — official pre flight token counting via the Gemini SDK.

### Cost calculators and dashboards

- [OpenAI Tokenizer](https://platform.openai.com/tokenizer) — official browser tokenizer for cl100k_base / o200k_base.
- [Claude Tokenizer (web)](https://claude-tokenizer.vercel.app/) — community built, uses the official Anthropic API.
- [LiteLLM Pricing Calculator](https://docs.litellm.ai/docs/proxy/cost_tracking) — `/cost/estimate` endpoint built into the LiteLLM proxy. Forecasts spend from expected token usage and request volume.
- [PricePerToken](https://pricepertoken.com) — comparison and leaderboards across 300+ models.
- [TokenCost App](https://tokencost.app) — leaderboard with quality vs cost scoring.
- [CostGoat](https://costgoat.com) — desktop app for tracking OpenRouter and multi provider spend in real time.

---

## Cost-Aware Serving (Research)

The papers worth knowing if you want to think rigorously about cost vs quality.

- [FrugalGPT: How to Use Large Language Models While Reducing Cost and Improving Performance](https://arxiv.org/abs/2305.05176) — Chen, Zaharia, Zou (2023). The seminal paper. Proposes prompt adaptation, LLM approximation, and LLM cascade. Demonstrates up to 98% cost reduction at GPT-4 quality on benchmark tasks.
- [RouteLLM: Learning to Route LLMs with Preference Data](https://arxiv.org/abs/2406.18665) — Ong et al. (ICLR 2025). Preference data driven router. Foundation for the open source RouteLLM framework.
- [Hybrid LLM: Cost-Efficient and Quality-Aware Query Routing](https://arxiv.org/abs/2404.14618) — Ding et al. (ICLR 2024). Cost quality cascade routing.
- [AutoMix: Automatically Mixing Language Models](https://arxiv.org/abs/2310.12963) — Aggarwal et al. Smaller model self verifies before escalating to a larger model.
- [xRouter: Training Cost-Aware LLMs Orchestration via Reinforcement Learning](https://arxiv.org/abs/2510.08439) — RL trained cost aware orchestrator. Useful for thinking about routing as a learnable policy.
- [Don't Break the Cache: Prompt Caching for Long-Horizon Agentic Tasks](https://arxiv.org/abs/2510.19420) — recent eval of prompt caching across OpenAI, Anthropic, and Google for agent workloads.
- [PowerInfer-2: Fast Large Language Model Inference on a Smartphone](https://arxiv.org/abs/2406.06282) — SJTU. On device cost reduction through smart serving on mobile hardware.

---

## Benchmarks & Leaderboards

Cost vs quality leaderboards. Quality only ones (LMSYS Arena etc.) are out of scope.

- [Artificial Analysis](https://artificialanalysis.ai) — the most cited LLM cost and quality benchmark. Live data on quality, speed, and price across all major models. Most other leaderboards source from here.
- [LLM-Stats](https://llm-stats.com) — 296+ models with composite scores blending quality, price, and speed. Live API sampled performance metrics.
- [Vellum LLM Leaderboard](https://www.vellum.ai/llm-leaderboard) — community trusted leaderboard across reasoning, coding, math.
- [TokenCost Leaderboard](https://tokencost.app/leaderboard) — sortable by quality, value (quality per dollar), and speed.
- [PricePerToken Leaderboards](https://pricepertoken.com/leaderboards) — coding, reasoning, value, speed, price filters.
- [CostGoat LLM API Comparison](https://costgoat.com/compare/llm-api) — 324+ APIs ranked by quality, price, and value.
- [OpenRouter Rankings](https://openrouter.ai/rankings) — usage based rankings across the OpenRouter platform.

---

## Contributing

PRs and issue suggestions welcome. To keep the list useful:

- The project should be **actively maintained** (commits within the last 6 months, or genuinely complete and stable).
- The entry should solve a **clearly defined cost problem**: tracking, routing, caching, quantization, batching, pricing data, or budget enforcement.
- Descriptions should be **one line, present tense, factual**. Not marketing copy.
- One entry per PR for easy review.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full format.

---

## Related lists

- [awesome-mobile-llm](https://github.com/stevelaskaridis/awesome-mobile-llm). On device and mobile LLMs, research flavoured.
- [awesome-on-device-AI-systems](https://github.com/jeho-lee/Awesome-On-Device-AI-Systems). On device AI inference systems.
- [Awesome-LLMOps](https://github.com/tensorchord/Awesome-LLMOps). Broader LLMOps tooling.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [Ankit Virdi](https://github.com/ankitvirdi4) has waived all copyright and related rights to this work.

---

Maintained by [Ankit Virdi](https://github.com/ankitvirdi4). Also building [react-native-llm-meter](https://github.com/ankitvirdi4/react-native-llm-meter), LLM observability for React Native and Expo apps.
