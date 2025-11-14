---
name: today
description: Daily work planning assistant that connects to Jira and Outlook calendar to generate a time-based TODAY.md file. Use this when users request daily planning, work organization, or need to see their schedule with assigned tickets. Auto-prioritizes tasks and suggests 30-minute work blocks between meetings. Jira board and work hours (default 9am-5pm) are configurable.
---

# Today

## Overview

This skill helps plan the workday by integrating Jira tickets with Outlook calendar events to generate a time-based TODAY.md file in the user's home directory. The skill auto-prioritizes tasks, suggests work blocks between meetings, and provides a chronological view of the day.

## When to Use This Skill

Use this skill when users need to:
- Plan their workday: "plan my day", "create my daily plan", "what's on my agenda today"
- Generate TODAY.md file: "create TODAY.md", "update my daily plan"
- See schedule with tasks: "show my schedule with my Jira tickets", "what should I work on today"
- Organize work around meetings: "help me schedule work between meetings"

## Configuration

This skill requires two configurable settings:

### 1. Jira Board Configuration

On first use or when explicitly requested, ask the user which Jira board to query:
- Request the Jira board URL (e.g., https://yourcompany.atlassian.net/jira/software/projects/PROJ/boards/1234)
- Extract the board ID from the URL pattern: `/boards/{board_id}`
- Use the board ID for querying assigned tickets

### 2. Work Hours Configuration

Default work hours: **9am-5pm**

Ask the user if they want to customize their work hours. Support various formats:
- "9am-5pm" (default)
- "8am-6pm"
- "9:00-17:00"
- "8:30am-5:30pm"

Use the configured work hours to:
- Filter calendar events within work hours
- Suggest work blocks only during work hours
- Structure the TODAY.md file chronologically within work hours

## Workflow Decision Tree

When this skill is invoked, follow this decision tree:

1. **First-time use or configuration change?**
   - YES → Ask for Jira board URL and work hours preference
   - NO → Use existing configuration (or ask if not remembered)

2. **Gather today's data:**
   - Load `references/jira-setup.md` for Jira query guidance
   - Load `references/outlook-setup.md` for Outlook query guidance
   - Query Jira for assigned tickets from configured board
   - Query Outlook calendar for today's meetings within work hours

3. **Process and prioritize:**
   - Load `references/daily-plan-workflow.md` for detailed workflow steps
   - Auto-prioritize tickets based on due dates and priority labels
   - Identify gaps between meetings (minimum 30 minutes)
   - Suggest which tickets to work on during each gap

4. **Generate TODAY.md:**
   - Load `references/today-format.md` for formatting guidance
   - Use `assets/today-template.md` as the base structure
   - Create chronological time-based view
   - Write to ~/TODAY.md

## Available MCP Tools

### Atlassian/Jira Tools
- Use MCP tools prefixed with `mcp__atlassian__*` for Jira operations
- Key tools: search issues, get issue details, query boards
- See `references/jira-setup.md` for detailed tool usage

### Outlook/Microsoft 365 Tools
- Use MCP tools prefixed with `mcp__outlook__*` or similar for calendar operations
- Key tools: get calendar events, list meetings
- See `references/outlook-setup.md` for setup and tool usage
- **Note:** If Outlook MCP server is not configured, provide setup instructions from references

## Resources

This skill includes:

### references/
- `jira-setup.md` - Jira board configuration and query patterns
- `outlook-setup.md` - Outlook MCP server setup and calendar access
- `daily-plan-workflow.md` - Detailed step-by-step workflow for generating the daily plan
- `today-format.md` - TODAY.md file structure and formatting guidelines

### assets/
- `today-template.md` - Base template for the TODAY.md file with placeholders

Load reference files progressively as needed during workflow execution. Do not load all references upfront—only load what's needed for the current step.
