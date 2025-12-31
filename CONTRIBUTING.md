# Contributing to INDEX

Thanks for interest in contributing!

## How to Contribute

### Adding New Sources
1. Add a new `searchX()` function in `index.html`
2. Add it to the `searchAllSources()` Promise.allSettled array
3. Test it works
4. Update README.md sources table
5. Submit PR with description of API

### Improving Synthesis
- TF-IDF scoring logic is in `calculateScore()`
- Sentence extraction in `extractSentences()`
- Feel free to improve algorithms

### Reporting Issues
- Describe what broke
- Include search query that failed
- List which source(s) errored
- Share browser/OS info

## Guidelines

- Keep it lightweight (no heavy dependencies)
- No API keys in code
- Respect API rate limits
- Test before submitting PR
- Update docs if behavior changes

## Development

Just edit `index.html` and test in your browser. No build process needed.

## License

All contributions are MIT licensed.
