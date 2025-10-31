# 🔗 Beads & Beads MCP Documentation

This documentation covers the Beads integration for the hve-ado-scaffold project, enabling dependency-aware issue tracking and AI-assisted development workflows for data science and Azure DevOps contexts.

## What is Beads?

Beads (`bd`) is a lightweight, git-versioned issue tracker designed for AI-supervised coding workflows. Issues link together like beads on a string through four dependency types (blocks, related, parent-child, discovered-from), making it easy for AI agents to follow complex task streams over long horizons.

> **Note:** Beads is in active development (currently v0.9.x alpha). The core features work well, but expect API changes before 1.0. Use with discretion for development and internal projects. The JSONL format ensures data portability for future migrations.
>
> **Solo Workflows Only:** Beads currently has critical bugs with multi-repository and multi-clone scenarios. Use beads only in single-repository workspaces until version 1.0.0.

Key features:

* 🎯 **Dependency-aware**: Four dependency types keep work properly sequenced
* 🤖 **Agent-friendly**: JSON output and ready work detection designed for AI
* 📦 **Git-versioned**: JSONL records sync across machines automatically
* ⚡ **Zero setup**: `bd init` creates a project-local database instantly
* 🔍 **Ready work detection**: Automatically finds unblocked issues

Learn more at [steveyegge/beads](https://github.com/steveyegge/beads).

## Why Use Beads in HVE-ADO-Scaffold?

Beads provides an **optional alternative** to the `task-planner` chatmode and `.copilot-tracking/plans` Markdown files for tracking GitHub Copilot implementation work:

* **Structured tracking** instead of Markdown files
* **Dependency graphs** instead of linear task lists
* **Automatic discovery** of new work during implementation
* **MCP server integration** for GitHub Copilot agent mode
* **Coexists with ADO workflows** - use beads for local development iteration, sync to ADO for team visibility

**When to use Beads vs Markdown Planning:**

| Use Beads When...                                  | Use Markdown Plans When...                        |
|----------------------------------------------------|---------------------------------------------------|
| Working solo on complex multi-step implementation  | Collaborating with team members                   |
| Need dependency tracking between tasks             | Simple linear workflows                           |
| Want AI agent to discover/track work automatically | Manual planning and control preferred             |
| Local development with frequent iterations         | Formal documentation required                     |
| Comfortable with alpha software and CLI tools      | Prefer stable, proven workflows                   |

**Beads and Azure DevOps:**

Beads complements rather than replaces ADO workflows:

* **Beads**: Local, fast, agent-optimized, git-versioned, solo workflow
* **ADO**: Team visibility, process compliance, reporting, stakeholder communication
* **Recommended workflow**: Use beads for rapid local development cycles, then sync important milestones to ADO for team coordination

## What's in This Documentation?

This guide covers:

* 🚀 [Getting Started](#getting-started) - Install bd CLI and configure MCP
* 🎯 [Using bd Commands](#-using-bd-commands) - Essential CLI commands
* 🔄 [Typical Beads Workflow](#-typical-beads-workflow) - Complete development cycle
* 📚 [Reference](#-reference) - Chatmodes, prompts, and additional resources

## Getting Started

**Recommended:** Use the [Beads DevContainer](#-quick-start-use-the-beads-devcontainer-recommended) for the fastest setup.

**Alternative:** Follow the [Manual Installation](#-alternative-manual-installation) steps if you prefer not to use devcontainers.

### 🚀 Quick Start: Use the Beads DevContainer (Recommended)

The fastest way to get started with beads is to use the pre-configured Beads devcontainer that includes:

* Beads CLI (`bd`) pre-installed
* Beads MCP server pre-configured
* All required dependencies (uv, Go toolchain)
* Beads-specific chat settings (instructions, prompts, chatmodes)
* All HVE ADO Scaffold features (PowerShell, Azure CLI, Python, .NET, etc.)

#### Launch the Beads DevContainer

1. Open the Command Palette (`Cmd+Shift+P` on macOS, `Ctrl+Shift+P` on Windows/Linux)
2. Run **Dev Containers: Reopen in Container**
3. Select **hve-ado-scaffold-beads** from the list
4. Wait for the container to build and start

#### What You Get

When the devcontainer starts:

* ✅ Beads CLI available: `bd --version`
* ✅ MCP server automatically configured in agent mode
* ✅ Beads chatmodes, instructions, and prompts ready to use
* ✅ All GitHub Copilot settings configured for beads workflow
* ✅ All HVE ADO Scaffold tools and configurations

#### Verify Setup

1. Open a terminal in VS Code and run:

   ```bash
   bd --version
   ```

2. Open Chat view and switch to **Agent** mode
3. Click the **Tools** button
4. Look for beads tools (`beads_create`, `beads_ready`, `beads_show`, etc.)
5. Trust the beads MCP server when prompted

You're ready to use beads! Skip to [🎯 Using bd Commands](#-using-bd-commands).

---

### 📦 Alternative: Manual Installation

If you prefer not to use the devcontainer, you can install beads manually.

#### Install Beads CLI

Quick install:

```bash
curl -fsSL https://raw.githubusercontent.com/steveyegge/beads/main/install.sh | bash
```

The installer places `bd` in `~/.local/bin/bd` and adds it to your PATH.

#### Install uv Package Manager

The `uv` package manager is required to run the beads MCP server:

```bash
pip install uv
```

Alternative installation methods:

| Method             | Command           | Notes                    |
|--------------------|-------------------|--------------------------|
| pipx (recommended) | `pipx install uv` | Isolated installation    |
| pip (global)       | `pip install uv`  | System-wide installation |
| brew (macOS)       | `brew install uv` | Via Homebrew             |

#### Configure MCP Server

Add the beads MCP server to your workspace configuration:

1. Add the beads server configuration to `.vscode/mcp.json` (see JSON example below)
2. Restart VS Code or reload the window
3. Trust the beads MCP server when prompted

**Example MCP configuration:**

#### Initialize Beads in Your Repository

```bash
cd /path/to/your/repository
bd init
```

This creates `.beads/` directory with the issue database.

## 🎯 Using bd Commands

While you can use the `bd` CLI directly, we recommend using **💬 GitHub Copilot Chat** with the beads MCP server for a more intuitive experience. Copilot can help you understand bead status, create issues with proper formatting, and maintain dependencies.

### Essential Commands Quick Reference

| Command      | Purpose                 | Example                                       |
|--------------|-------------------------|-----------------------------------------------|
| `bd init`    | Initialize beads        | `bd init` or `bd init --prefix hve`           |
| `bd create`  | Create a new issue      | `bd create "Add data validation" -p 1 -t feature` |
| `bd list`    | List all issues         | `bd list` or `bd list --status open`          |
| `bd show`    | Show issue details      | `bd show hve-1`                               |
| `bd update`  | Update issue            | `bd update hve-1 --status in_progress`        |
| `bd close`   | Complete an issue       | `bd close hve-1 --reason "Implemented"`       |
| `bd ready`   | Find unblocked work     | `bd ready` or `bd ready --limit 5`            |
| `bd dep add` | Add dependency          | `bd dep add hve-2 hve-1`                      |
| `bd stats`   | Show project statistics | `bd stats`                                    |

### Common Usage Patterns

Initialize a new project:

```bash
bd init --prefix hve
# Issues will be named: hve-1, hve-2, etc.
```

Create different issue types:

```bash
bd create "Add data preprocessing pipeline" -p 1 -t feature
bd create "Fix null value handling" -p 0 -t bug
bd create "Document data schema" -p 2 -t task
```

Filter and search:

```bash
bd list --status open --priority 1
bd list --type bug
bd ready --priority 0  # Find highest priority unblocked work
```

Work with dependencies:

```bash
# Make hve-2 block hve-1 (hve-1 depends on hve-2)
bd dep add hve-2 hve-1 --type blocks

# Add related work
bd dep add hve-5 hve-3 --type related

# Create parent-child relationship
bd dep add hve-10 hve-11 --type parent-child
```

Update issue status and fields:

```bash
# Claim work
bd update hve-5 --status in_progress --assignee yourname

# Change priority
bd update hve-7 --priority 0

# Add notes
bd update hve-3 --notes "Waiting for data source access"
```

Get JSON output for scripting:

```bash
bd list --json
bd show hve-1 --json
bd ready --json
```

### 💬 Using GitHub Copilot Chat (Recommended)

Instead of remembering CLI syntax, use Copilot Chat in agent mode:

* "List all open beads"
* "Create a feature for data validation with high priority"
* "Show me hve-5 details"
* "Mark hve-3 as in progress"
* "Find ready work to do next"

Copilot will use the beads MCP server to execute commands and provide friendly, formatted responses.

## 🔄 Typical Beads Workflow

This workflow guides you from initial research through implementation and commit, using beads to track everything along the way.

### Phase 1: 📝 Research & Discovery

Use the `task-researcher` chatmode to build comprehensive research before creating beads.

1. Switch to `task-researcher` mode in Copilot Chat
2. Provide a research prompt with specific context

Example Prompt:

```text
Research data validation patterns for Streamlit dashboards using microsoft-docs
and github_repo tools. Focus on pandas validation, error handling, and user
feedback patterns. Think hard and build a concise research document.
```

1. Review the generated research document
2. Save or attach the research for the next phase

### Phase 2: 🎯 Planning with bd-task-planner

Convert research into actionable beads with proper dependencies.

1. `/clear` your Copilot Chat context to start fresh
2. Switch to `bd-task-planner` chatmode
3. Invoke the planner with your research

Example:

```text
/bd-planner-plan
```

Then attach or reference your research document from Phase 1.

1. Review the proposed epic/feature/task/bug structure
2. Let Copilot create the beads with proper:
   * Descriptions and design notes
   * Acceptance criteria
   * Dependencies and priorities
   * Labels for filtering (e.g., `python`, `data-science`, `streamlit`)
3. Make adjustments by chatting with the planner:
   * "Update hve-5 to add dependency on hve-3"
   * "Change priority of hve-7 to 0"
   * "Add more detail to hve-4 acceptance criteria"

### Phase 3: ⚡ Implementation Loop

Work through beads systematically using agent mode.

#### Start a Bead

1. `/clear` the Copilot Chat context
2. Switch to **Agent** mode
3. Start work on the next ready bead

Without specific bead:

```text
/bd-start
```

With specific bead ID:

```text
/bd-start bead=hve-5
```

Agent mode will:

* Fetch the bead details
* Review related beads and dependencies
* Read relevant instruction files (`.github/instructions/*.instructions.md`)
* Implement all required changes
* Update or create new beads for discoveries
* Complete the bead with summary

#### Review and Commit

1. Review all changes made by the agent
2. Test and validate the implementation
3. Work with the agent for adjustments if needed
4. Commit using the suggested commit message

Suggested workflow:

```bash
# Agent provides a commit message following conventions
git add .
git commit -m "feat(data): add validation pipeline

- Implement pandas schema validation
- Add user-friendly error messages
- Update Streamlit dashboard UI

🔗 hve-5"
git push
```

#### Continue Next Bead

1. Go back to step 1 (clear context)
2. Start the next bead with `/bd-start`

### Phase 4: 🔍 Track Progress

Check your progress at any time:

```bash
bd ready              # See unblocked work
bd list --status open # All open issues
bd stats              # Project statistics
```

Or ask Copilot:

* "Show me all open beads"
* "What's ready to work on next?"
* "Show statistics for this project"

### Tips for Success

✅ Always `/clear` context between beads to avoid confusion

✅ Let agent mode discover and create new beads as work progresses

✅ Review agent changes before committing

✅ Use descriptive bead titles and detailed acceptance criteria

✅ Keep dependencies updated as you learn more

✅ Commit after each completed bead

✅ Sync important beads to ADO for team visibility

❌ Don't skip the research phase for complex work

❌ Don't manually edit beads when agent mode can do it

❌ Don't ignore new discoveries—create beads for them

❌ Don't commit without reviewing agent changes

❌ Don't use beads in multi-repository workspaces (alpha limitation)

## 📚 Reference

### Project Files

| File                                                                    | Purpose                                                     |
|-------------------------------------------------------------------------|-------------------------------------------------------------|
| [bd-task-planner.chatmode.md](chatmodes/bd-task-planner.chatmode.md)   | Chatmode for planning work and creating beads from research |
| [bd-implementation.instructions.md](instructions/bd-implementation.instructions.md) | Instructions for agent mode implementation using beads |
| [bd-planner-plan.prompt.md](prompts/bd-planner-plan.prompt.md)         | Prompt template for bead planning workflow                  |
| [bd-start.prompt.md](prompts/bd-start.prompt.md)                       | Prompt template for starting bead implementation            |

### External Resources

| Resource                                                                                          | Description                                   |
|---------------------------------------------------------------------------------------------------|-----------------------------------------------|
| [Beads Repository](https://github.com/steveyegge/beads)                                           | Official beads source code and documentation  |
| [Beads README](https://github.com/steveyegge/beads/blob/main/README.md)                           | Comprehensive beads documentation             |
| [Beads Workflow Guide](https://github.com/steveyegge/beads/blob/main/WORKFLOW.md)                 | Detailed workflow patterns and best practices |
| [VS Code MCP Documentation](https://code.visualstudio.com/docs/copilot/customization/mcp-servers) | Official VS Code MCP server documentation     |
| [Model Context Protocol](https://modelcontextprotocol.io/)                                        | MCP specification and documentation           |
| [beads-mcp PyPI](https://pypi.org/project/beads-mcp/)                                             | Python package for beads MCP server           |

### Beads and Azure DevOps Integration

While beads and ADO can coexist, they serve different purposes:

**Use Beads for:**

* Rapid local development iterations
* AI-assisted task discovery and dependency tracking
* Solo developer workflows
* Detailed implementation planning with agent mode

**Use ADO for:**

* Team collaboration and visibility
* Process compliance and governance
* Sprint planning and reporting
* Stakeholder communication

**Optional ADO Sync Workflow:**

1. Complete beads locally during development
2. At milestones, create ADO work items from closed beads
3. Link ADO items using bead dependency information
4. Update bead notes with ADO work item IDs for traceability

### Getting Help

* Review existing beads: `bd list` or ask Copilot "show me all beads"
* Check bead details: `bd show <id>` or ask Copilot "show details for hve-5"
* Find examples: Browse chatmodes and prompts in this directory
* Ask Copilot: Use agent mode for interactive help with beads

🎉 **Ready to start?** Launch the Beads DevContainer and initialize beads:

```bash
bd init --prefix hve
```

Then use `/bd-planner-plan` in GitHub Copilot Chat to create your first set of beads!
