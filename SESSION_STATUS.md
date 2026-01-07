# TangoVoice Session Status
**Last Updated**: 2026-01-06 21:50 EST
**Agent**: Cronas (TangoVoice owner)

## CURRENT STATUS: GPT Workaround Complete

### Completed
1. GPT is working! Tested successfully with practicas (6 events returned)
2. Found bug: Expired recurring events appear in winter queries
3. Added GPT workaround to filter out events with startDate > 2 months in past
4. Committed and pushed fix to TangoVoice repo
5. Sent bug report to Fulton for backend fix (agent-messages/inbox/fulton/msg_20260106_214500_cronas_001.json)

### Next Steps
1. Update GPT in ChatGPT Builder with new instructions from `gpt-config/system-instructions.txt`
2. Test with "What milongas are tonight?" - should NOT show "Tango Al Fresco"

## BACKEND NOTE (From Fulton)
- Recurring events behavior is BY DESIGN - frontend calculates occurrences
- Long-term fix: Data cleanup to mark expired recurring events as isActive=false
- Workaround approved by Fulton

## GPT STATUS
- Schema: ✅ Updated and working (under 300 char limits)
- Instructions: ✅ Recurring event filter workaround added
- API: ✅ v1.13.17 live, category filter working

## TESTING
```bash
# This returns expired events (bug):
curl -s 'https://calendarbeaf-prod.azurewebsites.net/api/events?appId=1&start=2026-01-06&end=2026-01-06&categoryId=66c4d370a87a956db06c49e9&active=true'

# After GPT update, ask: "What milongas are tonight?"
# Should NOT show "Tango Al Fresco" (summer event)
```
