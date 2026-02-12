# Web MCP Server

Web fetch and search tools for AI agents. Based on [Claude Code's WebFetch and WebSearch tool designs](https://gist.github.com/bgauryy/0cdb9aa337d01ae5bd0c803943aa36bd).

## Setup

### Using tool CLI

Install the CLI from https://github.com/zerocore-ai/tool-cli

```bash
# Install from tool.store
tool install library/web
```

```bash
# Configure API keys (optional but recommended)
tool config set library/web brave_api_key=YOUR_BRAVE_API_KEY
```

```bash
# View available tools
tool info library/web
```

```bash
# Fetch a webpage
tool call library/web -m fetch -p url="https://example.com"
```

```bash
# Search the web
tool call library/web -m search -p query="rust async programming"
```

## Tools

### `fetch`

Fetches content from a URL and converts HTML to markdown.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `url` | string | Yes | URL to fetch (HTTP auto-upgrades to HTTPS) |
| `timeout_ms` | number | No | Request timeout in ms (default: 30000) |
| `max_length` | number | No | Max content bytes (default: 1MB, max: 10MB) |

**Output:**
| Field | Type | Description |
|-------|------|-------------|
| `content` | string | Fetched content (HTML converted to markdown) |
| `final_url` | string | Final URL after redirects |
| `status` | number | HTTP status code |
| `content_type` | string | MIME type |
| `truncated` | boolean | Whether content was truncated |

### `search`

Searches the web using the best available provider.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `query` | string | Yes | Search query (min 2 characters) |
| `max_results` | number | No | Max results (default: 10, max: 50) |
| `allowed_domains` | string[] | No | Only include results from these domains |
| `blocked_domains` | string[] | No | Exclude results from these domains |

**Output:**
| Field | Type | Description |
|-------|------|-------------|
| `results` | array | Search results with `title`, `url`, `snippet` |
| `count` | number | Number of results returned |
| `provider` | string | Search provider used |

## Search Providers

The server automatically selects the best available provider based on configured API keys:

| Priority | Provider | Config Key | Free Tier |
|----------|----------|------------|-----------|
| 1 | Brave Search | `brave_api_key` | 2000/month |
| 2 | Tavily | `tavily_api_key` | 1000/month |
| 3 | SerpAPI | `serpapi_api_key` | 100/month |
| 4 | DuckDuckGo | (none) | Unreliable* |

*DuckDuckGo uses HTML scraping and may trigger bot detection. Use an API-based provider for reliable results.

## Configuration

Configure API keys through the tool CLI:

```bash
# Recommended - best free tier
tool config set library/web brave_api_key=YOUR_BRAVE_API_KEY

# Alternative providers
tool config set library/web tavily_api_key=YOUR_TAVILY_API_KEY
tool config set library/web serpapi_api_key=YOUR_SERPAPI_API_KEY
```

### Getting API Keys

| Provider | Sign Up |
|----------|---------|
| Brave Search | https://brave.com/search/api/ |
| Tavily | https://tavily.com/ |
| SerpAPI | https://serpapi.com/ |

## License

Apache-2.0
