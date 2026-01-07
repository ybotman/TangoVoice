# TangoVoice Session Status
**Last Updated**: 2026-01-06 19:00 EST
**Agent**: Cronas (TangoVoice owner)

## PROJECT SUMMARY

**TangoVoice** is a Custom GPT that helps users find Argentine tango events via voice/natural language queries to the TangoTiempo calendar API.

**GitHub Repo**: https://github.com/ybotman/TangoVoice

## CURRENT STATUS

### BACKEND v1.13.17 NOW LIVE!

Confirmed at 2026-01-06 18:57 EST:
```
GET /api/health
{"version":"1.13.17"}
```

Category filter test (Practicas):
```
GET /api/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49ea&active=true
Result: 6 events returned!
```

### What's Done:
1. Created TangoVoice GPT configuration files in `/gpt-config/`:
   - `README.md` - How to build the GPT
   - `openapi-spec.yaml` - OpenAPI schema for GPT Actions
   - `system-instructions.txt` - GPT personality/behavior rules
   - `api-reference.md` - API documentation
   - `conversation-starters.txt` - Example prompts

2. Schema uses correct API params:
   - `start`/`end` (not startDate/endDate) - matches calendar-be
   - `active` param (default "true")
   - `categoryId` with hardcoded ObjectId enums:
     - Milonga: 66c4d370a87a956db06c49e9
     - Practica: 66c4d370a87a956db06c49ea
     - Class: 66c4d370a87a956db06c49eb
     - Workshop: 66c4d370a87a956db06c49ed
     - Festival: 66c4d370a87a956db06c49ec
     - Marathon: 66c4d370a87a956db06c49ef
     - Encuentro: 68f7fcc90710b7f41bf0c2f3

3. Terminology fixed: Uses "Argentine tango" not "dance"
4. Description under 300 chars

### Backend Fixes in v1.13.17:
- Fixed date filtering (uses startDate field)
- Fixed category filtering ($or on categoryFirstId/Second/Third)
- Fixed venue filtering (venueID capital ID)
- Active param defaults to true

## READY FOR GPT UPDATE

User can now update GPT in ChatGPT Builder:
1. Paste `system-instructions.txt` in Instructions
2. Paste `openapi-spec.yaml` in Actions Schema
3. Test with "What practicas are this week?"

## COLLABORATION STATUS

**Agent Messages Repo**: https://github.com/ybotman/masterCalendarCollab

**Conversation with Fulton**: COMPLETE
- Sent bug report about Events.js field name mismatches
- Fulton fixed and deployed v1.13.17
- Confirmed working at 2026-01-06 18:57 EST

**My Inbox**: `inbox/cronas/`
**Fulton's Inbox**: `inbox/fulton/`

## TESTING

```bash
# Check API version
curl -s "https://calendarbeaf-prod.azurewebsites.net/api/health" | jq '.version'
# Expected: "1.13.17"

# Test events with category (Practicas)
curl -s "https://calendarbeaf-prod.azurewebsites.net/api/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49ea&active=true"
# Expected: 6 events

# Test events with category (Milongas)
curl -s "https://calendarbeaf-prod.azurewebsites.net/api/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49e9&active=true"
```

## IMPORTANT NOTES

- TangoVoice owns this repo ONLY - do not modify calendar-be-af
- API URL: `https://calendarbeaf-prod.azurewebsites.net/api`
- The GPT must match calendar-be API params (not the other way around)
