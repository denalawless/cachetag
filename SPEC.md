# SPEC.md: The #cachetag™ Protocol (v1.0.0)

**A Standard for Content-Addressable, Tag-Based LLM Context Recall**

**Author:** Dena Lawless
**Status:** Concept & Reference Implementation — core recall mechanism benchmarked; full protocol not yet production-deployed
**Reference:** #cachetag

---

## 1. Abstract

The #cachetag protocol defines a syntax for tagging static, reusable blocks of content within an LLM workflow. Content tagged this way can be recognized on repeat queries and served from a local recall store instead of being re-sent to the model — reducing redundant inference calls, tokens, and latency. In benchmark testing, this approach eliminated 93.3% of repeat-query model calls (see Section 3).

---

## 2. Syntax Definition

A #cachetag block is defined by a semantic identifier followed by content enclosed in brackets.

- **Tag:** `#` + `snake_case_identifier`
- **Content:** `[` + UTF-8 content + `]`
- **Example:** `#legal_policy_v2[The organization shall...]`

### 2.1 Namespace Scoping (proposed, not yet implemented)

To support multi-tenant environments, #cachetag is designed to support optional namespacing to prevent identifier collisions between unrelated tagged content.

- **Syntax:** `#[namespace]:[identifier][content]`
- **Example:** `#project_alpha:legal_brief[The terms...]`
- **Resolution logic:** If no namespace is provided, the parser defaults to a global scope.

*Status: this is a designed extension to the syntax. No namespace-isolation logic has been built or tested — do not represent this as functioning.*

---

## 3. Empirical Validation

Unlike earlier drafts of this spec, the figures below come directly from a real, logged benchmark — not an estimate or a described-but-unrun test.

**Test conditions:**
- **Model:** `claude-sonnet-5`
- **Corpus:** 5 distinct tagged content blocks, replayed 15 times each (150 total logged events)
- **TTL:** 30 days (cache entries auto-expire and refresh via a live model call once past this window)
- **Token counts:** taken directly from the API's `response.usage` field on every call — not estimated

**Results:**

| Metric | Result |
|---|---|
| Repeat-query model calls avoided | 93.3% |
| Total-token reduction (blended, hits + misses) | 92.8% |
| Latency reduction (blended) | 92.0% |
| Cache hit latency | ~0ms (local lookup) vs. ~1.2s per live model call |

Full raw results (`results_raw.jsonl`) and run manifest available on request.

**Known limitations, stated plainly:**
- Small corpus (5 tags), single model, single test session — not yet validated at high-volume production scale
- No TTL expiry event occurred within this test window (99 seconds; TTL is 30 days) — expiry/refresh logic is unit-tested in isolation but not yet observed in a live, time-elapsed run
- Not yet tested against GPT or Gemini

---

## 4. Addressing & Retrieval

The current implementation checks incoming queries against tagged entries in a local recall store before deciding whether to call the model:

- **On a hit** (tag exists, within TTL): the stored answer is returned directly. The model is not called.
- **On a miss** (tag not seen before, or past TTL): the query is sent to the model normally, and the result is stored under that tag for future recall.

*Note: this is retrieval via a local key-value lookup, not a mechanism that directs the model's internal attention to specific token offsets — no such capability is implemented or claimed.*

---

## 5. Compliance Levels (roadmap, not current state)

These describe the intended maturity path for a production implementation. **None beyond L1 currently exist.**

- **L1 (Basic):** Plain-text tag modularization via a local script — *this is the level that has been built and benchmarked.*
- **L2 (Optimized):** A faster, production-grade parser — *not yet built.*
- **L3 (Protocol):** Native integration as a published, interoperable protocol (e.g., via MCP) — *aspirational; requires a published interface spec that does not yet exist.*

---

## 6. Intellectual Property Notice

This document, together with the accompanying repository, serves as a formal public declaration of Prior Art, dated November 17, 2025. The #cachetag protocol and its architectural logic are proprietary. Unauthorized use, reproduction, reverse engineering, or commercial implementation is strictly prohibited.

*Saved under #cachetag*
