# TangoVoice Session Status
**Last Updated**: 2026-01-06 18:55 EST
**Agent**: Cronas (TangoVoice owner)

## PROJECT SUMMARY

**TangoVoice** is a Custom GPT that helps users find Argentine tango events via voice/natural language queries to the TangoTiempo calendar API.

**GitHub Repo**: https://github.com/ybotman/TangoVoice

## CURRENT STATUS

### What's Done:
1. Created TangoVoice GPT configuration files in `/gpt-config/`:
   - `README.md` - How to build the GPT
   - `openapi-spec.yaml` - OpenAPI schema for GPT Actions
   - `system-instructions.txt` - GPT personality/behavior rules
   - `api-reference.md` - API documentation
   - `conversation-starters.txt` - Example prompts

2. Schema updated to use correct API params:
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

### What's Pending:
1. **Backend deployment** - Fulton (calendar-be-af agent) says v1.13.17 is deployed with:
   - Fixed date filtering (uses startDate field)
   - Fixed category filtering ($or on categoryFirstId/Second/Third)
   - Fixed venue filtering (venueID capital ID)

   BUT API health check still shows v1.11.0 - deployment not live yet.

2. **Once backend is live**:
   - Test: `GET /api/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49ea`
   - Should return practicas for this week
   - Update GPT in ChatGPT Builder with final schema

## COLLABORATION STATUS

**Agent Messages Repo**: https://github.com/ybotman/masterCalendarCollab

**Conversation with Fulton**:
- Sent bug report about Events.js field name mismatches
- Fulton confirmed fix and deployed v1.13.17
- Waiting for deployment to go live (API still shows v1.11.0)

**My Inbox**: `inbox/cronas/`
**Fulton's Inbox**: `inbox/fulton/`

## NEXT STEPS

1. Poll for new messages from Fulton
2. When API shows v1.13.17+, test category filtering
3. Once working, confirm to Fulton
4. User can then update GPT in ChatGPT Builder:
   - Paste `system-instructions.txt` in Instructions
   - Paste `openapi-spec.yaml` in Actions Schema
   - Test with "What practicas are this week?"

## IMPORTANT NOTES

- TangoVoice owns this repo ONLY - do not modify calendar-be-af
- API URL: `https://calendarbeaf-prod.azurewebsites.net/api`
- The GPT must match calendar-be API params (not the other way around)

## TESTING

```bash
# Check API version
curl -s "https://calendarbeaf-prod.azurewebsites.net/api/health" | jq '.version'

# Test events with category
curl -s "https://calendarbeaf-prod.azurewebsites.net/api/events?appId=1&start=2026-01-06&end=2026-01-12&categoryId=66c4d370a87a956db06c49ea&active=true"

# Poll agent messages
cd /Users/tobybalsley/Documents/AppDev/MasterCalendar/agent-messages
git pull origin main
ls inbox/cronas/
```
