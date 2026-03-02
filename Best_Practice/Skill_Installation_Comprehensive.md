# OpenClaw Skill Installation Comprehensive Guide

## 1. Tavily Search Skill Installation (2026-03-02)

### Method Used
- Direct npm installation: `npm install -g openclaw-tavily`
- Manual copy to OpenClaw skills directory
- API key configuration via environment variable

### Key Steps
1. Install package globally
2. Locate skill directory in node_modules
3. Copy to OpenClaw skills directory
4. Configure TAVILY_API_KEY in ~/.bashrc
5. Verify functionality with test query

### Challenges
- clawhub command not available initially
- Environment variable persistence issues
- Required manual directory copying

## 2. Find-Skills Skill Installation (2026-03-02)

### Method Used
- Manual skill creation due to clawhub rate limits
- Custom SKILL.md and script implementation

### Key Steps
1. Create skill directory structure
2. Write SKILL.md with proper metadata
3. Implement search.mjs script using clawhub CLI
4. Test basic functionality

### Challenges
- clawhub registry rate limiting
- Security warnings on some skills
- Required manual implementation

## 3. Proactive-Agent-1-2-4 Skill Installation (2026-03-02)

### Method Used
- Manual skill creation due to clawhub limitations
- Custom heartbeat monitoring implementation

### Key Steps
1. Create skill directory and SKILL.md
2. Implement heartbeat.mjs for configuration file checking
3. Adapt to actual workspace structure (files in root, not memory subdirectory)
4. Test configuration validation

### Challenges
- Expected directory structure differed from actual
- Missing MEMORY.md and ONBOARDING.md files in workspace
- Required custom path handling

## 4. Agent-Browser Alternative (2026-03-02)

### Method Used
- Leveraged built-in OpenClaw browser functionality
- No additional installation required

### Key Steps
1. Discovered OpenClaw has native `browser` command
2. Verified functionality with snapshot test
3. Confirmed full browser automation capabilities

### Advantages
- No external dependencies
- Built-in security and reliability
- Full feature set (snapshot, navigate, interact, etc.)

## 5. Official Authority Skill Collection (2026-03-02)

### Packages Installed
- **@anthropic-ai/claude-code**: Official Anthropic CLI tool
- **skills**: Universal AI agent skills ecosystem  
- **openskills**: Anthropic SKILL.md format support
- **nano-pdf**: PDF editing with natural language
- **Existing built-in skills**: pptx-creator, openai-image-gen, qwen-image, url-digest

### Functional Coverage
#### Document Processing
- ✅ **PPTX**: pptx-creator (built-in)
- ✅ **PDF**: nano-pdf (installed)  
- ⚠️ **Word**: Not directly available, but can use general document tools

#### Creative Creation  
- ✅ **Image Generation**: openai-image-gen, qwen-image (both built-in)
- ✅ **Content Creation**: Multiple text generation capabilities

#### Development Tools
- ✅ **Web Artifacts**: url-digest for content extraction
- ✅ **Search**: tavily-search for AI-optimized web search
- ✅ **Browser Automation**: Built-in browser command

### Installation Strategy
- Use npm global installation when possible
- Fall back to manual skill creation for clawhub limitations  
- Leverage existing built-in OpenClaw capabilities
- Prioritize official/maintained packages over community forks

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