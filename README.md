# AI Context - Developer Mode Workflow System

## Overview

AI Context is a task-driven development system that initializes an AI agent directly into **Developer mode** using MCP (Model Context Protocol) servers for task management. This system enforces a structured, deterministic workflow where each task follows an isolated Product Requirements Document (PRD) process without requiring a separate project definition.

## Architecture

```
ai-context/
├── init.txt              # Initialization config - triggers developer mode
├── developer/
│   └── context.txt       # Core workflow rules and enforcement
```

## Initialization

The system is activated when `init.txt` is loaded, which:

1. Skips any mode selection step
2. Immediately initializes the AI in **Developer mode**
3. Loads and enforces rules from `developer/context.txt`

## Workflow

### Step 1: Task Source Discovery

The AI asks: **"Which MCP server should be used to fetch tasks?"**

- Examples: Notion MCP, Linear MCP, Jira MCP, etc.
- The AI must not assume or pre-configure a source

### Step 2: Fetch Tasks

After the user specifies an MCP server:

1. The AI retrieves the task list via MCP tools
2. Presents a concise list including:
   - Task title
   - Status (if available)
   - Short identifier (index or ID)

### Step 3: Task Selection

The AI asks: **"Which task do you want to work on?"**

- User selects exactly **ONE** task
- The AI **MUST NOT** proceed without explicit selection
- No multi-tasking or batch processing

### Step 4: PRD Creation

Once a task is selected, the AI generates a **Product Requirements Document (PRD)** saved to:

```
ai-context/tasks/{task_name_resume}-prd.md
```

**Filename Rules:**
- Lowercase only
- Hyphens instead of spaces
- Short summary of the task

**PRD Structure (STRICT - no deviations):**

1. **Title**
2. **Objective**
3. **Scope**
4. **User Input Summary**
5. **Technical Plan**
6. **Affected Files**
7. **Risks / Notes**

Rules:
- Keep it concise
- Do not invent extra sections
- Do not reuse previous PRDs

### Step 5: Approval Gate

The AI asks: **"Can I proceed with this PRD?"**

- **YES**: Start implementation
- **NO**: Update PRD based on feedback and ask again

### Step 6: Implementation

During implementation, the AI must:

- Follow the PRD **strictly**
- Not introduce unrelated changes
- Keep consistency with the existing repository

## Core Principles

- **Task-First, Not PRD-First**: Tasks originate from external MCP sources
- **Isolation**: Each selected task = new PRD file (never reuse or merge)
- **No Global Memory**: No shared project memory file across tasks
- **Deterministic**: Structured, predictable workflow with clear gates
- **Minimal Interaction**: Direct, clear communication with essential questions only

## Enforcement Rules

The AI **MUST** enforce:

- ❌ No MCP defined → **Ask for it**
- ❌ No task selected → **Block execution**
- ❌ User tries to skip PRD → **Enforce PRD creation**
- ❌ No approval before implementation → **Require approval**

## File Rules

The AI is **ONLY** allowed to create or modify:

- `ai-context/tasks/{task_name}-prd.md`

No system files (`init.txt`, `developer/context.txt`, etc.) should be touched unless explicitly requested.

## Behavior Guidelines

- **Direct and structured** in communication
- **Ask minimal but necessary** questions
- **Do not assume** missing information
- **Prioritize clarity** over verbosity
- Be concise — keep interaction minimal

## Task Isolation Rules (STRICT)

- Each selected task → new PRD file
- Never reuse or overwrite another task's PRD
- Never merge multiple tasks into one PRD
- Maintain complete separation between tasks

## Example Workflow

```
User: Load init.txt

AI: Which MCP server should be used to fetch tasks? (e.g., Notion MCP)

User: Notion MCP

AI: [Retrieves tasks from Notion]
Available tasks:
1. Add dark mode toggle (Status: Todo)
2. Fix navbar responsiveness (Status: In Progress)
3. Update footer links (Status: Done)

Which task do you want to work on?

User: 1

AI: [Generates PRD for "add-dark-mode-toggle-prd.md"]
Title: Add Dark Mode Toggle
Objective: ...
Scope: ...
...

Can I proceed with this PRD?

User: Yes

AI: [Begins implementation...]
```

## Getting Started

1. Load `init.txt` to initialize Developer mode
2. Specify an MCP server when prompted
3. Select a task from the retrieved list
4. Review and approve the generated PRD
5. Watch the AI implement the solution

## Purpose

This system creates a clean, scalable, task-driven workflow where:

- Tasks come from MCP (external sources)
- Each task has its own isolated PRD
- The user stays in full control of execution
- The AI remains structured, deterministic, and predictable

## License

This is an AI workflow configuration system.

---

**Version**: 1.0  
**Last Updated**: May 2026
