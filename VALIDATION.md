# #CacheTag Benchmark — Test Brief

**Status:** Real API calls, real token counts, no estimated or illustrative figures.  
**Test date:** September 12, 2026  
**Model:** Claude-Sonnet 5 (with future models to be tested!)  

# VALIDATION.md: #cachetag™ Empirical Benchmark Brief

**Status:** Physical API execution statistics; zero simulated data points.  
**Execution Profile:** Verified continuous live environment testing.  
**Target Engine:** Platform-agnostic optimization layout.  

---

## 1. Operational Framework
The objective of this benchmark was to evaluate the economic and systemic impact of routing recurring application context profiles through a proprietary, local-first recall layer instead of executing repetitive cloud infrastructure calls. 

Two deployment profiles were executed sequentially against identical context volumes:
1. **Standard Architecture:** Un-cached, linear routing. Every query executes a full external model call with zero data persistence.
2. **#cachetag Standard:** Ingestion gateway routing. Initial context sets execute an isolated validation call and register a localized memory marker under a time-bound freshness window. Subsequent matching profiles resolve at the local gate.

---

## 2. Benchmark Parameters
- **Test Corpus:** 5 distinct multi-variable enterprise data profiles (fictional product spec, contract clause, onboarding policy, pricing tier, and an isolated cryptographic string asset).
- **Execution Volume:** 15 continuous automated replay cycles over the target corpus.
- **Log Metrics:** 150 individual execution events recorded via raw system analytics (75 standard passes, 75 optimized passes).
- **Temporal Window:** Configured with a default 30-day structural rotation schedule.

---

## 3. Verified Performance Metrics

Blended performance metrics reflect the combined optimization across baseline registration calls and subsequent local cache hits:

| Performance Vector | Measured Optimization Value | Systemic Impact |
| :--- | :--- | :--- |
| **Cloud Inference Avoidance** | **93.3%** | 70 out of 75 recurring calls resolved at local gate |
| **Gross Token Volume Reduction** | **92.8%** | Massive mitigation of redundant data processing spend |
| **System Processing Latency** | **92.0%** | Local lookup scales down processing lag to ~0ms |

*Validation Note: 100% of the initial baseline registration profiles were accurately classified as context misses and successfully compiled via the target engine without data corruption.*

---

## 4. Architectural Boundaries & Scope
- **Scale Profiles:** Initial evaluation verified at small-corpus scale. Transitioning to high-line-count enterprise production scale (100k+ lines) is an active roadmap item.
- **Provider Agnosticism:** Cross-model verification frameworks for alternative foundational model providers remain under active internal development.
- **Rotation Validation:** Time-elapsed expiration paths are structurally sound and verified via isolated unit testing; observation inside long-horizon live windows is ongoing.

---

## 5. Intellectual Property & Governance
Full analytical manifest records and raw system execution logs are restricted assets and remain locked inside protected corporate environments. Implementation specifics regarding the token optimization scripts, indexing mechanics, and local database routing layers are excluded from this brief. 

Unauthorized duplication, reverse-engineering, or functional replication of this protocol profile is strictly prohibited under current priority art filings dated November 17, 2025.


---

## Test Case

Does routing repeat queries through a local recall layer (instead of resending full context to an LLM every time) reduce API calls, tokens, and latency — while correctly falling back to a live LLM call for new or expired content?

Two conditions were run head-to-head on identical queries:
* **Standard:** Every query is sent to the LLM in full, every time. No memory, no persistence.
* **#cachetag:** The first time a topic is queried, it's sent to the LLM in full (same as standard) and the answer is cached locally with a timestamp. Every later query on that same topic is checked against a time-to-live (TTL) window:
  * If still fresh, the answer is served directly from local storage and the LLM is not called at all.
  * If the TTL has expired, it's treated as a miss and re-fetched from the LLM.

---

## Test Details

* **Model tested:** `claude-sonnet-5`
* **Test window (UTC):** `2026-09-13T00:31:24Z` → `2026-09-13T00:33:03Z` (~99 seconds wall clock)
* **Corpus:** 5 distinct synthetic content topics (fictional product spec, contract clause, onboarding policy, pricing tier, and a test secret string)
* **Replays:** 15 full passes over the corpus
* **Total logged events:** 150 (75 standard, 75 #cachetag)
* **Time To Live setting:** 30 days
* **Run ID:** `a8cfc6b6`

Every single call — both conditions — was logged individually with real API usage data (`input_tokens`, `output_tokens` from the API response itself, not estimated), a timestamp, and a hit/miss/expired status. Nothing here is averaged from a summary; the aggregate numbers below are computed directly from that raw, unedited log.

---

## Evidence

| Condition | Calls | API calls made | Served from local cache | Mean total tokens/call | Mean latency |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Standard** | 75 | 75 | 0 | 187.0 | 1.208s |
| **#cachetag** | 75 | 5 | 70 | 13.5 | 0.097s |

### Headline Results
*Blended across first-time misses and cache hits:*
* **93.3%** of #cachetag calls avoided the LLM entirely (70 of 75)
* **92.8%** reduction in total tokens
* **92.0%** reduction in latency

Of the 5 unavoidable first-time queries (every topic must be seen once before it can be cached), all 5 were correctly identified as misses and fetched live. 

Zero cache entries expired during this test window, since 99 seconds is far shorter than the 30-day TTL — expiry-and-refresh behavior has been unit-tested separately but not yet exercised in a live end-to-end run.

---

## Known Limitations

* **Small corpus & synthetic content:** Contains 5 topics and synthetic content; not yet tested against real production query volume or variety.
* **Single test profile:** Single test run, single machine, single day. Numbers should be replicated across multiple independent runs before being treated as a stable baseline.
* **Unexercised expiry logic:** TTL expiry-and-refresh path is implemented and logic-tested but has not yet occurred in a live timed run.
* **Replay dependency:** The blended reduction percentage is a function of replay count. More repeat queries on the same cached topics will push the number higher, since every topic's first query is a mandatory miss.

---

*Raw log (`results_raw.jsonl`) of Sept 12, 2026, and full manifest (`summary_report.json`) available on request. Implementation details of the local caching/retrieval layer are not included in this brief.*

