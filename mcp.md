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

**Repository Operations:**
- `create_repository` - Create new GitHub repository
- `fork_repository` - Fork a repository to your account
- `get_file_contents` - Get file/directory contents from repo
- `create_or_update_file` - Create or update single file
- `delete_file` - Delete file from repository
- `push_files` - Push multiple files in single commit
- `list_branches` - List repository branches
- `create_branch` - Create new branch
- `get_tag` - Get tag details
- `list_tags` - List repository tags

**Issue Management:**
- `create_issue` - Create new issue
- `update_issue` - Update existing issue
- `get_issue` - Get issue details
- `list_issues` - List repository issues
- `add_issue_comment` - Add comment to issue
- `get_issue_comments` - Get issue comments
- `search_issues` - Search for issues across GitHub

**Pull Request Management:**
- `create_pull_request` - Create new PR
- `update_pull_request` - Update PR details
- `merge_pull_request` - Merge a PR
- `get_pull_request` - Get PR details
- `list_pull_requests` - List repository PRs
- `get_pull_request_diff` - Get PR diff
- `get_pull_request_files` - Get changed files
- `update_pull_request_branch` - Update PR branch
- `search_pull_requests` - Search PRs across GitHub

**Code Review Features:**
- `create_pending_pull_request_review` - Start a review
- `add_comment_to_pending_review` - Add review comment
- `submit_pending_pull_request_review` - Submit review
- `delete_pending_pull_request_review` - Cancel review
- `get_pull_request_reviews` - Get PR reviews
- `get_pull_request_comments` - Get PR comments
- `create_and_submit_pull_request_review` - Quick review

**AI-Powered Features:**
- `create_pull_request_with_copilot` - Delegate PR creation to Copilot
- `request_copilot_review` - Request Copilot code review
- `assign_copilot_to_issue` - Assign issue to Copilot

**Workflow & Actions:**
- `list_workflows` - List repository workflows
- `run_workflow` - Trigger workflow run
- `list_workflow_runs` - List workflow runs
- `get_workflow_run` - Get run details
- `cancel_workflow_run` - Cancel running workflow
- `rerun_workflow_run` - Rerun entire workflow
- `rerun_failed_jobs` - Rerun only failed jobs
- `list_workflow_jobs` - List jobs in run
- `get_job_logs` - Get job logs (with failed_only option)
- `list_workflow_run_artifacts` - List run artifacts
- `download_workflow_run_artifact` - Download artifact
- `get_workflow_run_usage` - Get usage metrics

**Security Scanning:**
- `list_code_scanning_alerts` - List code security alerts
- `get_code_scanning_alert` - Get specific alert details
- `list_dependabot_alerts` - List dependency alerts
- `get_dependabot_alert` - Get Dependabot alert details
- `list_secret_scanning_alerts` - List secret scanning alerts
- `get_secret_scanning_alert` - Get secret alert details

**Search & Discovery:**
- `search_repositories` - Search GitHub repositories
- `search_code` - Search code across GitHub
- `search_issues` - Search issues
- `search_pull_requests` - Search PRs
- `search_users` - Search users
- `search_orgs` - Search organizations

**Notifications & User:**
- `get_me` - Get authenticated user details
- `list_notifications` - List user notifications
- `get_notification_details` - Get notification details
- `dismiss_notification` - Mark notification as read/done
- `manage_notification_subscription` - Manage subscriptions
- `mark_all_notifications_read` - Mark all as read

**Utility Operations:**
- `get_commit` - Get commit details
- `list_commits` - List commits on branch/tag
- `list_discussion_categories` - List discussion categories
- `list_discussions` - List repository discussions
- `get_discussion` - Get discussion details
- `get_discussion_comments` - Get discussion comments

**Power Features:**
- Batch file operations with `push_files`
- Copilot integration for automated PR creation
- Smart log retrieval with `failed_only` option
- Review workflow with pending reviews
- Comprehensive search across all GitHub resources

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

**Power Features:**
- Multi-model support (Gemini 2.0/2.5, custom models)
- Web search integration for current information
- Thinking modes: minimal, low, medium, high, max
- Thread continuation for multi-turn conversations
- Image analysis support

### 📚 Context7 - Documentation Lookup

**Key Tools:**
- `resolve-library-id` - Find library IDs from package names
- `get-library-docs` - Fetch comprehensive documentation

**Power Features:**
- 2000+ libraries with code examples
- Version-specific documentation
- Trust scores for library quality
- Code snippet counts

### 💻 Desktop Commander - System Operations (v0.2.6)

**Key Tools:**
- `read_file` - Read files with offset/pagination
- `write_file` - Write/append files (chunked for performance)
- `create_directory` - Create folders
- `list_directory` - List directory contents
- `move_file` - Move/rename files
- `search_files` - Find files by name pattern
- `search_code` - Ripgrep-powered code search
- `get_file_info` - File metadata and stats
- `edit_block` - Surgical text replacements
- `start_process` - Start terminal processes/REPLs
- `interact_with_process` - Send commands to running processes
- `read_process_output` - Get process output
- `list_sessions` - View active terminal sessions
- `kill_process` - Terminate processes

**Power Features:**
- Python/Node REPL for data analysis
- Smart process detection (knows when waiting for input)
- File operation safety (allowed directories only)
- Chunked file writing (25-30 lines recommended)
- Image file support (PNG, JPEG, GIF, WebP)

### 📋 Task Master - Project Management (v0.21.0)

**Key Tools:**
- `initialize_project` - Set up Task Master in project
- `parse_prd` - Generate tasks from PRD documents
- `analyze_project_complexity` - Complexity analysis
- `expand_task` - Break tasks into subtasks
- `expand_all` - Expand all pending tasks
- `get_tasks` - List all tasks with filtering
- `get_task` - Get specific task details
- `next_task` - Find next task based on dependencies
- `set_task_status` - Update task status
- `add_task` - Add new tasks with AI
- `update_task` - Update task information
- `remove_task` - Delete tasks
- `add_dependency` - Create task dependencies
- `list_tags` - View task tags/contexts
- `research` - AI-powered research with project context

**Power Features:**
- Automatic task generation from PRDs
- Dependency management and validation
- Multiple tag contexts for different workflows
- Complexity-based task expansion
- Git integration for task storage
- Multi-language support

### 🧠 Sequential Thinking - Problem Decomposition

**Key Tools:**
- `sequentialthinking` - Step-by-step problem solving

**Power Features:**
- Dynamic thought adjustment
- Revision and backtracking support
- Branch exploration for alternatives
- Hypothesis generation and verification
- Handles problems with unclear scope

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

1. **Project Setup with GitHub**
   ```
   "Create a new repository for my React project"
   → GitHub MCP creates repository
   → Task Master initializes project structure
   → Parses requirements into tasks
   ```

2. **Documentation Research**
   ```
   "Show me the latest React hooks documentation"
   → Context7 fetches current React docs
   → Provides code examples and best practices
   ```

3. **Implementation**
   ```
   "Implement the user authentication feature"
   → Desktop Commander creates files
   → Zen tools for code generation
   → Sequential thinking for complex logic
   ```

4. **Testing & Review**
   ```
   "Generate tests for the auth module"
   → Zen testgen creates comprehensive tests
   → Desktop Commander runs test suite
   → Zen codereview checks quality
   ```

5. **GitHub Integration**
   ```
   "Create a PR for the auth feature"
   → GitHub MCP creates pull request
   → Request Copilot review
   → Merge when approved
   ```

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

1. **Identify Issue**
   ```
   "Debug why the login fails intermittently"
   → Zen debug tool starts investigation
   → Desktop Commander examines logs
   → Sequential thinking traces execution
   ```

2. **Root Cause Analysis**
   ```
   → Zen tracer analyzes code flow
   → Desktop Commander searches for patterns
   → Identifies race condition in async code
   ```

3. **Fix & Verify**
   ```
   → Implements fix with proper error handling
   → Zen testgen creates regression tests
   → Zen precommit validates changes
   → GitHub MCP creates PR with fix
   ```

### 📊 Data Analysis Workflow

1. **Load Data**
   ```
   "Analyze the sales data in /data/sales.csv"
   → Desktop Commander starts Python REPL
   → Loads pandas, reads CSV
   → Performs initial exploration
   ```

2. **Analysis**
   ```
   → Interactive data manipulation
   → Statistical analysis
   → Visualization preparation
   ```

3. **Report Generation**
   ```
   → Zen docgen creates analysis report
   → Desktop Commander saves results
   → Task Master tracks completion
   ```

### 🔄 GitHub Workflow Examples

1. **Issue Management**
   ```
   "Create an issue for the performance problem"
   → GitHub MCP creates issue with details
   → Assigns to team member
   → Adds appropriate labels
   ```

2. **Pull Request Workflow**
   ```
   "Create a PR for the feature/auth branch"
   → GitHub MCP creates PR
   → Adds detailed description
   → Requests specific reviewers
   → Can delegate to Copilot for implementation
   ```

3. **CI/CD Management**
   ```
   "Run the deployment workflow on main branch"
   → GitHub MCP triggers workflow
   → Monitors run status
   → Gets logs if failures occur
   → Can rerun failed jobs only
   ```

4. **Security Scanning**
   ```
   "Check for security vulnerabilities"
   → GitHub MCP lists code scanning alerts
   → Gets Dependabot alerts
   → Reviews secret scanning results
   → Creates issues for critical findings
   ```

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

4. **Batch Operations**
   - Use GitHub MCP's push_files for multiple file updates
   - Desktop Commander's multi-file read
   - Task Master's expand_all for bulk task creation

5. **Smart Search**
   - Desktop Commander's ripgrep > basic search
   - GitHub MCP search_code for GitHub-wide searches
   - Context7 for library-specific searches

6. **REPL Mastery**
   ```python
   # Start Python REPL for data work
   "Start a Python session and load pandas"
   
   # Keep session alive for multiple operations
   # Reuse loaded data and libraries
   ```

7. **Task Management**
   - Use tags for different project contexts
   - Set up dependencies for automatic flow
   - Let complexity analysis guide task breakdown

8. **GitHub Efficiency**
   - Use pending reviews to batch PR comments
   - Delegate to Copilot for routine implementations
   - Use failed_only option for debugging CI failures

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

3. **Multi-Tool Combinations**
   ```
   GitHub (repo) → Research (Zen) → Plan (Sequential) → Implement (Desktop) → Test (Zen) → Deploy (GitHub)
   ```

4. **Context Preservation**
   - Use continuation_id in Zen tools for multi-turn conversations
   - Keep Task Master tags for different workflows
   - Maintain GitHub context with get_me for user info

5. **Performance Optimization**
   - Chunk file writes (25-30 lines)
   - Use file offsets for large files
   - Batch GitHub operations when possible
   - Use search before reading files

6. **Security Best Practices**
   - Regular Zen secaudit runs
   - GitHub security scanning integration
   - Pre-commit validation on sensitive code
   - Never commit secrets (use environment variables)

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

**Common Overlaps:**

3. **Documentation Search**
   - Context7 vs Zen research: Use Context7 for pure docs (80% faster, 75% fewer tokens)
   - Only use Zen when you need analysis WITH the docs

4. **File Operations**
   - Desktop Commander vs Bash: DC is safer with permissions, Bash more flexible
   - Desktop Commander vs direct Edit: DC for exploration, Edit for known changes

5. **Code Analysis**
   - Zen analyze vs manual review: Zen for architectural decisions, manual for quick checks
   - Zen codereview vs quick scan: Reserve Zen for PRs and critical code

6. **Planning**
   - Task Master vs Sequential Thinking: TM for project management, ST for algorithm design
   - Both vs manual: Use tools for 3+ step processes

7. **GitHub Operations**
   - GitHub MCP vs git commands: GitHub MCP for API operations, git for local ops
   - GitHub MCP vs gh CLI: GitHub MCP is integrated, gh needs separate install

### 💡 Productivity Hacks

1. **Aliases & Shortcuts**
   ```bash
   # Add to your shell config
   alias zm="claude chat 'Using zen tools, '"
   alias gh="claude chat 'Using GitHub MCP, '"
   alias td="claude chat 'Using task master, '"
   ```

2. **Template Commands**
   ```
   "Using zen debug, investigate [issue]"
   "Using GitHub MCP, create PR for [feature]"
   "Using desktop commander, analyze [file]"
   "Create task: [description]"
   "Think through [problem]"
   ```

3. **Workflow Automation**
   - Create Task Master tags for repeated workflows
   - Use GitHub workflows triggered via MCP
   - Combine tools for complex operations

## Command Reference *[UPDATED]*

### Essential Commands

```bash
# MCP Management
claude mcp list                    # List all servers
claude mcp add <server>           # Add new server
claude mcp remove <server>        # Remove server
claude mcp logs <server>          # View server logs

# Documentation Commands
"Show me [library] docs"          # Context7 for indexed
"Get GitHub docs for [repo]"      # GitMCP for any repo
"Crawl docs from [url]"           # Crawl4AI for web

# Validation Commands
"Check script for hallucinations"  # Crawl4AI validation
"Parse repo into knowledge graph"  # Build validation DB

# Quick Tool Access
"Use zen to [action]"             # Trigger Zen tools
"Use GitHub MCP to [action]"      # GitHub operations
"Search docs for [library]"       # Context7 lookup
"Analyze file [path]"             # Desktop Commander
"Create task: [description]"      # Task Master
"Think through [problem]"         # Sequential thinking

# Quick Access Patterns
"Using GitMCP, search [repo] for [topic]"
"Using Crawl4AI, get docs from [website]"
"Check if this AI code is valid"

# Power Combos
"Debug and fix [issue]"           # Zen debug + fix
"Review and refactor [file]"      # Review + improve
"Plan and implement [feature]"    # Full workflow
"Create PR with implementation"   # GitHub + Copilot
```

### Tool Patterns

```
# GitHub MCP Pattern
"Using GitHub MCP, [action] [target]"
Example: "Using GitHub MCP, create issue for login bug"
Example: "Using GitHub MCP, merge PR #42"

# Zen Pattern
"Using zen [tool], [specific request]"
Example: "Using zen thinkdeep, analyze the authentication architecture"

# Context7 Pattern
"Show me [library] docs for [topic]"
Example: "Show me React docs for useEffect cleanup"

# Desktop Commander Pattern
"[Action] the file [path]"
Example: "Search for TODO comments in /src"

# Task Master Pattern
"[Action] task: [description]"
Example: "Create task: Implement user profile page"
```

## Troubleshooting *[UPDATED]*

### Common Issues

1. **MCP Server Connection Failed**
   ```bash
   # Check logs
   claude mcp logs <server-name>
   
   # Restart Claude Code
   claude restart
   
   # Remove and re-add server
   claude mcp remove <server>
   claude mcp add <server>
   ```

2. **GitHub MCP Authentication**
   - Ensure GitHub token is configured in environment
   - Check token has required permissions (repo, workflow, etc.)
   - Use get_me to verify authentication

3. **Permission Denied (Desktop Commander)**
   - Check allowed directories in settings
   - Use absolute paths
   - Verify file permissions

4. **Task Master Not Finding Project**
   - Always provide projectRoot parameter
   - Initialize project first
   - Check .taskmaster directory exists

5. **Zen Tools Timeout**
   - Use appropriate thinking_mode
   - Break complex problems into steps
   - Check API keys for enabled models

6. **GitMCP Not Finding Repository**
   - Use full owner/repo format
   - Check if repo is public
   - Try match_common_libs first

7. **Crawl4AI Connection Issues**
   - Verify Supabase/Neo4j credentials
   - Check URL accessibility
   - Respect rate limits

### Performance Tips *[UPDATED]*

1. **Reduce Context Usage**
   - Use GitHub MCP search instead of reading many files
   - Batch related operations (single tool call vs multiple)
   - Clear completed todos regularly
   - Prefer Desktop Commander search over reading multiple files

2. **Optimize File Operations**
   - Read specific line ranges for large files (offset/limit parameters)
   - Always search before reading entire files (ripgrep first)
   - Chunk writes for better performance (25-30 lines per write)
   - Use `get_file_info` to check size before reading

3. **API Rate Limits & Token Costs**
   - GitHub MCP: ~1-3k tokens per operation (efficient)
   - Context7: ~2-5k tokens per query (very efficient)
   - Zen: 5-20k tokens depending on model and thinking_mode
   - Desktop Commander: ~100-500 tokens (most efficient)

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

### Getting Help

1. **Built-in Help**
   ```
   /help                         # Claude Code help
   claude mcp logs <server>      # Debug server issues
   ```

2. **Report Issues**
   - Claude Code: https://github.com/anthropics/claude-code/issues
   - Individual MCP servers: Check their GitHub repos

3. **Community Resources**
   - MCP Discord community
   - GitHub discussions
   - Stack Overflow (mcp tag)

---

## Quick Reference Card *[UPDATED]*

| Task | Tool | Example Command |
|------|------|----------------|
| Popular library docs | Context7 | "Show me React hooks documentation" |
| Any GitHub docs | GitMCP | "Get docs for owner/repo" |
| Web documentation | Crawl4AI | "Crawl the official docs site" |
| Validate AI code | Crawl4AI | "Check this script for hallucinations" |
| Build knowledge graph | Crawl4AI | "Parse this repo into knowledge graph" |
| Create GitHub issue | GitHub MCP | "Create issue for the login bug" |
| Create pull request | GitHub MCP | "Create PR for feature/auth branch" |
| Review PR | GitHub MCP | "Review and approve PR #42" |
| Run CI workflow | GitHub MCP | "Run deployment workflow on main" |
| Debug code | Zen debug | "Debug why the API returns 500 errors" |
| Find docs | Context7 | "Show me Express.js middleware docs" |
| Edit files | Desktop Commander | "Add error handling to /src/api.js" |
| Manage tasks | Task Master | "Create tasks from the PRD document" |
| Complex planning | Sequential Thinking | "Plan the migration to microservices" |
| Code review | Zen codereview | "Review the authentication module" |
| Generate tests | Zen testgen | "Create tests for the User model" |
| Security audit | Zen secaudit | "Audit the payment processing code" |
| Refactor code | Zen refactor | "Refactor the legacy validation logic" |
| Search GitHub | GitHub MCP | "Search for React hooks examples" |
| Manage notifications | GitHub MCP | "Show my GitHub notifications" |

Remember: **8 MCPs = 8x the power!** Choose wisely for maximum efficiency! 🚀