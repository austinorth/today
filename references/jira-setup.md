# Jira Board Configuration and Query Patterns

This reference provides detailed guidance for configuring and querying Jira boards for the Today skill.

## Board Configuration

### Extracting Board ID from URL

Jira board URLs follow this pattern:
```
https://{site}.atlassian.net/jira/software/projects/{PROJECT}/boards/{BOARD_ID}
```

**Example:**
```
https://example-company.atlassian.net/jira/software/projects/PROJ/boards/123
```
- Site: `example-company`
- Project: `PROJ`
- Board ID: `123`

To extract the board ID:
1. Request the full Jira board URL from the user
2. Parse the URL to extract the numeric board ID after `/boards/`
3. Store the board ID for use in queries

### Determining Cloud ID

The Atlassian MCP tools require a `cloudId` parameter. To get the cloud ID:
- Use `mcp__atlassian__getAccessibleAtlassianResources` to list accessible resources
- Extract the `id` field from the response for the matching site
- Alternatively, use the site URL directly (e.g., `https://example-company.atlassian.net`)

## Querying Assigned Tickets

### Using JQL (Jira Query Language)

To find tickets assigned to the current user on a specific board, use JQL:

**Basic query for assigned tickets:**
```jql
assignee = currentUser() AND status != Done ORDER BY priority DESC, created DESC
```

**Query for specific board:**
```jql
assignee = currentUser() AND project = {PROJECT_KEY} AND status != Done ORDER BY priority DESC, created DESC
```

**Query with status filtering:**
```jql
assignee = currentUser() AND status IN ("To Do", "In Progress") ORDER BY priority DESC, duedate ASC
```

### MCP Tool Usage

Use the `mcp__atlassian__searchJiraIssuesUsingJql` tool:

```
mcp__atlassian__searchJiraIssuesUsingJql({
  cloudId: "example-company.atlassian.net",
  jql: "assignee = currentUser() AND status != Done ORDER BY priority DESC",
  fields: ["summary", "status", "priority", "duedate", "issuetype"],
  maxResults: 50
})
```

### Field Extraction

For each ticket, extract the following information:
- **Key**: Issue key (e.g., "PROJ-123")
- **Summary**: Issue title/summary
- **Status**: Current status (e.g., "To Do", "In Progress", "In Review")
- **Priority**: Priority level (e.g., "High", "Medium", "Low")
- **Due Date**: When the ticket is due (if set)
- **Issue Type**: Type of issue (e.g., "Bug", "Story", "Task")

## Prioritization Logic

Auto-prioritize tickets using the following criteria (in order):

1. **Due Date**: Tickets due today or overdue come first
2. **Priority**: High priority before Medium before Low
3. **Status**: "In Progress" before "To Do" before "In Review"
4. **Creation Date**: Newer tickets first if all else is equal

## Example Query Flow

1. Ask user for board URL: "Which Jira board would you like to use?"
2. Extract board ID and project key from URL
3. Get cloud ID using `getAccessibleAtlassianResources`
4. Query for assigned tickets using JQL with project filter
5. Sort and prioritize based on due dates and priority
6. Return ticket list with key, summary, status for TODAY.md generation

## Handling Errors

If Jira queries fail:
- Check if the Atlassian MCP server is properly configured
- Verify the user has access to the specified board
- Confirm the board ID and project key are correct
- Provide helpful error messages to guide the user

## Alternative: Search by Board ID

Some Jira APIs allow filtering by board ID directly, but JQL with project key is more reliable and widely supported.
