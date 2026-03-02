# OpenClaw Skill Installation Guide

## Tavily Search Skill Installation (2026-03-02)

### Prerequisites
- OpenClaw CLI installed
- Node.js/npm environment
- Tavily API key (free tier: 1000 searches/month)

### Installation Steps

#### 1. Install via npm
```bash
npm install -g openclaw-tavily
```

#### 2. Locate skill directory
```bash
npm list -g openclaw-tavily --depth=0
# Typically: /home/admin/.npm-global/lib/node_modules/openclaw-tavily
```

#### 3. Copy to OpenClaw skills directory
```bash
cp -r /home/admin/.npm-global/lib/node_modules/openclaw-tavily/skills/tavily-search /home/admin/.npm-global/lib/node_modules/openclaw/skills/
```

#### 4. Configure API Key
```bash
echo 'export TAVILY_API_KEY="your-api-key"' >> ~/.bashrc
source ~/.bashrc
```

#### 5. Verify installation
```bash
openclaw skills list
# Should show tavily-search as available (not missing)
```

### Available Tools
- `tavily_search` - Web search with AI answers
- `tavily_extract` - Extract content from URLs  
- `tavily_crawl` - Crawl websites recursively
- `tavily_map` - Discover site structure
- `tavily_research` - Deep research reports

### Troubleshooting
- **Missing skill**: Ensure TypeScript files are compiled or use script fallbacks
- **API key errors**: Verify environment variable is set and sourced
- **Permission denied**: Check file permissions in skills directory

### Alternative Installation Methods
If `clawhub` command is unavailable:
- Use npm global install (as above)
- Manual git clone + build (if source available)
- Direct copy from existing installations