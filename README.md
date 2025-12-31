# INDEX – Research Synthesis Engine

A minimal, 2000s-style research aggregation tool that queries 15+ academic databases to synthesize real answers to your questions.

## What It Does

1. **Searches 15 research sources in parallel**: arXiv, PubMed, Crossref, OpenAlex, SSRN, ResearchGate, Semantic Scholar, Zenodo, CORE, bioRxiv, medRxiv, PLOS, IEEE Xplore, DOAJ, Wikipedia

2. **Synthesizes intelligent answers** using TF-IDF sentence extraction—pulls the most relevant sentences from all sources and combines them into coherent research-backed answers

3. **Shows all sources** with direct links to originals

4. **Assesses research bias** using the BIAS FREE Framework—checks for hierarchy bias, failure to examine differences, and double standards

## How to Use

1. Open `index.html` in any modern web browser
2. Type your research question
3. Click "Search" (or press Enter)
4. Read the synthesized answer at the top
5. Explore sources and bias framework below
6. All data resets when you reload the page

## Key Features

✅ **Real answers, not summaries** – Answers come from actual research snippets, not generic text
✅ **15 parallel sources** – Academic preprints, open access, social networks, discipline-specific databases
✅ **Clean design** – 2000s blog aesthetic, single scrolling page, no corporate bloat
✅ **No tracking** – Results reset on reload, no persistent data
✅ **Pure research only** – No news sites, blogs, or marketing content
✅ **Open, transparent** – See exactly where information comes from

## Architecture

- **Frontend**: Vanilla HTML/CSS/JS (single file, no dependencies)
- **APIs**: 15 free public research APIs (no keys required for most)
- **Synthesis**: TF-IDF-style keyword extraction + sentence scoring
- **Framework**: BIAS FREE methodology for structural bias detection

## Sources Included

| Category | Sources |
|----------|---------|
| Preprints | arXiv, bioRxiv, medRxiv |
| Published Research | Crossref, OpenAlex, IEEE Xplore |
| Open Access | DOAJ, CORE, PLOS, Zenodo |
| Academic Networks | ResearchGate, SSRN |
| Biomedical | PubMed |
| General Knowledge | Wikipedia |
| AI-Powered Search | Semantic Scholar |

## Limitations

- Some APIs may have rate limits (searches slow if overused)
- Synthesis quality depends on source snippet quality
- Results reset on page reload (no saved history)
- Some databases may be temporarily unavailable

## Next Steps

- Add more sources (Google Scholar, JSTOR, ProQuest)
- Improve synthesis with better NLP models
- Add search history (localStorage option)
- Add export/PDF download of results
- Support advanced search operators

## Notes

This tool is built on public, free research APIs. No API keys required for most sources. Runs entirely in your browser—no server backend, no data collection.

**Use responsibly**: Respect API rate limits and terms of service for each data source.
