# Content Aggregator Tool

## Overview

This tool aggregates content from multiple sources, filters for relevance to your chosen topic, extracts key themes, and generates a draft newsletter. It's customizable to track any topic of interest across multiple content sources.

## Features

- **Automated Content Collection**: Fetches content from:
  - Gmail (using labels you configure)
  - Reddit posts from subreddits you specify
- **Intelligent Content Analysis**: Uses Claude API to:
  - Filter for relevance to your chosen topic
  - Extract specific themes and trends
  - Score importance based on customizable criteria
- **Newsletter Generation**: Creates a draft Google Doc with:
  - Organized topics based on relevance
  - Source references
  - Ready for human review

## System Requirements

- Python 3.7+
- Required API access:
  - Gmail API
  - Reddit API 
  - Claude API
  - Google Docs API

## Setup Instructions

### 1. Gmail API Setup

1. Create a Google Cloud Project:
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project
   - Enable the Gmail API and Google Docs API

2. Configure OAuth Consent Screen:
   - In your GCP project, go to "APIs & Services" > "OAuth consent screen"
   - Select "External" user type
   - Enter app information (name, user support email, developer contact)
   - Add scopes: `https://www.googleapis.com/auth/gmail.readonly` and `https://www.googleapis.com/auth/documents`
   - Add your email as a test user

3. Create OAuth Credentials:
   - Go to "APIs & Services" > "Credentials"
   - Create OAuth client ID, select "Desktop app" type
   - Download the JSON file and rename to `gmail_credentials.json`
   - Place in the project root directory

4. Gmail Configuration:
   - Create a label in Gmail for content you want to track
   - Configure the tool to use this label (in config.py)

### 2. Reddit API Setup

### 3. Claude API Setup

### 4. Environment Setup

1. Clone this repository
2. Install dependencies:
   ```
   pip3 install -r requirements.txt
   ```
3. Copy the example environment file:
   ```
   cp .env.example .env
   ```
4. Edit the `.env` file with your credential file paths

## Usage

## Process Flow

1. **Content Collection**: Tool fetches content from configured sources
2. **Initial Filtering**: Claude API identifies relevant content based on your criteria
3. **Topic Extraction**: Claude API analyzes content for trends and topics
4. **Newsletter Generation**: Draft is created in Google Docs
5. **Manual Review**: Human editor reviews and finalizes

## Troubleshooting

## License