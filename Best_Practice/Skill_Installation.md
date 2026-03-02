# OpenClaw Skill Installation Guide

## 1. Tavily Search Skill Installation (2026-03-02)

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

## 2. Find-Skills Skill Installation (2026-03-02)

### Method Used
Manual skill creation due to clawhub rate limits and security warnings.

### Installation Steps
1. Create directory: `mkdir -p /home/admin/.npm-global/lib/node_modules/openclaw/skills/find-skills`
2. Create SKILL.md with proper metadata and usage instructions
3. Create scripts/search.mjs implementing clawhub search functionality
4. Test with: `openclaw skill run find-skills --query "test"`

### Usage
- **Basic search**: `openclaw skill run find-skills --query "关键词1 关键词2"`
- **Precise search**: `openclaw skill run find-skills --query "小红书 卡片生成" --type "content-generation"`

## 3. Proactive-Agent-1-2-4 Skill Installation (2026-03-02)

### Method Used
Manual skill creation adapting to actual workspace structure.

### Installation Steps
1. Create skill directory and SKILL.md
2. Implement heartbeat.mjs for configuration file validation
3. Handle actual workspace structure (core files in root, not memory subdirectory)
4. Test configuration checking functionality

### Core Configuration Files Checked
- AGENTS.md, HEARTBEAT.md, MEMORY.md, ONBOARDING.md, SOUL.md, TOOLS.md, USER.md

## 4. Agent-Browser Alternative (2026-03-02)

### Method Used
Leveraged built-in OpenClaw browser functionality instead of external skill.

### Built-in Browser Commands
- **Snapshot**: `openclaw browser snapshot --url "https://example.com"`
- **Navigation**: `openclaw browser navigate --url "https://example.com"`
- **Full automation**: Use `openclaw browser --help` for complete options

### Advantages
- No external dependencies required
- Built-in security and reliability
- Full browser automation capabilities

## 5. Official Authority Skill Collection (2026-03-02)

### Installed Packages
- **@anthropic-ai/claude-code**: Official Anthropic CLI tool
- **skills**: Universal AI agent skills ecosystem  
- **openskills**: Anthropic SKILL.md format support
- **nano-pdf**: PDF editing with natural language

### Functional Coverage by Category

#### Document Processing
- ✅ **PPTX**: pptx-creator (built-in OpenClaw skill)
- ✅ **PDF**: nano-pdf (installed via npm)
- ⚠️ **Word**: Not directly available, use general document tools

#### Creative Creation  
- ✅ **Image Generation**: openai-image-gen, qwen-image (both built-in)
- ✅ **Content Creation**: Multiple text generation capabilities

#### Development Tools
- ✅ **Web Artifacts**: url-digest for content extraction
- ✅ **Search**: tavily-search for AI-optimized web search
- ✅ **Browser Automation**: Built-in browser command

## General Best Practices

### When clawhub fails:
1. Check if skill exists in npm registry directly
2. Look for built-in OpenClaw alternatives
3. Implement manual skill creation as last resort
4. Always verify functionality after installation

### Environment Configuration:
1. Set API keys in ~/.bashrc for persistence
2. Source environment before testing
3. Verify permissions on skill directories
4. Test with simple queries before complex operations

### Troubleshooting Common Issues:
- **Rate Limits**: Wait and retry, or use alternative methods
- **Security Warnings**: Review code manually before --force installation  
- **Missing Dependencies**: Install required binaries (python, uv, etc.)
- **Path Issues**: Verify OpenClaw skills directory location