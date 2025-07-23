# MCP (Model Context Protocol) Power User Guide

## Table of Contents
1. [Overview](#overview)
2. [Installed MCP Servers](#installed-mcp-servers)
3. [Server Capabilities & Tools](#server-capabilities--tools)
4. [Development Workflows](#development-workflows)
5. [Power User Tips](#power-user-tips)
6. [Command Reference](#command-reference)
7. [Troubleshooting](#troubleshooting)

## Overview

MCP servers extend Claude's capabilities by providing specialized tools for different domains. Think of them as plugins that give Claude superpowers for specific tasks. You have **8 powerful MCP servers** installed that transform Claude from a chatbot into a full development environment.

### Token Economics & Efficiency

**Understanding Token Costs:**
- Each MCP tool call has a token cost ranging from minimal (100-500) to substantial (5,000-20,000)
- Context window usage accumulates quickly with file reads and multi-tool workflows
- Smart tool selection can reduce token usage by 50-80%

**Efficiency Guidelines:**
| Operation | Efficient Choice | Token Cost | Inefficient Choice | Token Cost |
|-----------|-----------------|------------|-------------------|------------|
| Read docs (indexed) | Context7 | ~2-5k | Zen research | ~10-20k |
| Read docs (any GitHub) | GitMCP | ~1-3k | Web search + parse | ~10k+ |
| Simple file read | Desktop Commander | ~500 | Task + multiple tools | ~5-10k |
| Code search | Desktop Commander ripgrep | ~1k | Reading all files | ~20k+ |
| Quick fix | Direct edit | ~1k | Zen refactor full analysis | ~15k |
| Task planning | Task Master | ~2k | Manual tracking | ~5k+ |
| GitHub operation | GitHub MCP | ~1-3k | Multiple git commands | ~5k+ |
| Web documentation | Crawl4AI RAG | ~2-5k | Manual crawling | ~15k+ |
| AI validation | Crawl4AI hallucination check | ~3k | Manual verification | ~10k+ |

**Token-Saving Strategies:**
1. **Search before read**: Use ripgrep to find specific content instead of reading entire files
2. **Batch operations**: Combine multiple file operations in single tool calls
3. **Choose the right depth**: Use Zen's "minimal" or "low" thinking modes for simple tasks
4. **Reuse contexts**: Keep REPLs alive, use continuation IDs, maintain default repos
5. **Targeted queries**: Request specific documentation sections, not entire libraries
6. **GitHub efficiency**: Use batch operations like push_files for multiple file updates
7. **Documentation hierarchy**: Context7 → GitMCP → Crawl4AI (in order of efficiency)

### Quick Start Commands
```bash
# List all MCP servers
claude mcp list

# Add a new MCP server
claude mcp add <server-name>

# Remove an MCP server
claude mcp remove <server-name>

# View server logs (for debugging)
claude mcp logs <server-name>
```

## Installed MCP Servers

### 1. 🐙 GitHub MCP
**Purpose**: Complete GitHub API integration for repository and project management  
**Adoption**: Extremely popular (built-in with Claude Code)  
**Best for**: Repository management, issues, PRs, CI/CD, code collaboration  
**Tools**: 70+ GitHub API operations  
**Efficiency note**: Low-moderate tokens (1-3k). Most efficient for GitHub operations vs multiple git commands.

### 2. 🧘 Zen MCP (v5.8.2)
**Purpose**: Advanced AI reasoning and analysis tools  
**Adoption**: 4,912 stars (High activity)  
**Best for**: Complex problem-solving, code reviews, debugging, documentation generation  
**Tools**: 16 specialized AI tools  
**Efficiency note**: High token usage (5-20k per call). Use for complex tasks that truly need AI reasoning. For simple operations, consider direct implementation.

### 3. 📚 Context7
**Purpose**: Real-time documentation lookup for indexed libraries/frameworks  
**Adoption**: 21,221 stars (Most popular)  
**Best for**: Finding up-to-date docs for 2000+ indexed libraries  
**Tools**: 2 tools (resolve-library-id, get-library-docs)  
**Efficiency note**: Moderate tokens (2-5k). Fastest for indexed libraries. Use GitMCP for non-indexed.

### 4. 💻 Desktop Commander (v0.2.6)
**Purpose**: File system operations and process management  
**Best for**: File manipulation, running commands, data analysis with Python/Node REPL  
**Tools**: 20 file/process tools  
**Efficiency note**: Minimal tokens (~500). Most efficient for file operations. Prefer over Bash for safety.

### 5. 📋 Task Master (v0.21.0)
**Purpose**: AI-powered project and task management  
**Adoption**: 19,121 stars (Extremely popular)  
**Best for**: Breaking down complex projects, tracking progress, generating tasks from PRDs  
**Tools**: 35 project management tools  
**Efficiency note**: Moderate tokens (2-3k). Excellent for maintaining context across long projects.

### 6. 🧠 Sequential Thinking
**Purpose**: Step-by-step problem decomposition  
**Best for**: Complex algorithms, multi-step solutions, planning  
**Tools**: 1 tool (sequentialthinking)  
**Efficiency note**: Variable tokens (1-5k per step). Use for genuinely complex problems requiring iterative refinement.

### 7. 🔍 GitMCP *[NEW]*
**Purpose**: Generic GitHub repository documentation retrieval  
**Best for**: Documentation for ANY GitHub repo (not just indexed ones)  
**Tools**: 5 tools for doc/code search  
**Efficiency note**: Low tokens (1-3k). Perfect complement to Context7 for non-indexed libraries.

### 8. 🕷️ Crawl4AI RAG *[NEW]*
**Purpose**: Web crawling, RAG queries, and AI validation  
**Best for**: Web documentation, AI hallucination detection, knowledge graphs  
**Tools**: 8 tools including crawling, RAG, and validation  
**Efficiency note**: Moderate tokens (2-5k). Unique capability for web content and AI validation.

## Efficiency Decision Tree

**"What tool should I use?"**

```
Need documentation?
├─ Is it a popular library?
│  ├─ YES → Context7 (2000+ indexed libraries)
│  └─ NO → Is it on GitHub?
│     ├─ YES → GitMCP (any GitHub repo)
│     └─ NO → Crawl4AI RAG (web crawl)
└─ Continue...

Working with GitHub?
├─ Repository ops → GitHub MCP
├─ Issues/PRs → GitHub MCP  
├─ CI/CD → GitHub MCP
└─ Local git → Bash tool

Working with files?
├─ Simple read/write → Desktop Commander
├─ Complex search → Desktop Commander (ripgrep)
└─ Multiple files → Desktop Commander (batch)

Need AI analysis?
├─ Complex debugging → Zen debug (worth the tokens)
├─ Code review → Zen codereview
├─ AI validation → Crawl4AI hallucination check
├─ Quick question → Direct implementation
└─ Research → Context7/GitMCP first, then Zen

Managing tasks?
├─ New project → Task Master (great ROI)
├─ Quick todo → Your memory (save tokens)
└─ Complex dependencies → Task Master

Complex problem solving?
├─ Algorithm design → Sequential Thinking
├─ Multi-step planning → Sequential Thinking
└─ Simple logic → Direct implementation

Web content needed?
├─ Documentation → Crawl4AI smart_crawl
├─ RAG search → Crawl4AI perform_rag_query
└─ Knowledge base → Crawl4AI + Supabase
```

## Server Capabilities & Tools

### 🐙 GitHub MCP - Complete GitHub Integration

[Previous GitHub MCP content remains the same - 70+ tools listed]

### 🧘 Zen MCP - Advanced AI Tools (v5.8.2)

**16 Key Tools:**
- `chat` - Collaborative brainstorming with AI models
- `thinkdeep` - Multi-stage investigation for complex problems
- `debug` - Systematic debugging with root cause analysis
- `codereview` - Comprehensive code quality assessment
- `secaudit` - Security vulnerability analysis
- `docgen` - Automatic documentation generation
- `refactor` - Code improvement suggestions
- `testgen` - Generate comprehensive test suites
- `analyze` - Deep code analysis and architecture review
- `planner` - Sequential planning for complex tasks
- `consensus` - Multi-model consensus for decisions
- `precommit` - Pre-commit validation and checks
- `tracer` - Code execution flow analysis
- `challenge` - Critical thinking tool (auto-triggers on disagreements)
- `listmodels` - View available AI models
- `version` - Check Zen MCP version and configuration

### 📚 Context7 - Documentation Lookup

[Previous Context7 content remains the same]

### 💻 Desktop Commander - System Operations (v0.2.6)

[Previous Desktop Commander content remains the same - 20 tools]

### 📋 Task Master - Project Management (v0.21.0)

[Previous Task Master content remains the same - 35 tools]

### 🧠 Sequential Thinking - Problem Decomposition

[Previous Sequential Thinking content remains the same]

### 🔍 GitMCP - Generic GitHub Documentation *[NEW]*

**Key Tools (5 total):**
- `match_common_libs_owner_repo_mapping` - Map library names to GitHub repos
- `fetch_generic_documentation` - Fetch docs from any GitHub repository
- `search_generic_documentation` - Semantic search in GitHub documentation
- `search_generic_code` - Code search in GitHub repositories
- `fetch_generic_url_content` - Fetch content from URLs

**Power Features:**
- Works with ANY GitHub repository (not limited to indexed ones)
- Complements Context7 for complete documentation coverage
- Low token usage (1-3k per operation)
- Supports pagination for code search
- Respects robots.txt for URL fetching

**When to Use:**
- Library not in Context7's index
- Private GitHub repositories
- Latest code that might not be indexed
- Custom or niche libraries

### 🕷️ Crawl4AI RAG - Web Crawling & AI Validation *[NEW]*

**Key Tools (8 total):**
- `crawl_single_page` - Crawl and store single web page
- `smart_crawl_url` - Intelligent crawling based on URL type
- `get_available_sources` - List crawled sources in Supabase
- `perform_rag_query` - RAG search on stored content
- `search_code_examples` - Find code examples in stored content
- `check_ai_script_hallucinations` - Validate AI-generated Python scripts
- `query_knowledge_graph` - Explore Neo4j knowledge graph
- `parse_github_repository` - Parse repo into knowledge graph

**Power Features:**
- Web content storage in Supabase for later retrieval
- AI hallucination detection using knowledge graphs
- Automatic sitemap detection and parallel crawling
- Code example extraction and search
- Neo4j integration for relationship mapping

**Unique Capabilities:**
- Only MCP with web crawling abilities
- Only MCP with AI hallucination detection
- Knowledge graph exploration for code understanding
- Persistent storage for crawled content

## Development Workflows

### 🚀 Full-Stack Development Workflow

[Previous workflow content remains the same]

### 📖 Documentation Research Workflow *[UPDATED]*

1. **Check Indexed Libraries First**
   ```
   "Show me React hooks documentation"
   → Context7 resolve-library-id
   → Get comprehensive docs with examples
   ```

2. **Non-Indexed GitHub Libraries**
   ```
   "Get documentation for small-github-library"
   → GitMCP match_common_libs
   → Fetch and search documentation
   ```

3. **Web Documentation**
   ```
   "Get the latest Kubernetes docs from their website"
   → Crawl4AI smart_crawl_url
   → Store in Supabase
   → RAG queries for specific topics
   ```

### 🔍 AI Validation Workflow *[NEW]*

1. **Generate Code with AI**
   ```
   "Create a data processing script"
   → Zen tools generate code
   → Save to file
   ```

2. **Validate for Hallucinations**
   ```
   "Check if this script has AI hallucinations"
   → Crawl4AI check_ai_script_hallucinations
   → Validates imports, methods, parameters
   → Reports confidence scores
   ```

3. **Build Knowledge Base**
   ```
   "Parse this repository into knowledge graph"
   → Crawl4AI parse_github_repository
   → Creates Neo4j graph of code structure
   → Enables better validation
   ```

### 🐛 Debugging Workflow

[Previous workflow content remains the same]

### 📊 Data Analysis Workflow

[Previous workflow content remains the same]

## Power User Tips

### 🎯 Efficiency Maximizers *[UPDATED]*

1. **Documentation Hierarchy**
   - Context7 for popular libraries (fastest)
   - GitMCP for any GitHub repo (flexible)
   - Crawl4AI for web content (comprehensive)

2. **AI Validation Best Practices**
   - Parse key repositories into knowledge graph first
   - Run hallucination checks on critical AI-generated code
   - Use confidence scores to guide manual review

3. **Smart Crawling**
   - Use Crawl4AI's smart_crawl for automatic URL type detection
   - Store frequently accessed docs in Supabase
   - Reuse stored content with RAG queries

[Rest of Power User Tips remain the same]

### 🔥 Advanced Techniques *[UPDATED]*

1. **Multi-Tool Documentation Flow**
   ```
   Popular lib → Context7
   GitHub lib → GitMCP  
   Web docs → Crawl4AI
   Analysis → Zen research
   ```

2. **Validation Pipeline**
   ```
   Generate (Zen) → Save (Desktop) → Validate (Crawl4AI) → Fix → Commit (GitHub)
   ```

[Rest of Advanced Techniques remain the same]

### 🔄 Tool Overlaps & Smart Choices *[UPDATED]*

**Documentation Tools:**
1. **Context7 vs GitMCP vs Crawl4AI**
   - Context7: Use for 2000+ indexed libraries (fastest, best examples)
   - GitMCP: Use for non-indexed GitHub repos (good for latest code)
   - Crawl4AI: Use for non-GitHub web documentation (most flexible)

2. **AI Validation**
   - Crawl4AI hallucination check: For Python scripts with imports
   - Zen codereview: For general code quality and patterns
   - Manual review: For business logic correctness

[Rest of Tool Overlaps remain the same]

## Command Reference *[UPDATED]*

### Essential Commands

```bash
# Documentation Commands
"Show me [library] docs"          # Context7 for indexed
"Get GitHub docs for [repo]"      # GitMCP for any repo
"Crawl docs from [url]"           # Crawl4AI for web

# Validation Commands
"Check script for hallucinations"  # Crawl4AI validation
"Parse repo into knowledge graph"  # Build validation DB

# Quick Access Patterns
"Using GitMCP, search [repo] for [topic]"
"Using Crawl4AI, get docs from [website]"
"Check if this AI code is valid"
```

[Rest of Command Reference remains the same]

## Troubleshooting *[UPDATED]*

### Common Issues

[Previous issues remain the same]

6. **GitMCP Not Finding Repository**
   - Use full owner/repo format
   - Check if repo is public
   - Try match_common_libs first

7. **Crawl4AI Connection Issues**
   - Verify Supabase/Neo4j credentials
   - Check URL accessibility
   - Respect rate limits

### Performance Tips *[UPDATED]*

4. **Smart Tool Selection by Token Cost**
   ```
   Minimal (<1k tokens): Desktop Commander, GitMCP fetch
   Low (1-5k tokens): Context7, Task Master, GitMCP search, Crawl4AI crawl
   Medium (5-10k tokens): Zen with "low" thinking, Sequential Thinking
   High (10-20k tokens): Zen with "high/max" thinking, complex research
   ```

5. **Documentation Efficiency Scale**
   ```
   Most Efficient → Least Efficient
   Context7 (indexed) → GitMCP (GitHub) → Crawl4AI (web) → Zen research (analysis)
   ```

## Quick Reference Card *[UPDATED]*

| Task | Tool | Example Command |
|------|------|----------------|
| Popular library docs | Context7 | "Show me React hooks documentation" |
| Any GitHub docs | GitMCP | "Get docs for owner/repo" |
| Web documentation | Crawl4AI | "Crawl the official docs site" |
| Validate AI code | Crawl4AI | "Check this script for hallucinations" |
| Build knowledge graph | Crawl4AI | "Parse this repo into knowledge graph" |
| [All previous entries remain...] | | |

Remember: **8 MCPs = 8x the power!** Choose wisely for maximum efficiency! 🚀