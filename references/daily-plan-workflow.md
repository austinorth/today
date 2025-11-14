# Daily Plan Workflow

This reference provides the detailed step-by-step workflow for generating the TODAY.md file.

## Workflow Overview

The daily planning workflow consists of five main phases:
1. Configuration
2. Data Collection
3. Processing & Prioritization
4. Schedule Generation
5. File Creation

## Phase 1: Configuration

### Step 1.1: Determine Work Hours

Ask the user for their work hours preference (if not already configured):

**Default:** 9am-5pm

**Prompt example:**
"What are your work hours for today? (default: 9am-5pm)"

**Parse various formats:**
- "9am-5pm" → 09:00 to 17:00
- "8:30am-5:30pm" → 08:30 to 17:30
- "9:00-17:00" → 09:00 to 17:00
- "8-5" → 08:00 to 17:00

Store the parsed start and end times for use in filtering and scheduling.

### Step 1.2: Determine Jira Board

Ask the user for their Jira board (if not already configured):

**Prompt example:**
"Which Jira board would you like to use? Please provide the board URL."

**Extract board information:**
- Parse the URL to extract board ID
- Extract project key if possible
- Store for querying

**Example input:**
`https://example-company.atlassian.net/jira/software/projects/PROJ/boards/123`

**Extracted:**
- Board ID: `123`
- Project: `PROJ`
- Site: `example-company.atlassian.net`

## Phase 2: Data Collection

### Step 2.1: Fetch Jira Tickets

Query Jira for tickets assigned to the current user:

**JQL Query:**
```jql
assignee = currentUser() AND status != Done ORDER BY priority DESC, duedate ASC
```

**Using MCP Tool:**
```
mcp__atlassian__searchJiraIssuesUsingJql({
  cloudId: "example-company.atlassian.net",
  jql: "assignee = currentUser() AND status != Done ORDER BY priority DESC",
  fields: ["summary", "status", "priority", "duedate", "issuetype"],
  maxResults: 50
})
```

**Extract for each ticket:**
- Issue key (e.g., "PROJ-123")
- Summary/title
- Status (e.g., "To Do", "In Progress")
- Priority (e.g., "High", "Medium", "Low")
- Due date (if set)
- Issue type (e.g., "Bug", "Story", "Task")

### Step 2.2: Fetch Calendar Events

Query Outlook for today's meetings within work hours:

**Time range:**
- Start: Today at work start time (e.g., 9:00 AM)
- End: Today at work end time (e.g., 5:00 PM)

**Expected MCP tool pattern:**
```
mcp__outlook__get_calendar_events({
  startDateTime: "2024-01-15T09:00:00",
  endDateTime: "2024-01-15T17:00:00",
  timeZone: "America/Los_Angeles"
})
```

**Extract for each meeting:**
- Subject/title
- Start time
- End time
- Location or meeting link
- Duration (calculate from start/end)

**Handle missing Outlook:**
If Outlook MCP tools are not available:
- Inform the user that calendar integration is not set up
- Offer to create a Jira-only plan
- Provide setup instructions from `outlook-setup.md`
- Continue with just ticket prioritization

## Phase 3: Processing & Prioritization

### Step 3.1: Prioritize Tickets

Sort tickets using the following criteria (in priority order):

1. **Due Date Priority:**
   - Overdue (due date in the past): Highest priority
   - Due today: High priority
   - Due this week: Medium priority
   - No due date or later: Lower priority

2. **Priority Label:**
   - High priority tickets before Medium
   - Medium priority tickets before Low
   - No priority label: treat as Medium

3. **Status Priority:**
   - "In Progress": Highest (continue existing work)
   - "To Do": Medium (new work)
   - "In Review": Lower (waiting on others)

4. **Creation Date:**
   - Among equal tickets, newer ones first

**Result:** Ordered list of tickets from highest to lowest priority

### Step 3.2: Identify Time Gaps

Analyze the calendar to find gaps between meetings:

**Algorithm:**
1. Sort meetings chronologically by start time
2. Compare each meeting's end time with the next meeting's start time
3. If gap >= 30 minutes, it's a potential work block
4. Gaps < 30 minutes are considered transition/break time

**Example:**
```
9:00 AM - 10:00 AM: Team Standup
10:00 AM - 11:30 AM: [GAP: 90 minutes - 3 work blocks possible]
11:30 AM - 12:00 PM: 1:1 with Manager
12:00 PM - 1:00 PM: [GAP: 60 minutes - lunch/2 work blocks]
1:00 PM - 2:00 PM: Project Review
2:00 PM - 5:00 PM: [GAP: 180 minutes - 6 work blocks possible]
```

### Step 3.3: Assign Tickets to Time Blocks

Match prioritized tickets to available time gaps:

**Strategy:**
- Estimate 30 minutes per ticket for basic work
- High-priority tickets get assigned to earlier gaps
- Complex tickets may need multiple 30-minute blocks
- Don't over-schedule—leave buffer time

**Example assignment:**
```
10:00 AM - 10:30 AM: [PROJ-123] High priority bug fix
10:30 AM - 11:00 AM: [PROJ-145] Review pull request
11:00 AM - 11:30 AM: [PROJ-167] Update documentation
```

## Phase 4: Schedule Generation

### Step 4.1: Create Chronological Timeline

Merge meetings and work blocks into a single chronological timeline:

**Timeline structure (work hours: 9am-5pm):**
```
9:00 AM - 10:00 AM: [MEETING] Team Standup
10:00 AM - 10:30 AM: [WORK] PROJ-123 - High priority bug fix
10:30 AM - 11:00 AM: [WORK] PROJ-145 - Review pull request
11:00 AM - 11:30 AM: [WORK] PROJ-167 - Update documentation
11:30 AM - 12:00 PM: [MEETING] 1:1 with Manager
12:00 PM - 1:00 PM: [LUNCH/BREAK]
1:00 PM - 2:00 PM: [MEETING] Project Review
2:00 PM - 2:30 PM: [WORK] PROJ-189 - Implement new feature
2:30 PM - 3:00 PM: [WORK] PROJ-201 - Fix unit tests
3:00 PM - 5:00 PM: [WORK] PROJ-234 - Code review and testing
```

### Step 4.2: Add Context and Details

For each item in the timeline, include relevant details:

**For meetings:**
- Subject/title
- Duration
- Location or meeting link (if available)

**For work blocks:**
- Ticket key and summary
- Status and priority
- Due date (if urgent)
- Estimated time blocks needed

## Phase 5: File Creation

### Step 5.1: Format the Output

Use the template from `assets/today-template.md` and populate with:

**Header section:**
- Today's date
- Work hours (e.g., "9:00 AM - 5:00 PM")
- Jira board reference
- Quick summary (X meetings, Y tickets)

**Body section:**
- Chronological timeline from Phase 4
- Clear separation between meetings and work blocks
- Visual indicators for different types of items

**Footer section:**
- Unscheduled tickets (lower priority items that didn't fit)
- Notes or reminders
- End-of-day reflection prompt

### Step 5.2: Write to File

Write the formatted content to `~/TODAY.md`:

**Location:** `/Users/austin/TODAY.md` (user's home directory)

**Overwrite behavior:** Replace existing TODAY.md if it exists

**Success message:**
"✅ TODAY.md has been created at ~/TODAY.md"

**Include instructions:**
- How to regenerate the file
- How to change configuration (board, work hours)
- How to manually adjust the plan

## Error Handling

### Common Issues:

**1. Jira query fails:**
- Verify board URL and credentials
- Check network connection
- Fall back to asking user to manually list tickets

**2. Outlook query fails:**
- Check if MCP server is configured
- Provide setup instructions
- Offer Jira-only mode

**3. No meetings found:**
- Confirm with user if this is expected
- Generate plan with just work blocks for tickets
- Suggest full day for deep work

**4. No tickets found:**
- Confirm user has assigned tickets on the board
- Offer to search across all boards
- Generate plan with just calendar events

**5. Parsing errors:**
- Gracefully handle malformed data
- Provide sensible defaults
- Inform user of assumptions made

## Workflow Completion

After successful creation of TODAY.md:

1. Display success message with file location
2. Offer to open the file or show preview
3. Ask if user wants to adjust any configuration
4. Provide instructions for regenerating later
5. Remind user they can manually edit the file

**Example completion message:**
```
✅ Your daily plan has been created!

📋 TODAY.md is ready at: ~/TODAY.md

Summary:
• 4 meetings scheduled
• 8 Jira tickets prioritized
• 6 work blocks suggested

You can regenerate this plan anytime by asking me to "update TODAY.md"
```
