# Next Steps for International Education News Aggregator

## Development Environment

- Create a repository with the file structure from the tech spec
- Set up Python environment with required dependencies (`requirements.txt`)
- Configure authentication for Gmail, Reddit, Claude (via `.env`), and Google Docs
- Test minimal API requests with dummy data to confirm access
- Use `.env` for API keys and secrets (do not commit to repo)

## Research & Validation

- Verify API quotas and rate limits for Gmail, Reddit, Claude, and Google Docs
- Test 3–5 real content samples with Claude 3 Haiku for relevance filtering and topic extraction
- Check token usage and chunk size needed for Claude 3 Haiku (max input ~4k tokens)
- Review structure of Gmail newsletters and Reddit posts (titles, bodies, links, metadata)
- Review Reddit comment thread structure for future expansion
- Confirm that Claude output is structured enough for downstream formatting

## Implementation Plan

- Break down into small tasks with input/output and acceptance criteria for each
- Prioritize critical path components: collectors → relevance filter → analyzer → generator
- Build and test each component with minimal viable content
- Start with Reddit posts only; skip comments in MVP to reduce complexity and cost
- Track actual runtime and token usage to validate performance assumptions

## Documentation

- Create `PROMPTS.md` to document prompt engineering for Claude (relevance and topic extraction)
- Document expected input/output between components (e.g. JSON formats)
- Create CLI documentation (`README.md`) for running the tool
- Document common error scenarios and how to debug or recover from them
- Version control structured prompt changes and keep examples

## Optional (Recommended)

- Test plan for verifying each module works (unit + integration level)
- Example outputs for structured topic extraction and final newsletter format
- Note limitations: token limits, cost boundaries, false positives/negatives in filtering