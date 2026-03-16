# INDEX — Research Synthesis Engine

A client-side research aggregation tool that queries 18 academic databases in parallel, scores source quality, detects consensus, and synthesizes answers using a local NLP pipeline and optional free AI summarization via Hugging Face. Zero cost. No API keys. Runs entirely in the browser.

## What It Does

1. **Searches 18 research sources in parallel** — arXiv, PubMed, Crossref, OpenAlex, Semantic Scholar, DOAJ, CORE, Zenodo, bioRxiv, medRxiv, PLOS, Europe PMC, DBLP, SSRN, IEEE, ResearchGate, Unpaywall, Wikipedia
2. **Synthesizes answers with a local NLP pipeline** — TF-IDF with IDF weighting, bigram query matching, source quality weighting, length normalization, research-language boosting, and MMR (Maximal Marginal Relevance) diversity selection to avoid redundant sentences
3. **Optionally generates an AI summary** via Hugging Face's free BART-large-CNN inference API — no key required, automatic retry if the model is cold-starting
4. **Scores evidence confidence** — composite metric across result volume, source diversity, database tier, abstract richness, and query-match rate
5. **Detects source consensus** — pairwise Jaccard similarity clustering plus contradiction-language scanning to report whether sources agree, conflict, or address different facets
6. **Rates source quality** — each result is tiered by database reputation, abstract availability, and citation count (where available)
7. **Evaluates research bias** using the BIAS FREE Framework across three dimensions: hierarchy, framing, and double standards

## How to Use

1. Open `index.html` in any modern browser
2. Choose a mode: **AI + Local NLP** (Hugging Face + full pipeline) or **Local NLP Only** (zero external calls beyond the database queries)
3. Type a research question or click a suggested topic pill
4. Press Enter or click the search button
5. Watch the loading dashboard as each database reports back across three phases: querying, NLP processing, and rendering
6. Read the local NLP synthesis at the top, then the AI summary below it (if enabled)
7. Check the confidence score and consensus analysis
8. Browse sources (filterable by database), each with a quality badge
9. Review the bias framework prompts
10. All data is stateless — resets on reload

## Two Modes

| Mode | What It Does | External Calls |
|---|---|---|
| **AI + Local NLP** | Full pipeline plus Hugging Face BART abstractive summarization | Database APIs + one HF call |
| **Local NLP Only** | Full local pipeline, no AI summarization | Database APIs only |

The local NLP pipeline runs identically in both modes. The Hugging Face call is additive — if it fails or is slow, the local synthesis is already complete and visible.

## NLP Pipeline Details

The local synthesis engine runs entirely in the browser with no dependencies:

- **Tokenization** — lowercased, punctuation-stripped, with a 100+ word stop-word list
- **TF-IDF scoring** — term frequency normalized by sentence length, multiplied by inverse document frequency across all source sentences, with a 4x boost for query terms
- **Bigram matching** — query bigrams receive a +2 score bonus when matched in source sentences
- **Source quality weighting** — sentences from higher-tier databases (PubMed, Crossref, OpenAlex = tier 3) score higher than lower-tier sources (Wikipedia = tier 1). Citation count and abstract availability add further weight
- **Research language boosting** — sentences containing terms like "meta-analysis", "trial", "significant", "treatment", "outcome" receive a relevance bonus
- **Length normalization** — prefers medium-length sentences (~120 chars) over fragments or run-on text
- **MMR selection** — instead of picking the top N highest-scoring sentences (which often say the same thing), Maximal Marginal Relevance balances relevance against diversity using pairwise Jaccard similarity. Lambda = 0.6

## Confidence Scoring

A composite 0–100% score computed from five signals:

| Signal | Max Weight | What It Measures |
|---|---|---|
| Result volume | 25 pts | Raw number of results returned |
| Source diversity | 20 pts | Number of distinct databases that returned results |
| Source quality | 25 pts | Average quality tier across all results |
| Abstract richness | 20 pts | Proportion of results with substantive abstracts (>100 chars) |
| Query match rate | 10 pts | Proportion of results that match at least 50% of query terms |

Thresholds: 65%+ = High, 35–64% = Medium, below 35% = Low.

## Consensus Detection

Analyzes agreement across sources using two methods:

- **Jaccard similarity clustering** — computes pairwise word overlap between the top 15 source abstracts. High average similarity (>12%) with few contradictions indicates agreement. Low similarity (<6%) with high contradiction count indicates active disagreement.
- **Contradiction language scanning** — flags sources containing terms like "however", "contrary", "disputed", "conflicting", "inconsistent"

Reports one of four states: Sources Agree, Active Disagreement, Mixed Signals, or Insufficient Data.

## Source Quality Tiers

| Tier | Score | Sources |
|---|---|---|
| 3 (highest) | Peer-reviewed / curated | PubMed, Crossref, OpenAlex, Europe PMC, Semantic Scholar, PLOS |
| 2 | Preprints / open access | arXiv, bioRxiv, medRxiv, DOAJ, CORE, Zenodo, DBLP |
| 1 | General / limited API | Wikipedia, SSRN, IEEE, ResearchGate, Unpaywall |

Bonus points added for: substantive abstract text (>80 chars, non-generic), citation count >10, citation count >100.

## AI Summarization (Hugging Face — Free)

In **AI + Local NLP** mode, the top 8 source abstracts (by quality score) are concatenated and sent to Hugging Face's free inference endpoint for `facebook/bart-large-cnn`. This model generates an abstractive summary — it rephrases and condenses rather than extracting sentences verbatim.

- No API key required (uses the free anonymous tier)
- If the model is cold (first request in a while), HF returns a 503 with an estimated wait time. INDEX retries up to 3 times automatically
- If HF is unavailable, a fallback message appears and the local NLP synthesis remains fully functional
- Input is capped at 1024 characters to stay within free-tier limits

## Architecture

- **Frontend**: Single HTML file, vanilla JS, no build step, no dependencies
- **Typography**: Playfair Display + Source Sans 3 (Google Fonts)
- **Database APIs**: 18 free public APIs (no keys required)
- **Local NLP**: TF-IDF + IDF + bigram matching + MMR + Jaccard clustering + contradiction scanning
- **AI Layer**: Hugging Face Inference API (free, optional)
- **Favicon**: Inline SVG data URI

## Sources

| Category | Sources |
|---|---|
| Preprints | arXiv, bioRxiv, medRxiv |
| Published Research | Crossref, OpenAlex |
| Open Access | DOAJ, CORE, PLOS, Zenodo |
| Biomedical | PubMed, Europe PMC |
| Computer Science | DBLP, Semantic Scholar |
| General Knowledge | Wikipedia |
| Defined but API-limited | SSRN, IEEE, ResearchGate, Unpaywall |

## Limitations

- Hugging Face free tier can be slow (10–30s cold start) or temporarily unavailable during high traffic
- Synthesis quality depends on snippet quality — many APIs return only titles or short metadata, not full abstracts
- Consensus detection uses word overlap, not semantic similarity — topically related but differently-worded sources may register as disagreeing
- SSRN, IEEE Xplore, ResearchGate, and Unpaywall lack free search endpoints and return empty results
- bioRxiv and medRxiv are routed through Europe PMC, which may not capture all preprints
- Confidence scoring is heuristic, not calibrated — treat it as a rough signal, not a statistical measure
- No persistent state — search history, results, and settings reset on reload

## Potential Extensions

- Semantic similarity via a free embedding model (e.g. Hugging Face `sentence-transformers`) for better consensus detection
- Citation graph traversal using OpenAlex's cited-by data
- localStorage toggle for search history
- PDF/Markdown/BibTeX export
- Advanced query syntax (boolean operators, date ranges, field-specific search)
- Source clustering visualization
- Full-text retrieval via Unpaywall DOI lookups for open-access papers

## Notes

Runs entirely in the browser. No server, no backend, no tracking, no cookies, no localStorage. The only external calls are to the 18 database APIs and (optionally) one Hugging Face inference request. All free.

**Respect API rate limits and terms of service for each data source.**
