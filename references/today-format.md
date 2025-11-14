# TODAY.md File Format

This reference defines the structure and formatting guidelines for the TODAY.md file.

## File Structure

The TODAY.md file consists of four main sections:

1. **Header** - Date, work hours, summary
2. **Timeline** - Chronological schedule with meetings and work blocks
3. **Backlog** - Unscheduled tickets
4. **Footer** - Notes and reflection

## Header Section

The header provides quick context about the day:

```markdown
# Today's Plan - [Day of Week], [Month] [Day], [Year]

**Work Hours:** [Start Time] - [End Time]
**Jira Board:** [Board Name/Project]
**Summary:** [X] meetings • [Y] tickets • [Z] work blocks

---
```

**Example:**
```markdown
# Today's Plan - Monday, January 15, 2024

**Work Hours:** 9:00 AM - 5:00 PM
**Jira Board:** PROJ (Board 123)
**Summary:** 4 meetings • 8 tickets • 6 work blocks

---
```

## Timeline Section

The timeline is the main body, showing a chronological view of the day:

### Format Rules

1. **Use consistent time format:** 12-hour format with AM/PM (e.g., "9:00 AM")
2. **Visual indicators:** Use emojis or tags to distinguish item types
3. **Hierarchical structure:** Use headers and bullet points for clarity
4. **Include duration:** Show time blocks clearly

### Meeting Format

```markdown
### 🗓️ [Start Time] - [End Time]: [Meeting Title]

**Duration:** [X] minutes
**Location:** [Meeting link or location]
**With:** [Organizer/Key attendees]
```

**Example:**
```markdown
### 🗓️ 9:00 AM - 10:00 AM: Team Standup

**Duration:** 60 minutes
**Location:** https://zoom.us/j/123456789
**With:** Engineering Team
```

### Work Block Format

```markdown
### 💼 [Start Time] - [End Time]: Work Block

**Suggested Tickets:**
- **[[TICKET-KEY]](ticket-url)** [Summary] `Status` `Priority`
  - Due: [Date] (if applicable)
  - Estimated: [X] minutes
```

**Example:**
```markdown
### 💼 10:00 AM - 11:00 AM: Work Block

**Suggested Tickets:**
- **[PROJ-123](https://example-company.atlassian.net/browse/PROJ-123)** Fix authentication bug `In Progress` `High`
  - Due: Today
  - Estimated: 60 minutes
- **[PROJ-145](https://example-company.atlassian.net/browse/PROJ-145)** Review pull request #456 `To Do` `Medium`
  - Estimated: 30 minutes
```

### Break/Lunch Format

```markdown
### 🍽️ [Start Time] - [End Time]: Lunch Break

_Recommended break time_
```

**Example:**
```markdown
### 🍽️ 12:00 PM - 1:00 PM: Lunch Break

_Recommended break time_
```

### Complete Timeline Example

```markdown
## Timeline

### 🗓️ 9:00 AM - 10:00 AM: Team Standup

**Duration:** 60 minutes
**Location:** https://zoom.us/j/123456789
**With:** Engineering Team

---

### 💼 10:00 AM - 11:30 AM: Work Block

**Suggested Tickets:**
- **[PROJ-123](https://example-company.atlassian.net/browse/PROJ-123)** Fix authentication bug `In Progress` `High`
  - Due: Today
  - Estimated: 60 minutes
- **[PROJ-145](https://example-company.atlassian.net/browse/PROJ-145)** Review pull request #456 `To Do` `Medium`
  - Estimated: 30 minutes

---

### 🗓️ 11:30 AM - 12:00 PM: 1:1 with Manager

**Duration:** 30 minutes
**Location:** Conference Room B
**With:** Sarah Johnson

---

### 🍽️ 12:00 PM - 1:00 PM: Lunch Break

_Recommended break time_

---

### 🗓️ 1:00 PM - 2:00 PM: Project Review Meeting

**Duration:** 60 minutes
**Location:** https://teams.microsoft.com/l/meetup-join/...
**With:** Product Team

---

### 💼 2:00 PM - 5:00 PM: Deep Work Block

**Suggested Tickets:**
- **[PROJ-167](https://example-company.atlassian.net/browse/PROJ-167)** Implement new dashboard feature `To Do` `High`
  - Due: This week
  - Estimated: 120 minutes
- **[PROJ-189](https://example-company.atlassian.net/browse/PROJ-189)** Update API documentation `To Do` `Medium`
  - Estimated: 60 minutes

---
```

## Backlog Section

List tickets that didn't fit into the schedule:

```markdown
## 📋 Backlog (Unscheduled Tickets)

These tickets are assigned to you but didn't fit in today's schedule:

- **[TICKET-KEY](url)** Summary `Status` `Priority`
  - Due: [Date]
- **[TICKET-KEY](url)** Summary `Status` `Priority`
```

**Example:**
```markdown
## 📋 Backlog (Unscheduled Tickets)

These tickets are assigned to you but didn't fit in today's schedule:

- **[PROJ-201](https://example-company.atlassian.net/browse/PROJ-201)** Fix unit tests `To Do` `Low`
  - Due: Next week
- **[PROJ-234](https://example-company.atlassian.net/browse/PROJ-234)** Research new framework `To Do` `Medium`
- **[PROJ-256](https://example-company.atlassian.net/browse/PROJ-256)** Code review for feature X `In Review` `Medium`
```

## Footer Section

End with notes and reflection prompts:

```markdown
---

## 📝 Notes

- Add your own notes here
- Track progress throughout the day
- Adjust schedule as needed

## 🎯 End of Day Reflection

_At the end of the day, consider:_
- What did I accomplish?
- What didn't get done and why?
- What should I prioritize tomorrow?

---

_Generated on [Date] at [Time]_
_To regenerate: Ask Claude to "update TODAY.md"_
_To change configuration: Ask Claude to "change my Jira board" or "update my work hours"_
```

## Formatting Guidelines

### Visual Hierarchy

Use markdown hierarchy to create scannable structure:
- `#` for main title
- `##` for major sections (Timeline, Backlog, Notes)
- `###` for time blocks and events
- `**bold**` for emphasis on ticket keys and important info
- `` `inline code` `` for status and priority labels

### Color Coding (via Emojis)

Use emojis consistently to create visual patterns:
- 🗓️ Meetings/Calendar events
- 💼 Work blocks
- 🍽️ Breaks/Lunch
- 📋 Lists/Backlogs
- 📝 Notes
- 🎯 Goals/Reflection
- ⚠️ High priority/Urgent
- ✅ Completed items (if updating throughout day)

### Links

Always link ticket keys to their Jira URL:
- Format: `[TICKET-KEY](full-jira-url)`
- Makes it easy to click through to ticket details
- Example: `[PROJ-123](https://example-company.atlassian.net/browse/PROJ-123)`

### Status and Priority Labels

Use inline code blocks for labels to make them stand out:
- `` `To Do` `` `` `In Progress` `` `` `In Review` ``
- `` `High` `` `` `Medium` `` `` `Low` ``

### Time Formatting

Be consistent with time formatting:
- Use 12-hour format with AM/PM
- Include leading zero for single-digit hours: "9:00 AM" not "9 AM"
- Use consistent separator: "-" for ranges (e.g., "9:00 AM - 10:00 AM")

## Dynamic Content

The template should be populated with dynamic content:

- `[Day of Week], [Month] [Day], [Year]` → Actual date
- `[Start Time] - [End Time]` → Configured work hours
- `[Board Name/Project]` → User's configured Jira board
- `[X] meetings` → Actual count from calendar
- `[Y] tickets` → Actual count from Jira query
- `[Z] work blocks` → Calculated based on gaps

## Adaptations

### No Meetings Scenario

If there are no meetings:
```markdown
## Timeline

### 💼 9:00 AM - 12:00 PM: Morning Deep Work

**Suggested Tickets:**
[List prioritized tickets]

---

### 🍽️ 12:00 PM - 1:00 PM: Lunch Break

---

### 💼 1:00 PM - 5:00 PM: Afternoon Deep Work

**Suggested Tickets:**
[Continue list of tickets]
```

### No Tickets Scenario

If there are no assigned tickets:
```markdown
## Timeline

[Show meetings as scheduled]

---

## 📋 No Tickets Assigned

You currently have no tickets assigned on the configured board.

**Suggestions:**
- Check other boards for assigned work
- Review and pick up unassigned tickets
- Focus on code reviews or documentation
- Use the time for learning or improvement projects
```

### Jira-Only Mode (No Calendar)

If Outlook is not configured:
```markdown
# Today's Plan - Monday, January 15, 2024

**Work Hours:** 9:00 AM - 5:00 PM
**Jira Board:** PROJ (Board 123)
**Note:** Calendar integration not configured. Showing ticket priorities only.

---

## 🎯 Prioritized Tickets

### High Priority
[List high priority tickets]

### Medium Priority
[List medium priority tickets]

### Low Priority
[List low priority tickets]

---

_To enable calendar integration, ask Claude about setting up Outlook MCP server._
```

## File Location

Always write to the user's home directory:
- **Path:** `~/TODAY.md` or `/Users/[username]/TODAY.md`
- **Overwrite:** Replace existing file each time
- **Backup:** Users can manually save previous versions if needed
