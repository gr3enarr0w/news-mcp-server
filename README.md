# News MCP Server 🗞️

Multi-source news aggregation MCP server with AI synthesis, bias removal, and conversation-aware caching. Aggregates news from **7 APIs + unlimited RSS feeds** with **7,300+ free daily requests** total.

## 🚀 Features

### Core Capabilities
- **Multi-Source Aggregation**: 7 news APIs + RSS feeds from major outlets
- **AI Bias Removal**: Extract verifiable facts, remove editorial language
- **Conversation-Aware Caching**: Persistent results during chat sessions
- **Unbiased Synthesis**: Combine perspectives from multiple sources
- **Breaking News**: Real-time updates from last 2 hours
- **25 MCP Tools**: Comprehensive news operations

### News Sources (7,300+ Daily API Requests)
| API | Daily Limit | Coverage |
|-----|-------------|----------|
| **NewsData.io** | 200 (FREE) | Global news, multiple languages |
| **Guardian** | 5,000 (FREE) | British journalism, world coverage |
| **NewsAPI.ai** | ~67 (2000/month) | Event-based news analysis |
| **GNews** | 100 (FREE) | Google News aggregation |
| **TheNewsAPI** | 100 (FREE) | Multi-source compilation |
| **MediaStack** | ~17 (500/month) | Historical and current news |
| **CurrentsAPI** | ~20 (600/month) | Regional news coverage |

### RSS Sources (Unlimited)
- **New York Times** (7 categories)
- **BBC News** (World, Politics, Business, Tech, Health, Science)
- **Reuters** (Breaking news, World, Business)
- **Associated Press** (Top stories, Politics, World)
- **CNN** (Latest news, Politics, World, Tech)
- **Christian Science Monitor** (For balanced perspective)

## 📦 Installation & Usage

### Claude Code Integration (Recommended)

1. **Download the bridge script**:
```bash
curl -O https://raw.githubusercontent.com/gr3enarr0w/news-mcp-server/main/mcp-simple-bridge.sh
chmod +x mcp-simple-bridge.sh
```

2. **Add to your Claude Code configuration**:
```json
{
  \"mcpServers\": {
    \"news-server\": {
      \"command\": \"./mcp-simple-bridge.sh\",
      \"env\": {
        \"GUARDIAN_API_KEY\": \"your-key-here\",
        \"GNEWS_API_KEY\": \"your-key-here\"
      }
    }
  }
}
```

3. **Usage**: The server provides conversation-aware caching - search results persist during your chat session but clear when done.

### Docker Compose (Unraid/Always-On)

```yaml
services:
  news-mcp-server:
    image: gr3enarr0w/news-mcp-server:latest
    container_name: news-mcp-server
    restart: unless-stopped
    environment:
      - GUARDIAN_API_KEY=your-key-here
      - GNEWS_API_KEY=your-key-here
      - REDIS_URL=redis://redis:6379
    depends_on:
      - redis
  
  redis:
    image: redis:7-alpine
    container_name: news-redis
    restart: unless-stopped
```

## 🔧 API Keys (Optional but Recommended)

Get free API keys to unlock full functionality:

| Service | Free Tier | Registration |
|---------|-----------|-------------|
| **Guardian** | 5,000/day | [Register](https://open-platform.theguardian.com/access/) |
| **GNews** | 100/day | [Register](https://gnews.io/) |
| **TheNewsAPI** | 100/day | [Register](https://www.thenewsapi.com/) |
| **CurrentsAPI** | 600/month | [Register](https://currentsapi.services/) |
| **MediaStack** | 500/month | [Register](https://mediastack.com/) |

> **Note**: RSS feeds work without any API keys! You get news from NYT, BBC, Reuters, AP, CNN, and CSM immediately.

## 🛠️ Available Tools

### Core News Functions
- `get_latest_news` - Latest from all sources
- `search_news` - Search across 7 APIs + RSS
- `get_breaking_news` - Last 2 hours only
- `get_by_source` - Specific outlet (NYT, BBC, WSJ, etc.)
- `get_by_category` - Politics, Tech, Business, etc.

### AI Analysis Tools  
- `synthesize_topic` - Multi-source neutral summary
- `remove_bias` - Extract verifiable facts only
- `compare_coverage` - How different sources cover same story
- `create_balanced_view` - Unbiased perspective synthesis
- `ask_source` - Query specific outlet's coverage

### Utility Functions
- `get_api_status` - Check API limits and availability
- `get_headlines` - Title + link only for quick scanning
- Plus 15+ additional specialized tools

## 🏗️ Architecture

### Conversation-Aware Caching
```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Claude Code   │───▶│  MCP Bridge      │───▶│  Docker Container│
│                 │    │  (Conversation   │    │  (30min persist) │
│                 │    │   Management)    │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                         │
                                                         ▼
                                               ┌─────────────────┐
                                               │  Redis Cache    │
                                               │  (Conv-Scoped)  │
                                               └─────────────────┘
```

### Cache Strategy
- **Articles**: 30 minutes (conversation length)  
- **API Responses**: 5 minutes (refresh rate)
- **Synthesis**: 30 minutes (AI processing expensive)
- **Breaking News**: 5 minutes (updates frequently)

## 🔒 Security & Performance

- **Read-only containers** with non-root user
- **Resource limits**: 512MB RAM, 1 CPU max
- **Automatic cleanup** of old conversation containers  
- **Redis memory management** with LRU eviction
- **Rate limiting** and request throttling

## 📊 Unraid Template

Available as Docker template for Unraid users:
- Pre-configured environment variables
- Health checks and monitoring
- Resource management
- Automatic updates

## 🤝 Contributing

This MCP server was generated with Claude Code and optimized for:
- High-volume news aggregation
- Bias-free reporting
- Conversation persistence
- Enterprise reliability

## 📄 License

MIT - Use freely for personal and commercial projects.

---

**Claude Code Generated** • [GitHub](https://github.com/gr3enarr0w/news-mcp-server) • [Docker Hub](https://hub.docker.com/r/gr3enarr0w/news-mcp-server)