# INDEX — Research Synthesis Engine

A client-side research aggregation tool that queries 18 academic databases in parallel, reconstructs abstracts, and synthesizes answers using weighted TF-IDF extraction.

## What It Does

1. **Searches 18 research sources in parallel** — arXiv, PubMed, Crossref, OpenAlex, Semantic Scholar, DOAJ, CORE, Zenodo, bioRxiv, medRxiv, PLOS, Europe PMC, DBLP, SSRN, IEEE, ResearchGate, Unpaywall, Wikipedia
2. **Synthesizes answers** using TF-IDF sentence scoring with inverse document frequency weighting, stop-word filtering, and research-term boosting — pulls the highest-relevance sentences across all sources and orders them coherently
3. **Shows all sources** with badges, snippets, direct links, and filterable tabs by database
4. **Evaluates research bias** using the BIAS FREE Framework across three dimensions: hierarchy, framing, and double standards

## How to Use

1. Open `index.html` in any modern browser
2. Type a research question — or click a suggested topic pill
3. Press Enter or click the search button
4. Watch the loading dashboard as each database reports back
5. Read the synthesized answer, browse sources (filterable by database), and review the bias framework
6. All data is stateless — resets on reload

## Key Features

- **Real extraction, not filler** — Answers are pulled from actual research snippets via weighted scoring, not templated text
- **18 parallel sources** — Preprints, open access journals, aggregators, biomedical databases, and CS indexes
- **Live search dashboard** — See each database resolve in real time with result counts and success/failure states
- **Source filtering** — Tab-based filtering to narrow results by database
- **Abstract reconstruction** — OpenAlex inverted indexes are rebuilt into readable abstracts; PubMed IDs are enriched via eSummary
- **Proper API parsing** — arXiv uses Atom XML parsing instead of a nonexistent JSON endpoint; bioRxiv/medRxiv route through Europe PMC's search API
- **No tracking** — Stateless, no cookies, no localStorage, no server
- **Single file** — Everything in one HTML file with an embedded SVG favicon

## Architecture

- **Frontend**: Vanilla HTML/CSS/JS, single file, no build step
- **Typography**: Playfair Display + Source Sans 3 (Google Fonts)
- **APIs**: 18 free public research APIs (no keys required)
- **Synthesis**: TF-IDF with IDF weighting, stop-word removal, query-term boosting (3×), research-term boosting, and length normalization
- **Framework**: BIAS FREE methodology (hierarchy, framing, double standards)
- **Favicon**: Inline SVG data URI — no external assets

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

Sources marked "API-limited" are included in the architecture and will return results if their APIs become publicly accessible. They fail gracefully and don't block the search.

## Limitations

- Some APIs enforce rate limits — heavy use may slow responses
- Synthesis quality depends on the snippet quality returned by each API
- Stateless by design — no saved history, no persistent state
- SSRN, IEEE Xplore, ResearchGate, and Unpaywall lack free search endpoints and return empty results currently
- bioRxiv and medRxiv are routed through Europe PMC, which may not capture all preprints

## Potential Extensions

- Google Scholar / JSTOR / ProQuest integration (requires API keys or scraping)
- localStorage toggle for search history
- PDF/Markdown export of results
- Citation generation (BibTeX, APA)
- Advanced query syntax (boolean operators, field-specific search)
- Clustering sources by topic or methodology

## Notes

Runs entirely in the browser. No server, no backend, no data collection. Built on free public APIs — no keys required for active sources.

**Respect API rate limits and terms of service for each data source.**
