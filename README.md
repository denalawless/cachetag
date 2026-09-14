# #CacheTag

## Founder's Note

I solved a problem I was seeing everywhere — poor AI recall affecting productivity, repeated queries, rising costs, and wasted resources. People kept saying "add more compute" while cutting jobs and while not finding actual efficiencies. I thought about the social platforms I've built and how they handle search and discovery, and applied that thinking to a new problem: reducing redundant inference. I've taken the most intuitive interface for human discovery — the hashtag — and applied it to a technical standard for AI memory efficiency.

#cachetag is a **local recall layer**: a persistent, tag-addressed store that sits in front of the model. It checks known queries against local memory before calling the LLM. On a hit, the model is not called at all — zero tokens, zero API cost, near-instant response. On a miss (new content, or content past its freshness window), it calls the model normally and stores the result for next time.

**(#cachetag™ v1)** — *Persistent local recall that eliminates redundant LLM calls and stops your AI from starting from scratch on repeat queries.*

**Lead Architect:** Dena Lawless
**Validation Contribution:** Abe Jarrett
**Project Origin:** November 17, 2025

---

## 🚀 The Value Proposition

Current LLM-based workflows suffer from a **redundant processing bottleneck**: every repeat query re-sends and re-processes context the system has already seen, even when the underlying content hasn't changed. In high-volume, high-repetition environments, this creates an unnecessary "re-read tax" — added cost and added latency for no new information gained.

#cachetag addresses this with a local, tag-addressed recall store: content is processed by the LLM once, cached locally with a freshness window, and served directly from local memory on every repeat query within that window — no model call required.

---

## 💡 Product Philosophy — The "Hashtag" Model

#cachetag lets a workflow avoid "re-reading" static content it has already processed. By tagging static reference material and separating it from dynamic, per-query questions, the system can recognize *"I've already answered this"* and skip the round-trip to the model entirely — falling back to a live call automatically when content is new or has aged past its time-to-live (TTL).

---

## 📊 Benchmark Results (measured, not estimated)

A controlled benchmark was run against Anthropic's Claude (`claude-sonnet-5`), comparing standard (always-call-the-model) behavior against #cachetag's recall-first behavior across a repeated query corpus. All figures below are computed directly from the API's own `response.usage` data — nothing is estimated.

- **93.3%** of repeat queries were served from local recall with **zero** model calls made
- **92.8%** total-token reduction, blended across first-time misses and repeat hits
- **92.0%** latency reduction, blended (near-instant on hits vs. a full model round-trip on a live call)
- Automatic **TTL-based expiry** (default 30 days): cached entries past their freshness window are treated as a miss, refreshed from a live model call, and re-cached

Full methodology and raw results available on request.

---

## 🛠️ Strategic Pillars

- **Semantic Addressing** — tagging static content blocks so repeat queries against them can be recognized and served locally
- **Time-Bound Freshness (TTL)** — cached answers expire on a configurable schedule and automatically refresh from a live model call, preventing stale answers from being served indefinitely
- **Static/Dynamic Decoupling** — separating permanent reference material from one-off, ephemeral questions to maximize reuse of what's already been processed
- **Local-First Recall** — the recall store lives outside the model provider's infrastructure, so persistence isn't tied to any one provider's session or cache window

---

## 🧭 Current Status

- Core recall mechanism benchmarked and verified against Claude (`claude-sonnet-5`)
- Cross-model testing (GPT, Gemini) not yet completed
- Tested at small-corpus scale; testing at high-line-count production scale (100k+ lines) is planned, not yet completed
- Production server-side deployment is the intended architecture; current implementation is a local, script-level proof of mechanism

---

## ⚖️ Intellectual Property Notice

This repository serves as a formal public declaration of Prior Art, dated November 17, 2025. The #cachetag protocol and its architectural logic are proprietary. Unauthorized use, reproduction, reverse engineering, or commercial implementation is strictly prohibited.

*Generated and saved under #cachetag*
