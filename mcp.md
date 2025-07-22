# MCP (Model Context Protocol) Power User Guide

## Table of Contents
1. [Overview](#overview)
2. [Installed MCP Servers](#installed-mcp-servers)
3. [Server Capabilities & Tools](#server-capabilities--tools)
4. [Development Workflows](#development-workflows)
5. [Power User Tips](#power-user-tips)
6. [Command Reference](#command-reference)
7. [Troubleshooting](#troubleshooting)
8. [GitHub Integration with Claude Code](#github-integration-with-claude-code)

## Overview

MCP servers extend Claude's capabilities by providing specialized tools for different domains. Think of them as plugins that give Claude superpowers for specific tasks. You have 5 powerful MCP servers installed that transform Claude from a chatbot into a full development environment.

### Token Economics & Efficiency

**Understanding Token Costs:**
- Each MCP tool call has a token cost ranging from minimal (100-500) to substantial (5,000-20,000)
- Context window usage accumulates quickly with file reads and multi-tool workflows
- Smart tool selection can reduce token usage by 50-80%

**Efficiency Guidelines:**
| Operation | Efficient Choice | Token Cost | Inefficient Choice | Token Cost |
|-----------|-----------------|------------|-------------------|------------|
| Read docs | Context7 | ~2-5k | Zen research | ~10-20k |
| Simple file read | Desktop Commander | ~500 | Task + multiple tools | ~5-10k |
| Code search | Desktop Commander ripgrep | ~1k | Reading all files | ~20k+ |
| Quick fix | Direct edit | ~1k | Zen refactor full analysis | ~15k |
| Task planning | Task Master | ~2k | Manual tracking | ~5k+ |

**Token-Saving Strategies:**
1. **Search before read**: Use ripgrep to find specific content instead of reading entire files
2. **Batch operations**: Combine multiple file operations in single tool calls
3. **Choose the right depth**: Use Zen's "minimal" or "low" thinking modes for simple tasks
4. **Reuse contexts**: Keep REPLs alive, use continuation IDs, maintain default repos
5. **Targeted queries**: Request specific documentation sections, not entire libraries

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

### 1. 🧘 Zen MCP
**Purpose**: Advanced AI reasoning and analysis tools  
**Adoption**: 4,912 stars (High activity)  
**Best for**: Complex problem-solving, code reviews, debugging, documentation generation  
**Efficiency note**: High token usage (5-20k per call). Use for complex tasks that truly need AI reasoning. For simple operations, consider direct implementation.

### 2. 📚 Context7
**Purpose**: Real-time documentation lookup for any library/framework  
**Adoption**: 21,221 stars (Most popular)  
**Best for**: Finding up-to-date docs, code examples, API references  
**Efficiency note**: Moderate tokens (2-5k). Faster than web search or Zen research for pure documentation needs.

### 3. 💻 Desktop Commander
**Purpose**: File system operations and process management  
**Best for**: File manipulation, running commands, data analysis with Python/Node REPL  
**Efficiency note**: Minimal tokens (~500). Most efficient for file operations. Prefer over Bash for safety, but Bash offers more flexibility.

### 4. 📋 Task Master
**Purpose**: AI-powered project and task management  
**Adoption**: 19,121 stars (Extremely popular)  
**Best for**: Breaking down complex projects, tracking progress, generating tasks from PRDs  
**Efficiency note**: Moderate tokens (2-3k). Excellent for maintaining context across long projects. Saves tokens by avoiding repeated context.

### 5. 🧠 Sequential Thinking
**Purpose**: Step-by-step problem decomposition  
**Best for**: Complex algorithms, multi-step solutions, planning  
**Efficiency note**: Variable tokens (1-5k per step). Use for genuinely complex problems requiring iterative refinement.

## Server Capabilities & Tools

## Efficiency Decision Tree

**"What tool should I use?"**

```
Need documentation?
├─ YES → Context7 (fastest, most efficient)
└─ NO → Continue...

Working with files?
├─ Simple read/write → Desktop Commander
├─ Complex search → Desktop Commander (ripgrep)
└─ Multiple files → Desktop Commander (batch operations)

Need AI analysis?
├─ Complex debugging → Zen debug (worth the tokens)
├─ Code review → Zen codereview (comprehensive)
├─ Quick question → Direct implementation (save tokens)
└─ Research → Context7 first, then Zen if needed

Managing tasks?
├─ New project → Task Master (great ROI)
├─ Quick todo → Your memory (save tokens)
└─ Complex dependencies → Task Master

GitHub operations?
├─ Use git commands via Bash tool
├─ Use gh CLI if installed
└─ See GitHub Integration section below
```

### 🧘 Zen MCP - Advanced AI Tools

**Key Tools:**
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
- `research` - AI-powered research with web search

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

### 💻 Desktop Commander - System Operations

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

### 📋 Task Master - Project Management

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

## Development Workflows

### 🚀 Full-Stack Development Workflow

1. **Project Setup**
   ```
   "Initialize a new React project with Task Master"
   → Task Master creates project structure
   → Parses requirements into tasks
   → Sets up git integration
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

5. **Git & GitHub**
   ```
   "Create a PR for the auth feature"
   → Use git commands via Bash
   → Create detailed commit messages
   → Push to GitHub with proper authentication
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

## Power User Tips

### 🎯 Efficiency Maximizers

1. **Batch Operations**
   - Use Desktop Commander's multi-file read
   - Task Master's expand_all for bulk task creation
   - Batch git operations

2. **Smart Search**
   - Desktop Commander's ripgrep > basic search
   - Use regex patterns for complex searches
   - Context7 for library-specific searches

3. **REPL Mastery**
   ```python
   # Start Python REPL for data work
   "Start a Python session and load pandas"
   
   # Keep session alive for multiple operations
   # Reuse loaded data and libraries
   ```

4. **Task Management**
   - Use tags for different project contexts
   - Set up dependencies for automatic flow
   - Let complexity analysis guide task breakdown

### 🔥 Advanced Techniques

1. **Multi-Tool Combinations**
   ```
   Research (Zen) → Plan (Sequential) → Implement (Desktop) → Test (Zen) → Deploy (Git/Bash)
   ```

2. **Context Preservation**
   - Use continuation_id in Zen tools for multi-turn conversations
   - Keep Task Master tags for different workflows
   - Maintain git branches for different features

3. **Performance Optimization**
   - Chunk file writes (25-30 lines)
   - Use file offsets for large files
   - Batch git commits when appropriate

4. **Security Best Practices**
   - Regular Zen secaudit runs
   - Pre-commit validation on sensitive code
   - Never commit secrets (use environment variables)

### 🔄 Tool Overlaps & Smart Choices

**Common Overlaps:**

1. **Documentation Search**
   - Context7 vs Zen research: Use Context7 for pure docs (80% faster, 75% fewer tokens)
   - Only use Zen when you need analysis WITH the docs

2. **File Operations**
   - Desktop Commander vs Bash: DC is safer with permissions, Bash more flexible
   - Desktop Commander vs direct Edit: DC for exploration, Edit for known changes

3. **Code Analysis**
   - Zen analyze vs manual review: Zen for architectural decisions, manual for quick checks
   - Zen codereview vs quick scan: Reserve Zen for PRs and critical code

4. **Planning**
   - Task Master vs Sequential Thinking: TM for project management, ST for algorithm design
   - Both vs manual: Use tools for 3+ step processes

5. **GitHub Operations**
   - Git via Bash for version control
   - gh CLI for GitHub-specific features
   - Direct API calls for automation

### 💡 Productivity Hacks

1. **Aliases & Shortcuts**
   ```bash
   # Add to your shell config
   alias zm="claude chat 'Using zen tools, '"
   alias td="claude chat 'Using task master, '"
   ```

2. **Template Commands**
   ```
   "Using zen debug, investigate [issue]"
   "Using desktop commander, analyze [file]"
   "Create git commit for [feature]"
   ```

3. **Workflow Automation**
   - Create Task Master tags for repeated workflows
   - Use GitHub workflows for CI/CD
   - Combine tools for complex operations

## Command Reference

### Essential Commands

```bash
# MCP Management
claude mcp list                    # List all servers
claude mcp add <server>           # Add new server
claude mcp remove <server>        # Remove server
claude mcp logs <server>          # View server logs

# Quick Tool Access
"Use zen to [action]"             # Trigger Zen tools
"Search docs for [library]"       # Context7 lookup
"Analyze file [path]"             # Desktop Commander
"Create task: [description]"      # Task Master
"Think through [problem]"         # Sequential thinking

# Power Combos
"Debug and fix [issue]"           # Zen debug + fix
"Review and refactor [file]"      # Review + improve
"Plan and implement [feature]"    # Full workflow
```

### Tool Patterns

```
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

# Git Pattern
"[Git operation] for [purpose]"
Example: "Create commit for authentication feature"
```

## Troubleshooting

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

2. **Permission Denied (Desktop Commander)**
   - Check allowed directories in settings
   - Use absolute paths
   - Verify file permissions

3. **GitHub Authentication Issues**
   - Use personal access tokens with git
   - Check token permissions (repo, workflow, etc.)
   - Store tokens securely (never in code)

4. **Task Master Not Finding Project**
   - Always provide projectRoot parameter
   - Initialize project first
   - Check .taskmaster directory exists

5. **Zen Tools Timeout**
   - Use appropriate thinking_mode
   - Break complex problems into steps
   - Check API keys for enabled models

### Performance Tips

1. **Reduce Context Usage**
   - Use Task tool for file searches (saves 50-80% tokens)
   - Batch related operations (single tool call vs multiple)
   - Clear completed todos regularly
   - Prefer Desktop Commander search over reading multiple files

2. **Optimize File Operations**
   - Read specific line ranges for large files (offset/limit parameters)
   - Always search before reading entire files (ripgrep first)
   - Chunk writes for better performance (25-30 lines per write)
   - Use `get_file_info` to check size before reading

3. **API Rate Limits & Token Costs**
   - Context7: ~2-5k tokens per query (very efficient)
   - Zen: 5-20k tokens depending on model and thinking_mode
   - Desktop Commander: ~100-500 tokens (most efficient)

4. **Smart Tool Selection by Token Cost**
   ```
   Minimal (<1k tokens): Desktop Commander, Git commands
   Low (1-5k tokens): Context7, Task Master, simple Zen ops
   Medium (5-10k tokens): Zen with "low" thinking, Sequential Thinking
   High (10-20k tokens): Zen with "high/max" thinking, complex research
   ```

## GitHub Integration with Claude Code

### Overview
Claude Code doesn't have a built-in GitHub MCP server that works reliably, but it has excellent git integration through the Bash tool. Here's how to effectively work with GitHub:

### Setting Up GitHub Access

1. **Personal Access Token (PAT)**
   ```bash
   # Create a PAT on GitHub with repo permissions
   # Use it in git commands:
   git clone https://<TOKEN>@github.com/username/repo.git
   ```

2. **SSH Key (Recommended for regular use)**
   ```bash
   # Generate SSH key
   ssh-keygen -t ed25519 -C "your_email@example.com"
   
   # Add to GitHub account
   # Then clone with:
   git clone git@github.com:username/repo.git
   ```

### Common GitHub Operations with Claude Code

1. **Cloning Repositories**
   ```
   "Clone the repository https://github.com/user/repo"
   → Claude uses Bash to run git clone
   ```

2. **Creating Commits**
   ```
   "Create a commit for the authentication feature"
   → Claude stages files with git add
   → Creates meaningful commit message
   → Commits with proper format
   ```

3. **Pushing to GitHub**
   ```bash
   # With token
   git push https://<TOKEN>@github.com/username/repo.git main
   
   # With SSH (if configured)
   git push origin main
   ```

4. **Creating Pull Requests**
   ```bash
   # If gh CLI is installed:
   gh pr create --title "Add authentication" --body "Description"
   
   # Otherwise, push branch and create PR via web
   ```

5. **Working with Issues**
   ```bash
   # With gh CLI:
   gh issue create --title "Bug: Login fails"
   gh issue list
   gh issue view 123
   ```

### Best Practices for GitHub with Claude Code

1. **Security**
   - Never hardcode tokens in files
   - Use environment variables when possible
   - Consider using SSH keys for regular work
   - Tokens should have minimal required permissions

2. **Workflow Tips**
   - Let Claude handle git operations via Bash
   - Use meaningful branch names
   - Claude can create detailed commit messages
   - Review changes before pushing

3. **Common Patterns**
   ```
   # Initialize git in new project
   "Initialize git repository and create initial commit"
   
   # Feature development
   "Create a new branch for user-profile feature"
   "Commit changes with descriptive message"
   "Push branch to GitHub"
   
   # Maintenance
   "Show git status and recent commits"
   "Pull latest changes from main branch"
   ```

4. **Troubleshooting GitHub Operations**
   - Authentication errors: Check token permissions
   - Push rejected: Pull latest changes first
   - Large files: Use .gitignore or Git LFS
   - Merge conflicts: Claude can help resolve them

### Alternative: GitHub API via curl

For automation without gh CLI:
```bash
# Create issue via API
curl -X POST \
  -H "Authorization: token <TOKEN>" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/owner/repo/issues \
  -d '{"title":"Issue title","body":"Issue description"}'

# Upload file via API
curl -X PUT \
  -H "Authorization: token <TOKEN>" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/owner/repo/contents/path/file.txt \
  -d '{"message":"Add file","content":"base64-encoded-content"}'
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

## Quick Reference Card

| Task              | Tool                | Example Command                        |
| ----------------- | ------------------- | -------------------------------------- |
| Debug code        | Zen debug           | "Debug why the API returns 500 errors" |
| Find docs         | Context7            | "Show me Express.js middleware docs"   |
| Edit files        | Desktop Commander   | "Add error handling to /src/api.js"    |
| Manage tasks      | Task Master         | "Create tasks from the PRD document"   |
| Complex planning  | Sequential Thinking | "Plan the migration to microservices"  |
| GitHub operations | Bash + git          | "Create a commit and push to GitHub"   |
| Code review       | Zen codereview      | "Review the authentication module"     |
| Generate tests    | Zen testgen         | "Create tests for the User model"      |
| Security audit    | Zen secaudit        | "Audit the payment processing code"    |
| Refactor code     | Zen refactor        | "Refactor the legacy validation logic" |

Remember: Combine tools for maximum power! 🚀