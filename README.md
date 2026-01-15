# URL to Markdown

Convert any webpage to clean, LLM-friendly markdown. Powered by [Crawl4AI](https://github.com/unclecode/crawl4ai).

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/render-examples/crawl4ai)

## Overview

This template deploys a web app with a simple UI and REST API that crawls URLs and returns their content as clean markdown, optimized for LLM consumption. It handles JavaScript-rendered pages, filters out boilerplate content, and supports batch processing.

## Ideas and use cases

**RAG and knowledge bases:**

- Build a company knowledge base by crawling documentation sites
- Create a searchable archive of blog posts or news articles
- Feed product pages into a shopping assistant chatbot
- Index competitor websites for market research queries

**Content pipelines:**

- Summarize long articles with GPT-4 or Claude
- Translate web content to other languages
- Extract key facts from research papers
- Generate social media posts from blog articles

**Data extraction:**

- Pull pricing tables from e-commerce sites
- Extract job listings from career pages
- Gather contact information from business directories
- Collect event details from calendars and schedules

**Monitoring and alerts:**

- Track changes on competitor pricing pages
- Monitor news sites for mentions of your brand
- Watch documentation for API changes
- Alert when new content is published

**AI agents and automation:**

- Let agents browse the web to answer questions
- Auto-generate reports from multiple sources
- Create daily digests from industry news
- Build research assistants that cite sources

**Training and fine-tuning:**

- Collect domain-specific text for fine-tuning models
- Build datasets from niche websites
- Gather examples for few-shot prompting
- Create evaluation sets from real-world content

## Endpoints

| Endpoint       | Method | Description                                |
| -------------- | ------ | ------------------------------------------ |
| `/`            | GET    | Web UI for converting URLs to markdown     |
| `/health`      | GET    | Health check                               |
| `/docs`        | GET    | Interactive API documentation (Swagger UI) |
| `/crawl`       | POST   | Crawl a single URL and return markdown     |
| `/crawl/batch` | POST   | Crawl multiple URLs concurrently (max 10)  |

## Usage

### Crawl a single URL

```bash
curl -X POST https://your-service.onrender.com/crawl \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com"}'
```

**Response:**

```json
{
  "url": "https://example.com",
  "title": "Example Domain",
  "markdown": "# Example Domain\n\nThis domain is for use in illustrative examples...",
  "word_count": 42,
  "success": true
}
```

### Crawl with options

```bash
curl -X POST https://your-service.onrender.com/crawl \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://news.ycombinator.com",
    "include_raw": true,
    "filter_threshold": 0.5
  }'
```

**Request options:**

| Field               | Type   | Default  | Description                                |
| ------------------- | ------ | -------- | ------------------------------------------ |
| `url`               | string | required | The URL to crawl                           |
| `include_raw`       | bool   | `false`  | Include unfiltered raw markdown            |
| `filter_threshold`  | float  | `0.4`    | Content filter aggressiveness (0-1)        |
| `wait_for_selector` | string | `null`   | CSS selector to wait for before extraction |
| `js_code`           | string | `null`   | JavaScript to execute before extraction    |

### Crawl multiple URLs

```bash
curl -X POST https://your-service.onrender.com/crawl/batch \
  -H "Content-Type: application/json" \
  -d '[
    "https://example.com",
    "https://httpbin.org/html"
  ]'
```

### Handle dynamic content

For pages that load content via JavaScript:

```bash
curl -X POST https://your-service.onrender.com/crawl \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/dynamic-page",
    "wait_for_selector": ".content-loaded",
    "js_code": "document.querySelector(\"button.load-more\").click()"
  }'
```

## Deployment

Click the **Deploy to Render** button above, or:

1. Fork this repository
1. Create a new **Blueprint** in your [Render Dashboard](https://dashboard.render.com/)
1. Connect your forked repository
1. Render automatically detects and deploys from `render.yaml`

## Environment variables

| Variable | Description             | Default |
| -------- | ----------------------- | ------- |
| `PORT`   | Port for the web server | `8000`  |

## Local development

```bash
# Clone the repository
git clone https://github.com/render-examples/crawl4ai.git
cd crawl4ai

# Build and run with Docker
docker build -t url-to-markdown .
docker run -p 8000:8000 url-to-markdown

# Visit http://localhost:8000 for the web UI
```

## Resources

- [Crawl4AI documentation](https://docs.crawl4ai.com/)
- [Crawl4AI GitHub](https://github.com/unclecode/crawl4ai)
- [FastAPI documentation](https://fastapi.tiangolo.com/)
- [Render Docker documentation](https://render.com/docs/docker)
