# TangoVoice Session Status
**Last Updated**: 2026-01-07 07:20 EST
**Agent**: Cronas (TangoVoice owner)

## CURRENT STATUS: Voice API v1.15.1 Deploying

### What's Working
- ✅ Voice API endpoint live at `/api/voice/events`
- ✅ Category names correct (Milonga, Practica, Class, etc.)
- ✅ RRULE expansion - recurring events show occurrence dates
- ✅ Timezone fix - dates now match recurrence patterns
- ⏳ venueTimezone field - deploying

### Voice API Fields
| Field | Description |
|-------|-------------|
| id | Event ID |
| title | Event name |
| category | Milonga/Practica/Class/etc. |
| dateFormatted | "Tuesday, January 6" |
| timeFormatted | "7:00 PM" |
| venueName | Venue name |
| venueCity | City |
| venueAddress | Full address |
| venueTimezone | "America/New_York" (coming) |
| isRecurring | true/false |
| isCanceled | true/false |
| description | Event description |

### GPT Files Ready
```
gpt-config/openapi-spec.yaml      - Actions schema
gpt-config/system-instructions.txt - Instructions (3072 chars)
```

### Key Instructions
1. Use ONLY API data - no hallucination
2. Display dateFormatted/timeFormatted exactly as returned
3. Times are in venue's local timezone
4. Check isCanceled and warn user

### Endpoint
```
GET https://calendarbeaf-prod.azurewebsites.net/api/voice/events
  ?appId=1
  &start=YYYY-MM-DD
  &end=YYYY-MM-DD
  &categoryId=<optional>
  &limit=20
```

### Test Commands
```bash
# Practicas this week
curl -s 'https://calendarbeaf-prod.azurewebsites.net/api/voice/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49ea'

# All events this week
curl -s 'https://calendarbeaf-prod.azurewebsites.net/api/voice/events?appId=1&start=2026-01-06&end=2026-01-12&limit=20'
```
