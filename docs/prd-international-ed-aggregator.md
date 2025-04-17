# Product Requirements Document: International Education News Aggregator

## Personas  
Primary: advisors, DSOs  
Secondary: international students, recruiters, service providers

## User Problem  
International education stakeholders (students, advisors/DSOs, recruiters, agencies, service providers) struggle to track important information across scattered sources during a period of unpredictable policy changes.

## Goals  
- Reduce time users spend gathering information  
- Improve decision quality  
- Prevent users from missing important updates  

## Success Metrics  
- Subscriber growth rate  
- Retention/churn rate  
- Open rates  
- Click-through rates on sources  
- % of topics retained after manual review  

## Timeline  
- Operational MVP within two weeks (capable of generating first draft newsletter)

## Scope & Constraints  
- Budget: Maximum $20/month
  - Include alerts and usage limits for all tools to prevent cost overruns
- Sources:  
  - Gmail newsletters (using "Newsletters" label)  
  - Specific subreddits: r/IntltoUSA, r/f1visa, r/immigration, r/USCIS  
- Explicit exclusion: **Twitter/X must not be used**  
- Content focus: US immigration/education policies only  
- Output limitations:  
  - Maximum 10 topics per newsletter  
  - Text only (no images/charts in V1)  
  - Weekly or bi-weekly frequency  
  - Content limited to the period since the last issue (typically 7 or 14 days)  
- Development: Local development using VSCode/Cursor
- Deployment: Local execution or Heroku if continuous operation needed

## Risks & Assumptions  
- API rate limits for Gmail and Reddit  
- Claude API token usage limits  
- Processing time constraints  
- Integration limitations between services  
- False positives/negatives in relevance filtering  
- API outages breaking the workflow  
- Content misclassification  
- Inconsistent quality in theme extraction  
- Cost overruns if processing exceeds expected volumes  

## Process Flow  
1. **Content Collection**  
   - Poll Gmail API for "Newsletters" label emails (matching frequency window)  
   - Retrieve posts from target subreddits (matching frequency window)  
2. **Initial Relevance Filtering**  
   - Use Claude API to identify potentially relevant content  
   - Filter criteria: mentions of international education, students, visas, studying abroad  
   - Threshold: low to avoid false negatives  
3. **Deep Content Analysis**  
   - Apply Claude API to extract specific topics from relevant content  
   - Identify trending topics appearing across multiple sources  
   - Group related mentions under common themes  
   - Score importance based on:  
     - Frequency across sources  
     - Impact severity (high/medium/low)  
     - Urgency based on deadlines  
     - Geographic scope  
   - Score relevance based on:  
     - Explicit mentions of international education  
     - Impact on target personas  
     - Timeliness  
4. **Newsletter Draft Generation**  
   - Create Google Doc with extracted trends and topics  
   - Format: topic headings with brief descriptions and source references  
   - Sort by relevance/importance score  
5. **Manual Review**  
   - System delivers draft for human editing  
   - Analytics capture which topics were kept/removed  
   - Track manual edits to improve automated filtering over time

## Out of Scope  
- Non-US education/immigration content  
- Twitter/X  
- Job listings or career advice  
- Visual content (charts, infographics, etc.)  
- Newsletter distribution (manual for MVP)
- Future enhancements (social media except Twitter/X, forums, government data integration)

## Open Questions  
- How can the manual review process feed back into filtering and analysis to improve automation over time?