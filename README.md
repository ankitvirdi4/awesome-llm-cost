# Awesome LLM Cost [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Tools, libraries, and patterns for reducing LLM API costs. Open source observability platforms, cost aware routers, prompt caching, semantic caches, batching APIs, quantization, and pricing data for Anthropic, OpenAI, and Gemini.

LLM cost is the most discussed pain point for teams shipping AI in 2026, but the tools and patterns for managing it are scattered across observability platforms, routing libraries, caching layers, quantization research, provider docs, and pricing calculators. There is no single map.

This list aims to be that map.

In scope: anything that helps you spend less on LLMs without giving up quality you actually need. Out of scope: general LLM observability, pure inference speed work, prompt engineering for quality. Every entry was verified to be actively maintained and in real use as of May 2026.

## Contents

- [Cost Tracking, Observability, and Budgets](#cost-tracking-observability-and-budgets)
- [Routing and Model Selection](#routing-and-model-selection)
- [Caching](#caching)
- [Prompt Compression](#prompt-compression)
- [Quantization, Distillation, and Compression](#quantization-distillation-and-compression)
- [Self Hosting Starting Points](#self-hosting-starting-points)
- [Batching and Async Patterns](#batching-and-async-patterns)
- [Pricing Data](#pricing-data)
- [Calculators and Estimators](#calculators-and-estimators)
- [Cost Aware Serving Research](#cost-aware-serving-research)
- [Benchmarks and Leaderboards](#benchmarks-and-leaderboards)
- [Patterns](#patterns)
- [Provider Cost Optimization Guides](#provider-cost-optimization-guides)

## Cost Tracking, Observability, and Budgets

Tools that show you where your tokens go and let you set limits before the bill arrives.

### Open source

- [agenttrace](https://github.com/luoyuctl/agenttrace) - Local-first TUI and CLI that turns AI coding agent trace logs into cost, token, latency, and health regression reports.
- [Langfuse](https://github.com/langfuse/langfuse) - Full LLM tracing, prompt management, evaluation, datasets, with token and cost tracking exposed via a metrics API. Self hostable on PostgreSQL and ClickHouse.
- [Helicone](https://github.com/Helicone/helicone) - Proxy first observability with cost tracking, caching, and rate limits, no SDK install required. Self hostable.
- [OpenLLMetry](https://github.com/traceloop/openllmetry) - OpenTelemetry instrumentation for LLMs from Traceloop. Vendor neutral, exports to any OTel backend.
- [Phoenix](https://github.com/Arize-ai/phoenix) - OpenTelemetry native observability via OpenInference, notebook friendly with strong RAG and drift detection.
- [Opik](https://github.com/comet-ml/opik) - Tracing, evaluation, and prompt playground from Comet. Good fit if you also fine tune your own models.
- [Laminar](https://github.com/lmnr-ai/lmnr) - Agent debugging first observability with transcript view, outcome tracking, and rollout debugging.
- [Langtrace](https://github.com/Scale3-Labs/langtrace) - OpenTelemetry based LLM tracing with a clean OTel implementation.
- [OpenObserve](https://github.com/openobserve/openobserve) - Unified LLM and infrastructure observability with significantly lower storage costs than Datadog.
- [TruLens](https://github.com/truera/trulens) - Evaluation first observability, strong for RAG.
- [PostHog LLM Observability](https://github.com/PostHog/posthog) - LLM observability inside the broader PostHog product analytics platform.
- [SigNoz](https://github.com/SigNoz/signoz) - OpenTelemetry native APM with LLM observability features.

### Commercial

- [LangSmith](https://smith.langchain.com) - Closed source. Deepest integration with LangChain and LangGraph, with input, output, and other costs separated per trace.
- [Datadog LLM Observability](https://www.datadoghq.com/product/llm-observability/) - Closed source. Cost breakdowns by provider, model, prompt, or user inside Datadog APM.
- [Braintrust](https://www.braintrust.dev) - Closed source. Eval first platform that attaches estimated cost and token count to every span in a trace.
- [Weights and Biases Weave](https://wandb.ai/site/weave) - Closed source. LLM instrumentation that captures token usage and applies built in pricing, with custom cost tracking for fine tuned models.
- [Galileo](https://galileo.ai) - Closed source. Eval first with quality alerting and drift detection.

### Mobile and on device

- [react-native-llm-meter](https://github.com/ankitvirdi4/react-native-llm-meter) - Token usage, cost, and streaming TTFT for Claude, GPT, and Gemini calls from React Native and Expo apps. Disclosure: maintained by this list's author.

### Budgets and circuit breakers

Most budget enforcement lives inside the tools above.

- [LiteLLM virtual keys and budgets](https://docs.litellm.ai/docs/proxy/users) - Per user, per team, and per key budgets enforced in the LiteLLM proxy. The most flexible self hostable budget layer.
- [Finout](https://www.finout.io) - Closed source. Broader FinOps platform with LLM cost as one of many tracked services. Useful at organisation scale.

### Cost telemetry standards

- [OpenTelemetry GenAI semantic conventions](https://github.com/open-telemetry/semantic-conventions/tree/main/docs/gen-ai) - The upstream standard for naming generative AI spans, attributes, and metrics. Adopted by Phoenix, OpenLLMetry, and Langtrace.

## Routing and Model Selection

Sit between your app and providers. Pick the right model for each request based on cost, quality, latency, or rules.

### Gateways and proxies

- [LiteLLM](https://github.com/BerriAI/litellm) - The reference open source LLM proxy. 100+ providers behind one OpenAI compatible API, with budgets, virtual keys, fallbacks, and spend logging.
- [Portkey](https://github.com/Portkey-AI/gateway) - AI gateway with semantic caching, guardrails, programmable routing, and observability. Self hostable or managed.
- [OpenRouter](https://openrouter.ai) - Closed source. 300+ models behind one API key with cache aware routing and prompt caching.
- [Helicone AI Gateway](https://github.com/Helicone/ai-gateway) - Rust based gateway with low overhead, built in caching, and load balancing.
- [Cloudflare AI Gateway](https://developers.cloudflare.com/ai-gateway) - Closed source. Edge native gateway with caching, rate limiting, and analytics. Free tier.
- [Vercel AI Gateway](https://vercel.com/docs/ai-gateway) - Closed source. Default provider in the Vercel AI SDK.
- [TrueFoundry AI Gateway](https://www.truefoundry.com) - Closed source. Enterprise gateway with RBAC, virtual models, and deployment flexibility.
- [Kong AI Gateway](https://konghq.com/products/kong-ai-gateway) - Closed source. Kong's AI plugins for teams already running Kong as their API layer.
- [Bifrost](https://github.com/maximhq/bifrost) - Rust based router with microsecond scale overhead at high RPS, plus built in budget controls and intelligent routing.

### Smart and cost aware routers

- [RouteLLM](https://github.com/lm-sys/RouteLLM) - Reference framework from LMSYS for serving and evaluating preference data driven LLM routers.
- [NotDiamond](https://www.notdiamond.ai) - Closed source. Per query model routing with quality aware predictions.

## Caching

Three independent layers. Each cuts a different slice of cost. You want all three.

### Provider prompt caching

Cuts the cost of the cached portion of input tokens. Use for stable system prompts, RAG context, tool definitions.

- [Anthropic Prompt Caching](https://docs.claude.com/en/docs/build-with-claude/prompt-caching) - 90 percent off cached input via explicit cache_control markers. The most aggressive provider discount available.
- [OpenAI Prompt Caching](https://platform.openai.com/docs/guides/prompt-caching) - 50 percent off cached input, automatic for prompts over 1024 tokens, no code changes required.
- [Google Gemini Context Caching](https://ai.google.dev/gemini-api/docs/caching) - Implicit and explicit caching, charged for cache storage. Best for very long contexts.
- [AWS Bedrock Prompt Caching](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-caching.html) - Bedrock's wrapping of Anthropic style prompt caching, with cachePoint markers translated automatically by SDKs like LiteLLM.

### Semantic caching libraries

Eliminates the LLM call entirely on a hit. Hit rate ranges from 10 percent for open ended chat to 70 percent for structured FAQ.

- [GPTCache](https://github.com/zilliztech/GPTCache) - The reference open source semantic cache with multiple embedding and storage backends, integrated with LangChain and llama_index.
- [Redis LangCache](https://redis.io/langcache/) - Closed source. Redis's official semantic cache product, production grade.
- [Upstash Semantic Cache](https://upstash.com/docs/vector/sdks/ts/semantic-cache) - Closed source. Serverless semantic cache built on Upstash Vector.
- [Portkey semantic cache](https://portkey.ai/features/semantic-cache) - Feature within the Portkey gateway with up to 40 percent reported cost reduction.

### Inference infrastructure KV cache

For self hosted inference. Reuses computed key value attention states across requests.

- [vLLM](https://github.com/vllm-project/vllm) - PagedAttention, continuous batching, and automatic prefix caching. The default open source LLM serving framework.
- [SGLang](https://github.com/sgl-project/sglang) - Fast LLM serving with RadixAttention prefix caching, particularly strong for prompts with shared structure.
- [LMCache](https://github.com/LMCache/LMCache) - Extends vLLM with GPU to CPU RAM to disk KV cache tiering for significant latency reduction on cache hits.

## Prompt Compression

Reduce the prompt itself before sending. Cuts both input cost and latency, stacks with provider caching.

- [LLMLingua](https://github.com/microsoft/LLMLingua) - Microsoft Research. Token level prompt compression with up to 20x ratio while preserving instruction quality. Includes LLMLingua, LongLLMLingua, and LLMLingua-2.

## Quantization, Distillation, and Compression

Make models smaller and cheaper to run. Mostly relevant for self hosted inference, but knowing the trade offs matters even if you only call APIs.

### Quantization libraries

- [bitsandbytes](https://github.com/bitsandbytes-foundation/bitsandbytes) - Runtime 4 and 8 bit quantization from Tim Dettmers, with NF4, FP4, and LLM.int8. The only option that supports QLoRA training on quantized base models.
- [ExLlamaV2](https://github.com/turboderp-org/exllamav2) - EXL2 quantization, speed focused, optimized for low latency inference.
- [llama.cpp and GGUF](https://github.com/ggml-org/llama.cpp) - The reference for CPU and Apple Silicon quantization. K quants retain about 92 to 95 percent of FP16 quality at about 25 percent of memory.
- [MLC LLM](https://github.com/mlc-ai/mlc-llm) - Universal compiler for deploying LLMs across hardware including mobile, web, and edge. Cross platform quantization.
- [MLX](https://github.com/ml-explore/mlx) - Apple Silicon native ML framework with quantization tools, from Apple ML Research.
- [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM) - NVIDIA's optimized inference framework with INT8 and FP8 quantization, kernel fusion, and multi GPU.

### Distillation

- [Hugging Face TRL](https://github.com/huggingface/trl) - Transformer Reinforcement Learning library with student teacher pipelines for distilling large model behaviour into smaller fine tuned models.
- [DistillKit](https://github.com/arcee-ai/DistillKit) - Arcee AI's open source distillation toolkit for producing small high quality student models from larger teachers.

### Foundational papers

- [Speculative Decoding](https://arxiv.org/abs/2211.17192) - Leviathan et al. The technique that powers 2 to 3x throughput improvements in vLLM and SGLang for self hosted serving.
- [AWQ: Activation-aware Weight Quantization](https://arxiv.org/abs/2306.00978) - Lin et al. The AWQ paper.
- [GPTQ: Accurate Post-Training Quantization](https://arxiv.org/abs/2210.17323) - Frantar et al. The GPTQ paper.
- [SmoothQuant](https://arxiv.org/abs/2211.10438) - Xiao et al. Activation outlier handling.
- [BitNet b1.58](https://arxiv.org/abs/2402.17764) - Microsoft. 1.58 bit quantization research.

## Self Hosting Starting Points

Run a model locally to test whether self hosting is cheaper than API calls. Not strictly cost engineering, but the on ramp most teams need before committing to a serving stack.

- [Ollama](https://github.com/ollama/ollama) - The simplest way to run open source LLMs locally. Single binary, model library with one line pulls, OpenAI compatible API.
- [LM Studio](https://lmstudio.ai) - Closed source. Desktop app for running and chatting with local models, with a built in OpenAI compatible server.

## Batching and Async Patterns

50 percent off, asynchronous. The single biggest discount most teams aren't using.

- [OpenAI Batch API](https://platform.openai.com/docs/guides/batch) - 50 percent off, 24 hour completion window, up to 50,000 requests per batch and 200 MB per file. Stacks with prompt caching for compounded discounts.
- [Anthropic Message Batches API](https://docs.claude.com/en/docs/build-with-claude/batch-processing) - 50 percent off, 24 hour window, up to 100,000 requests per batch with up to 300K output tokens via beta header.
- [Google Gemini Batch Mode](https://ai.google.dev/gemini-api/docs/batch-mode) - 50 percent off, supports very large batches via JSONL upload on both Gemini API and Vertex AI.
- [AWS Bedrock Batch Inference](https://docs.aws.amazon.com/bedrock/latest/userguide/batch-inference.html) - Bedrock specific batch jobs across supported models.
- [LiteLLM batch passthrough](https://docs.litellm.ai/docs/batches) - Unified interface to OpenAI, Anthropic, and Vertex batch APIs.

## Pricing Data

The community maintained data sources that the rest of the ecosystem depends on.

- [model_prices_and_context_window.json](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.json) - The canonical community maintained pricing JSON, lives in LiteLLM. 100+ providers. Vendor or fetch from this if you're building anything cost aware.
- [OpenRouter Models API](https://openrouter.ai/api/v1/models) - JSON API with current pricing for 300+ models, cached at the edge.
- [llm-info](https://www.npmjs.com/package/llm-info) - Community maintained model and pricing metadata for the JS ecosystem.

## Calculators and Estimators

### Tokenizers

- [tiktoken](https://github.com/openai/tiktoken) - The official OpenAI BPE tokenizer in Python.
- [js-tiktoken](https://github.com/dqbd/tiktoken) - JS port of tiktoken with both pure JS and WebAssembly builds. WebAssembly is 3 to 6 times faster than pure JS for large texts.
- [gpt-tokenizer](https://github.com/niieani/gpt-tokenizer) - Pure JS tokenizer with the smallest bundle and fastest small text performance.
- [tiktoken-go](https://github.com/pkoukk/tiktoken-go) - Go port of tiktoken.
- [tiktoken-rs](https://github.com/zurawiki/tiktoken-rs) - Rust port of tiktoken.
- [SharpToken](https://github.com/dmitry-brazhenko/SharpToken) - C# port of tiktoken.
- [jtokkit](https://github.com/knuddelsgmbh/jtokkit) - Java port of tiktoken.
- [tokencost](https://github.com/AgentOps-AI/tokencost) - Python library that combines tokenization with up to date pricing for cost calculation.
- [Anthropic countTokens API](https://docs.claude.com/en/docs/build-with-claude/token-counting) - Official, free, network based. Currently the only ground truth source for Claude models from the official SDK.
- [Gemini countTokens](https://ai.google.dev/gemini-api/docs/tokens) - Official pre flight token counting via the Gemini SDK.

### Cost calculators and dashboards

- [OpenAI Tokenizer](https://platform.openai.com/tokenizer) - Official browser tokenizer for cl100k_base and o200k_base.
- [Claude Tokenizer](https://claude-tokenizer.vercel.app/) - Community built tokenizer that uses the official Anthropic API.
- [LiteLLM Pricing Calculator](https://docs.litellm.ai/docs/proxy/cost_tracking) - Cost estimate endpoint built into the LiteLLM proxy that forecasts spend from expected token usage and request volume.
- [PricePerToken](https://pricepertoken.com) - Comparison and leaderboards across 300+ models.
- [TokenCost App](https://tokencost.app) - Leaderboard with quality versus cost scoring.
- [CostGoat](https://costgoat.com) - Closed source. Desktop app for tracking OpenRouter and multi provider spend in real time.

## Cost Aware Serving Research

The papers worth knowing if you want to think rigorously about cost versus quality.

- [FrugalGPT](https://arxiv.org/abs/2305.05176) - Chen, Zaharia, Zou. The seminal paper. Proposes prompt adaptation, LLM approximation, and LLM cascade with up to 98 percent cost reduction at GPT-4 quality on benchmark tasks.
- [RouteLLM: Learning to Route LLMs with Preference Data](https://arxiv.org/abs/2406.18665) - Ong et al. ICLR 2025. Foundation for the open source RouteLLM framework.
- [Hybrid LLM: Cost-Efficient and Quality-Aware Query Routing](https://arxiv.org/abs/2404.14618) - Ding et al. ICLR 2024. Cost quality cascade routing.
- [AutoMix: Automatically Mixing Language Models](https://arxiv.org/abs/2310.12963) - Aggarwal et al. Smaller model self verifies before escalating to a larger model.
- [xRouter: Training Cost-Aware LLMs Orchestration via Reinforcement Learning](https://arxiv.org/abs/2510.08439) - RL trained cost aware orchestrator. Useful for thinking about routing as a learnable policy.
- [Don't Break the Cache](https://arxiv.org/abs/2510.19420) - Recent eval of prompt caching across OpenAI, Anthropic, and Google for agent workloads.
- [PowerInfer-2: Fast Large Language Model Inference on a Smartphone](https://arxiv.org/abs/2406.06282) - SJTU. On device cost reduction through smart serving on mobile hardware.

## Benchmarks and Leaderboards

Cost vs quality leaderboards. Quality only ones (LMSYS Arena etc.) are out of scope.

- [Artificial Analysis](https://artificialanalysis.ai) - The most cited LLM cost and quality benchmark with live data on quality, speed, and price across all major models.
- [LLM-Stats](https://llm-stats.com) - 296+ models with composite scores blending quality, price, and speed.
- [Vellum LLM Leaderboard](https://www.vellum.ai/llm-leaderboard) - Community trusted leaderboard across reasoning, coding, and math.
- [TokenCost Leaderboard](https://tokencost.app/leaderboard) - Sortable by quality, value (quality per dollar), and speed.
- [PricePerToken Leaderboards](https://pricepertoken.com/leaderboards) - Coding, reasoning, value, speed, and price filters.
- [CostGoat LLM API Comparison](https://costgoat.com/compare/llm-api) - 324+ APIs ranked by quality, price, and value.
- [OpenRouter Rankings](https://openrouter.ai/rankings) - Usage based rankings across the OpenRouter platform.

## Patterns

The techniques themselves, not the tools that implement them. Learn the pattern, then pick a tool above.

### Cascade routing

Send each request to the cheapest model first. Run a cheap quality check on the response. Escalate to a more expensive model only if the check fails. Documented in the FrugalGPT paper and operationalised by the RouteLLM framework, both linked in the research and routing sections above. The economic argument is straightforward: most production traffic is easy and a small model is enough.

### Distillation as cost optimization

Train a small model to mimic a large model on your specific traffic. Common practice: log 50,000 to 100,000 production calls from GPT-4 or Claude Opus, fine tune a 7B parameter open model on those traces, swap the small model in for the easy 80 percent of traffic. Cost reductions of 20 to 50x are realistic when the task distribution is narrow. Use TRL or DistillKit (linked above) to build the pipeline.

### Batch and cache stacking

OpenAI and Anthropic both apply batch and cache discounts independently. A request that hits a cached prefix and is submitted via the Batch API gets the cache discount on input tokens plus the 50 percent batch discount on the rest. For workloads that tolerate 24 hour latency, this stack reaches 75 percent off list price.

### Eval driven tier selection

Define an evaluation set that reflects your real traffic. Run it across every model tier you would consider (Haiku, Sonnet, Opus, GPT-4o-mini, GPT-4o, Gemini Flash, Gemini Pro). Pick the cheapest tier that hits your quality bar. Re run quarterly. Most teams over provision because they never tested the cheaper tier on their actual data.

### Prompt deduplication via hashing

Many production workloads send the same exact prompt repeatedly. A keyed hash before the LLM call catches identical requests and serves from a cheap KV store with sub millisecond latency. Far cheaper than semantic caching when the input distribution is naturally repetitive (search autocomplete, classification pipelines, structured extraction).

### Prompt compression

Apply LLMLingua or a similar token level compressor to long prompts before sending. Up to 20x compression in published benchmarks while keeping instruction following intact. Stacks with provider prompt caching: cache the compressed prefix.

### Speculative decoding

For self hosted serving, generate K tokens from a small draft model and verify them in parallel with the target model. 2 to 3x throughput on output tokens with no quality loss when the draft and target are well matched. Native in vLLM and SGLang. See the Leviathan et al. paper linked in the foundational papers section.

## Provider Cost Optimization Guides

Provider docs that go beyond the basics. URLs may drift, search the doc site if a link 404s.

- [Anthropic pricing reference](https://docs.claude.com/en/docs/about-claude/pricing) - Per model rates plus the breakdown of cache write and read multipliers, useful when modelling cache economics.
- [Anthropic extended thinking guide](https://docs.claude.com/en/docs/build-with-claude/extended-thinking) - Cost tradeoffs of the extended thinking feature, where reasoning tokens dominate output cost.
- [OpenAI reasoning model best practices](https://platform.openai.com/docs/guides/reasoning) - Reasoning effort tradeoffs for o-series models. Higher effort costs more output tokens, often catastrophically more.
- [OpenAI optimizing accuracy](https://platform.openai.com/docs/guides/optimizing-llm-accuracy) - Cost relevant guidance on instruction placement, response length controls, and structured output.
- [Google Gemini pricing](https://ai.google.dev/pricing) - Per million token rates that change above the 200K token threshold for some models. Worth knowing before you accidentally double your bill.
- [AWS Bedrock cross region inference](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html) - Cross region inference profiles for cost arbitrage and provisioned throughput tradeoffs.

## Related Lists

- [awesome-mobile-llm](https://github.com/stevelaskaridis/awesome-mobile-llm) - On device and mobile LLMs, research flavoured.
- [awesome-on-device-AI-systems](https://github.com/jeho-lee/Awesome-On-Device-AI-Systems) - On device AI inference systems.
- [Awesome-LLMOps](https://github.com/tensorchord/Awesome-LLMOps) - Broader LLMOps tooling.

## Contributing

PRs and issue suggestions welcome. To keep the list useful:

- The project should be actively maintained, with commits within the last 6 months or genuinely complete and stable.
- The entry should solve a clearly defined cost problem.
- Descriptions should be one line, present tense, and factual.
- One entry per PR for easy review.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full format.

Maintained by Ankit Virdi.
