# Today - Daily Work Planning Skill

A Claude Code skill that integrates Jira tickets with Outlook calendar events to generate a time-based daily plan.

## Overview

The "Today" skill helps you plan your workday by:
- Fetching your assigned Jira tickets from a configurable board
- Retrieving your Outlook calendar events
- Auto-prioritizing tasks based on due dates and priority
- Suggesting 30-minute work blocks between meetings
- Generating a chronological TODAY.md file in your home directory

## Features

- **Configurable Jira Board**: Works with any Jira board URL
- **Configurable Work Hours**: Default 9am-5pm, fully customizable
- **Time-based Schedule**: Chronological view of your entire day
- **Auto-prioritization**: Smart sorting by due date, priority, and status
- **Work Block Suggestions**: Intelligently suggests tickets for gaps between meetings
- **Graceful Degradation**: Works with just Jira if Outlook isn't configured
- **Progressive Disclosure**: Loads detailed references only when needed

## Requirements

### Required
- Claude Code with skills support
- Atlassian/Jira MCP server (configured and connected)

### Optional
- Microsoft 365/Outlook MCP server (for calendar integration)

## Installation

### Option 1: Install from ZIP

1. Download `today.zip` from releases
2. Extract to your Claude skills directory
3. Restart Claude Code or reload skills

### Option 2: Install from Source

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/today.git
   cd today
   ```

2. Copy to your Claude skills directory:
   ```bash
   cp -r . ~/.claude/skills/today/
   ```

3. Restart Claude Code or reload skills

## Usage

Trigger the skill with any of these phrases:

- "plan my day"
- "create TODAY.md"
- "what's on my agenda today"
- "show my schedule with my Jira tickets"
- "help me schedule work between meetings"

### First-Time Setup

On first use, the skill will ask you to configure:

1. **Jira Board URL**: Provide your board URL (e.g., `https://yourcompany.atlassian.net/jira/software/projects/PROJ/boards/1234`)
2. **Work Hours**: Confirm or customize your work hours (default: 9am-5pm)

### Example Output

The skill generates a `~/TODAY.md` file with:

```markdown
# Today's Plan - Monday, January 15, 2024

**Work Hours:** 9:00 AM - 5:00 PM
**Jira Board:** PROJ (Board 123)
**Summary:** 4 meetings • 8 tickets • 6 work blocks

---

## Timeline

### 🗓️ 9:00 AM - 10:00 AM: Team Standup
**Duration:** 60 minutes
**Location:** https://zoom.us/j/123456789

### 💼 10:00 AM - 11:30 AM: Work Block
**Suggested Tickets:**
- **[PROJ-123]** Fix authentication bug `In Progress` `High`
- **[PROJ-145]** Review pull request #456 `To Do` `Medium`

[... more timeline entries ...]
```

## Configuration

### Jira Board

The skill extracts the board ID from your board URL:
```
https://{site}.atlassian.net/jira/software/projects/{PROJECT}/boards/{BOARD_ID}
```

To change your configured board, simply ask:
- "change my Jira board"
- "use a different board"

### Work Hours

Default work hours are 9am-5pm. Supported formats:
- "9am-5pm"
- "8:30am-5:30pm"
- "9:00-17:00"
- "8-5"

To change your work hours:
- "update my work hours"
- "change my schedule to 8am-6pm"

## Outlook/Microsoft 365 Setup

If you don't have the Outlook MCP server configured, the skill will:
1. Detect the missing integration
2. Provide setup instructions
3. Offer to create a Jira-only plan

For Outlook setup guidance, see `references/outlook-setup.md` in the skill directory.

## File Structure

```
today/
├── SKILL.md                          # Main skill definition
├── references/                       # Detailed documentation
│   ├── jira-setup.md                # Jira configuration guide
│   ├── outlook-setup.md             # Outlook MCP setup guide
│   ├── daily-plan-workflow.md       # Complete workflow steps
│   └── today-format.md              # TODAY.md formatting guide
└── assets/                          # Template files
    └── today-template.md            # Base template with placeholders
```

## How It Works

1. **Configuration**: Determines Jira board and work hours
2. **Data Collection**: Fetches Jira tickets and Outlook calendar events
3. **Processing**: Prioritizes tickets and identifies time gaps
4. **Schedule Generation**: Creates chronological timeline with work blocks
5. **File Creation**: Writes formatted TODAY.md to your home directory

For detailed workflow, see `references/daily-plan-workflow.md`.

## Prioritization Logic

Tickets are auto-prioritized using:

1. **Due Date**: Overdue → Due today → Due this week → Later
2. **Priority Label**: High → Medium → Low
3. **Status**: In Progress → To Do → In Review
4. **Creation Date**: Newer first (when all else equal)

## Troubleshooting

### Jira Issues

- Verify board URL is correct
- Check Atlassian MCP server is running
- Confirm you have access to the board

### Outlook Issues

- Check if Microsoft 365 MCP server is configured
- Verify authentication and permissions
- See `references/outlook-setup.md` for setup help

### No Meetings or Tickets

- The skill gracefully handles empty data
- Jira-only mode if no calendar integration
- Calendar-only mode if no assigned tickets

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

MIT License - feel free to use and modify as needed.

## Credits

Created using the [Claude Code Skill Creator](https://github.com/anthropics/claude-code) framework.
