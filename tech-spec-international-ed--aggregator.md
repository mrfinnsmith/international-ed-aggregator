# Technical Specification: International Education News Aggregator

## System Overview
Python CLI application running locally that fetches content from Gmail and Reddit, processes it through Claude API for filtering and analysis, and generates a draft newsletter in Google Docs. May be manually triggered or scheduled via cron/serverless in future versions.

## Components

### Content Collector

- **Gmail Fetcher**: Python module using Gmail API
  - Retrieves emails with "Newsletters" label within date window
  - Extracts text content, links, and metadata
  - Implementation: `google-api-python-client`

- **Reddit Scraper**: Python module using PRAW
  - Pulls posts from r/IntltoUSA, r/f1visa, r/immigration, r/USCIS
  - Collects titles, body text, links, `created_utc`, `score`, timestamps
  - Implementation: `praw` with built-in rate limiting
  - *Note*: Comments are excluded for MVP. Threaded comment analysis could be explored in a later version.

### Content Processor

- **Relevance Filter**: Initial pass using Claude 3 Haiku
  - Input: Raw content chunks (emails, posts)
  - Process: Determines if entire email/post contains relevant international education content
  - Output: JSON with relevant content segments
  - Implementation: `anthropic` Python SDK

- **Content Analyzer**: Deep analysis using Claude 3 Haiku
  - Input: Content that passed the relevance filter
  - Process: Extract topics, score importance, identify trends
  - Output: Structured JSON with topics, sources, scores
  - Implementation: `anthropic` SDK with prompt chaining

### Newsletter Generator

- **Draft Creator**: Google Docs generation
  - Input: Structured topic data
  - Process: Format topics by relevance score
  - Output: Google Doc with draft newsletter
  - Format: each topic is a section heading with 1–3 bullet points (summaries + source + link)
  - Implementation: `google-api-python-client`

### Analytics Tracker

- **Feedback Logger**: Simple logging system
  - Records kept/removed topics after manual review
  - Used to support future relevance/importance tuning (not yet implemented)
  - Implementation: Local JSON logs manually maintained during editing

## Tools/Services

| Service       | Purpose               | API/Library              | Limits                          | Cost           |
|---------------|------------------------|---------------------------|----------------------------------|----------------|
| Gmail API     | Fetch newsletters      | `google-api-python-client` | 1M units/day                    | Free           |
| Reddit API    | Fetch subreddit posts  | `praw`                     | 60 requests/min (confirmed)     | Free           |
| Claude API    | Text analysis          | `anthropic` SDK (Claude 3 Haiku) | 200k token context window     | ~$15/month     |
| Google Docs API | Newsletter creation | `google-api-python-client` | 300 req/min/user               | Free           |

## Data Flow

1. **Collection**: Emails and Reddit posts retrieved, preserving text, links, `created_utc`, `score`, and formatting
2. **Filtering**: Content chunks passed to Claude for relevance scoring
3. **Analysis**: Relevant chunks analyzed for topics and importance
4. **Aggregation**: Topics grouped by theme, scored, and ranked
5. **Generation**: Draft newsletter created in Google Docs
6. **Feedback**: Manual edits logged for improvement

## Error Handling

| Error Scenario        | Handling Approach                                              |
|-----------------------|----------------------------------------------------------------|
| API failures          | Retry with exponential backoff (max 3 attempts)               |
| Content parsing errors| Skip content, log specific parsing errors                     |
| Token limit exceeded  | Chunk content with overlap and process sequentially           |
| Budget threshold      | Alert when 80% of Claude monthly budget reached               |
| Retry failure         | Log all failed retries with error type and content ID         |

## Security/Privacy

- API credentials stored in local `.env` file (excluded from repo)
- Content stored locally for processing
- All source links preserved for attribution

## Implementation Details

### Main Application Structure
news_aggregator/
├── collectors/
│   ├── gmail_collector.py
│   └── reddit_collector.py
├── processors/
│   ├── relevance_filter.py
│   └── content_analyzer.py
├── generators/
│   └── newsletter_generator.py
├── utils/
│   ├── config.py
│   ├── logging.py
│   └── api_helpers.py  # includes Claude and Google API helpers
├── main.py
└── requirements.txt

### Execution Flow

1. Configure date range and parameters
2. Collect content from sources
3. Perform initial relevance filtering
4. Analyze relevant content for topics
5. Generate draft newsletter
6. Present for manual review
7. Log feedback

## Limitations

- Processing time: Estimated 15–30 minutes per full run
- Content volume: Max ~100 emails and ~500 Reddit posts per run
- API rate limits require pacing
- Claude token context limit requires chunking if content is large
- No deduplication or caching in V1 (repeat content might show up)

## Deployment

- Local CLI execution: `python main.py --days 7 --output_doc "Newsletter Draft"`
- Manual run only in MVP
- Optional future deployment via cron, serverless, or Heroku

## Development Timeline

| Component           | Tasks                                | Time Estimate |
|---------------------|----------------------------------------|----------------|
| Setup               | Environment, libraries, authentication | 1 day          |
| Content Collection  | Gmail and Reddit collectors            | 3 days         |
| Content Processing  | Relevance filter and deep analysis     | 4 days         |
| Newsletter Generation | Google Docs creation                 | 2 days         |
| Analytics           | Feedback logging                       | 1 day          |
| Integration         | End-to-end testing                     | 3 days         |

**Total: 14 days**