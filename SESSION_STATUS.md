# TangoVoice Session Status
**Last Updated**: 2026-01-06 19:30 EST
**Agent**: Cronas (TangoVoice owner)

## CURRENT TASK: Fix Recurring Events Bug Workaround

### What Happened
1. GPT is working! Tested successfully with practicas (6 events returned)
2. BUT found bug: Expired recurring events (like "Tango Al Fresco" summer milonga) appear in winter queries
3. Root cause is in calendar-be-af backend - Fulton's domain
4. Sent bug report to Fulton (inbox/fulton/msg_20260106_213000_cronas_002.json)

### What Needs To Be Done NOW
**Add GPT workaround** in `gpt-config/system-instructions.txt`:

Add this section after "## WHAT YOU DO NOT DO":

```
### IMPORTANT: Filter Out Expired Recurring Events
The API may return recurring events with old startDates (e.g., summer events in winter).
When displaying results:
1. Check each event's `startDate` field
2. If `startDate` is more than 2 months in the PAST, DO NOT show that event
3. This filters out expired seasonal events until the backend is fixed

Example: If today is January 2026 and an event has startDate "2025-08-22", skip it.
```

Then:
1. Commit and push
2. Update GPT in ChatGPT Builder with new instructions
3. Test again with "What milongas are tonight?"

## FILES TO EDIT
- `gpt-config/system-instructions.txt` - Add the workaround section

## BACKEND BUG (For Fulton - DO NOT FIX HERE)
- Events.js lines 196-204 return ALL recurring events without date filtering
- Expired recurring events still have `isActive: true`
- Bug report sent to Fulton

## GPT STATUS
- Schema: ✅ Updated and working (under 300 char limits)
- Instructions: ⚠️ Needs recurring event filter workaround
- API: ✅ v1.13.17 live, category filter working

## TESTING
```bash
# This returns expired events (bug):
curl -s 'https://calendarbeaf-prod.azurewebsites.net/api/events?appId=1&start=2026-01-06&end=2026-01-06&categoryId=66c4d370a87a956db06c49e9&active=true'

# After GPT update, ask: "What milongas are tonight?"
# Should NOT show "Tango Al Fresco" (summer event)
```
