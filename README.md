# n8n Inbox Hub

A personal automation system that monitors multiple email accounts, detects calendar events using AI, and creates them automatically.

## Features

- **Multi-Account Gmail Monitoring**: Watches both `wanzelin007@gmail.com` and `zelin.agent@gmail.com`
- **AI Event Detection**: Uses GPT-5.2 to intelligently detect scheduled events in emails
- **Automatic Calendar Creation**: Creates Google Calendar events with proper timing and details
- **Smart Routing**: Marks emails as read using the correct account credentials

## Architecture

```
[INPUT SOURCES]              [PROCESSING]              [OUTPUTS]
     │                            │                        │
Gmail (wanzelin007) ───┐          │                        │
                       │     ┌────▼────┐            ┌──────▼──────┐
Gmail (zelin.agent) ───┼────►│ Unified │────►GPT────►│ Router      │
                       │     │ Format  │   Detect   │             │
(Future: iMessage) ────┤     └─────────┘   Events   │ ┌───────────┤
                       │                            │ │           │
(Future: Webhook) ─────┘                   Has      │ ▼           ▼
                                          Event? ───┼─► Calendar  Mark Read
                                                    │   Event     (correct acct)
                                                    │
                                           No ──────▼
                                           event    Mark Read only
```

## Workflow Nodes

1. **Gmail Trigger (wanzelin007)** - Polls every minute for new emails
2. **Gmail Trigger (zelin.agent)** - Polls every minute for new emails
3. **Unify Input** - Standardizes email format and tracks source account
4. **Detect Event (GPT-5.2)** - AI extracts scheduling information
5. **Check Has Event** - Routes based on whether event was detected
6. **Create Calendar Event** - Adds event to Google Calendar
7. **Route Account** - Directs to correct Mark Read node
8. **Mark Read** - Marks email as read in the source account

## Event Detection Examples

The AI can detect events like:
- "Let's meet tomorrow at 3pm at Starbucks"
- "Your appointment is scheduled for Feb 10 at 2:30 PM"
- "Interview scheduled for Monday, February 10th, 2026 at 10:00 AM PST"
- "The concert is on Saturday at 7pm"
- "Don't forget dentist on Tuesday morning"

## Credentials Required

1. **Gmail OAuth2 (wanzelin007)** - For wanzelin007@gmail.com
2. **Gmail OAuth2 (zelin.agent)** - For zelin.agent@gmail.com
3. **Google Calendar OAuth2** - For wanzelin007's primary calendar
4. **OpenAI API** - For GPT-5.2 event detection

## Default Settings

- **Event Duration**: 30 minutes (when end time not specified)
- **Timezone**: America/Los_Angeles (Pacific Time)
- **Poll Interval**: Every minute

## Future Roadmap

### Phase 2: Additional Input Sources
- iMessage via AppleScript/Shortcuts
- Slack/Discord native triggers
- SMS via Twilio
- Manual webhook input

### Phase 3: Additional Outputs
- Task creation (Todoist/Notion)
- Smart reminders
- Daily/weekly summary reports

### Phase 4: Advanced Features
- Calendar conflict detection
- Smart categorization (Personal vs Work)
- Natural language chat interface

## Testing

Send test emails to verify:

1. **With event**: "Meeting with John tomorrow at 2pm at Starbucks"
   - Should create calendar event with correct date/time/location

2. **Without event**: "Here's the report you requested"
   - Should not create any calendar event

## License

Private - Personal use only
