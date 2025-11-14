# Outlook/Microsoft 365 MCP Server Setup

This reference provides guidance for setting up and using the Outlook/Microsoft 365 MCP server to access calendar events.

## MCP Server Setup

### Finding an Outlook MCP Server

The Outlook/Microsoft 365 MCP server may not be installed by default. Common options include:

**Option 1: Search for existing MCP servers**
- GitHub: `microsoft-365-mcp-server`, `outlook-mcp-server`, `msgraph-mcp-server`
- npm registry: Search for MCP servers with Microsoft Graph API integration

**Option 2: Use Microsoft Graph API MCP Server**
If available, use an MCP server that integrates with Microsoft Graph API, which provides:
- Calendar access (`/me/calendar/events`)
- Email access (`/me/messages`)
- Contact access (`/me/contacts`)

**Option 3: Build Custom MCP Server**
If no suitable server exists, guide the user to create one using the mcp-builder skill with:
- Python or TypeScript implementation
- Microsoft Graph API integration
- OAuth 2.0 authentication flow

### Authentication Setup

Microsoft 365 requires OAuth 2.0 authentication:

1. **Register Application in Azure Portal**
   - Go to https://portal.azure.com
   - Navigate to "App registrations"
   - Create new registration
   - Note the Application (client) ID

2. **Configure API Permissions**
   - Add Microsoft Graph API permissions:
     - `Calendars.Read` (for reading calendar events)
     - `Calendars.ReadWrite` (if creating events)
   - Grant admin consent if required

3. **Create Client Secret**
   - In "Certificates & secrets", create new client secret
   - Save the secret value securely

4. **Configure MCP Server**
   - Add credentials to MCP server configuration
   - Typical location: `~/.config/claude/mcp-config.json` or similar
   - Include client ID, client secret, and tenant ID

### MCP Configuration Example

```json
{
  "mcpServers": {
    "microsoft365": {
      "command": "npx",
      "args": ["-y", "microsoft-365-mcp-server"],
      "env": {
        "MS_CLIENT_ID": "your-client-id",
        "MS_CLIENT_SECRET": "your-client-secret",
        "MS_TENANT_ID": "your-tenant-id"
      }
    }
  }
}
```

## Querying Calendar Events

### Expected MCP Tool Names

Look for tools with patterns like:
- `mcp__outlook__get_calendar_events`
- `mcp__msgraph__list_calendar_events`
- `mcp__microsoft365__get_events`

### Query Today's Meetings

To fetch calendar events for today within work hours:

**Parameters typically required:**
- Start time: Beginning of work hours (e.g., 9:00 AM today)
- End time: End of work hours (e.g., 5:00 PM today)
- Time zone: User's local time zone (e.g., "America/Los_Angeles")

**Example API call pattern:**
```
{
  startDateTime: "2024-01-15T09:00:00",
  endDateTime: "2024-01-15T17:00:00",
  timeZone: "America/Los_Angeles"
}
```

### Field Extraction

For each calendar event, extract:
- **Subject**: Meeting title
- **Start Time**: When the meeting starts
- **End Time**: When the meeting ends
- **Location**: Meeting location (if physical) or meeting link (if virtual)
- **Organizer**: Who organized the meeting
- **Attendees**: List of attendees (optional)
- **Is All Day**: Boolean indicating if it's an all-day event

## Time Zone Handling

Always use the user's local time zone for queries and display:
- Query Outlook API with explicit time zone parameter
- Convert returned times to user's local time if needed
- Display times in 12-hour format (e.g., "9:00 AM") for readability

## Handling Setup Errors

If Outlook MCP server is not configured:

1. **Detect missing tools**: Check if Outlook/Microsoft 365 tools are available
2. **Provide setup instructions**: Direct user to this reference file
3. **Graceful degradation**: Offer to generate TODAY.md with just Jira tickets
4. **Test connection**: After setup, verify calendar access works

### Error Messages to Watch For

- "Authentication failed": Check credentials and OAuth configuration
- "Insufficient permissions": Verify API permissions in Azure Portal
- "Tool not found": MCP server not installed or not running
- "Rate limited": Too many API calls, implement backoff strategy

## Example Query Flow

1. Check if Outlook MCP tools are available
2. If not available, provide setup instructions and offer Jira-only mode
3. If available, get today's date and work hours configuration
4. Query calendar for events between start and end of work hours
5. Extract meeting details (subject, start time, end time)
6. Identify gaps between meetings for work block suggestions
7. Return meeting list for TODAY.md generation

## Alternative: Manual Entry

If Outlook integration cannot be set up, offer to let the user manually input their meetings for the day, or focus solely on Jira ticket organization without calendar integration.
