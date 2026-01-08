# TangoVoice Session Status
**Last Updated**: 2026-01-07 07:45 EST
**Agent**: Cronas (TangoVoice owner)

## CURRENT STATUS: GPT Ready - Voice NOT Supported

### Critical Discovery
**ChatGPT Voice mode does NOT support custom GPTs.**
- Voice mode ignores custom GPT instructions and Actions
- Falls back to base ChatGPT 4o
- This is an OpenAI limitation, not fixable on our end
- "TangoVoice" name is misleading

### Rename Suggestions
| Current | Suggested | Why |
|---------|-----------|-----|
| TangoVoice | **TangoTiempo Assistant** | Matches website name |
| TangoVoice | **TangoTiempo GPT** | Clear it's a GPT |
| TangoVoice | **Boston Tango Finder** | Describes function |
| TangoVoice | **Tango Event Finder** | Generic, expandable |
| TangoVoice | **TT Assistant** | Short, matches TT brand |

### What's Working (Text Mode Only)
- ✅ Voice API endpoint at `/api/voice/events`
- ✅ RRULE expansion - recurring events show correct dates
- ✅ Timezone fix - dates match recurrence patterns
- ✅ Category names correct (Milonga, Practica, etc.)
- ✅ venueTimezone field (deploying)
- ✅ GPT instructions (3427 chars, under 8000 limit)

### Voice API Fields
| Field | Description |
|-------|-------------|
| title | Event name |
| category | Milonga/Practica/Class/etc. |
| dateFormatted | "Tuesday, January 6" |
| timeFormatted | "7:00 PM" |
| venueName | Venue name |
| venueCity | City |
| venueTimezone | "America/New_York" |
| isRecurring | true/false |
| isCanceled | true/false |

### GPT Files
```
gpt-config/openapi-spec.yaml       - Actions schema
gpt-config/system-instructions.txt - Instructions
```

### GPT Key Rules
1. NO web browsing - API only for events
2. Training data OK for tango terminology
3. Use EXACT API data for events
4. Times are in venue's local timezone

### ChatGPT Builder Settings
- ☐ Web Browsing (DISABLED)
- ☐ Code Interpreter (DISABLED)
- ☑ Actions (ENABLED - TangoTiempo API)

### Voice Mode Limitation
OpenAI does not support custom GPT Actions in Voice mode.
No workaround available. Text mode only for now.

### Test Commands
```bash
# Practicas this week
curl -s 'https://calendarbeaf-prod.azurewebsites.net/api/voice/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49ea'

# All events
curl -s 'https://calendarbeaf-prod.azurewebsites.net/api/voice/events?appId=1&start=2026-01-06&end=2026-01-12&limit=20'
```
