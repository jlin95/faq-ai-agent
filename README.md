# evidently-docs-agent

A Q&A agent over the Evidently documentation. This is my project for the AI Shipping Labs AI Hero course.

## Setup

```bash
uv sync
export GROQ_API_KEY='your-key'   # or use direnv with a .envrc file
uv run jupyter notebook
```

- Never commit your API key. `.envrc` is in `.gitignore`.
- Start Jupyter after you set the key. Jupyter reads the environment only at start.

## Chunking with an LLM

- Notebook: `chunking.ipynb`
- The notebook uses the `openai` package with the Groq endpoint (`https://api.groq.com/openai/v1`).
- Model: `openai/gpt-oss-20b`.
- The LLM splits each Evidently doc into sections. Sections are separated by `===SECTION===`.
- `max_output_tokens` is 8000. The code raises an error if a reply is cut.
- Docs with empty content (4 of 95) are skipped.

## Checks in the notebook

| Check                      | What it shows                                         |
| -------------------------- | ----------------------------------------------------- |
| Chunk count and size range | Too many small chunks, or chunks that are too big     |
| Tiny chunks                | Fragments under 50 characters                         |
| Coverage                   | Text length in chunks, compared with the original doc |
| Missing lines              | Lines of the original that are not in the chunks      |

## Known limits

- The LLM rewrites the text. It does not copy it exactly.
- On long docs, some content can be dropped or reworded. In my tests, coverage ranged from 84% to 100% for the same doc across runs.
- I checked 3 of 91 docs. I did not check the rest.
- Free-tier rate limits can stop a full run. Failed docs are logged.
- Results can change between runs.

## Next steps

- [ ] Run all docs and review the worst coverage results.
- [ ] Compare with a heading-based split on real questions.
- [ ] Add search over the chunks.
