# TangoVoice GPT - Build Guide

## What is TangoVoice?

A Custom GPT that helps find Argentine tango events by querying the TangoTiempo calendar API via natural language.

**Example queries:**
- "What milongas are this weekend?"
- "Is Tango Spark happening tonight?"
- "What practicas are this week?"
- "Are there any festivals next month?"

---

## Files in This Folder

| File | Purpose | Where to Use |
|------|---------|--------------|
| `system-instructions.txt` | GPT personality & rules | GPT Builder → Instructions field |
| `openapi-spec.yaml` | API schema for Actions | GPT Builder → Actions → Schema |
| `api-reference.md` | API documentation (for your reference) | N/A (reference only) |
| `conversation-starters.txt` | Example prompts | GPT Builder → Conversation starters |

---

## Step-by-Step: Creating the GPT

### Step 1: Open GPT Builder

1. Go to https://chat.openai.com
2. Click your profile icon (bottom left)
3. Click **My GPTs**
4. Click **Create a GPT**

### Step 2: Configure Basic Info

In the **Create** tab:
- **Name**: `TangoVoice`
- **Description** (under 300 chars): `Find Argentine tango events - milongas, practicas, classes, festivals. Ask what's tonight, this week, or by venue.`
- **Instructions**: Copy/paste entire contents of `system-instructions.txt`

### Step 3: Add the API Action

1. Click **Configure** tab
2. Scroll to **Actions**
3. Click **Create new action**
4. Click **Import from URL** or paste directly
5. Copy/paste entire contents of `openapi-spec.yaml` into the Schema field
6. **Authentication**: Select **None** (our API is public)
7. **Privacy policy**: Leave blank or use `https://tangotiempo.com/privacy`

### Step 4: Add Conversation Starters

In the **Configure** tab, add these conversation starters:
- What tango events are happening tonight?
- Is there a milonga this weekend?
- Find me practicas this week
- What's happening at [venue name]?

### Step 5: Test the GPT

1. Click **Preview** (right side)
2. Try: "What milongas are this weekend?"
3. Verify it calls the API and returns real events
4. If errors, check the Action schema

### Step 6: Save & Publish

1. Click **Save** (top right)
2. Choose visibility:
   - **Only me** - Private testing
   - **Anyone with a link** - Share with friends
   - **Public** - Listed in GPT store
3. Click **Confirm**

---

## Troubleshooting

### "Action failed" or "Could not connect"
- Check the server URL: `https://calendar-be-af.azurewebsites.net/api`
- Make sure Authentication is set to "None"

### No events returned
- The API might have no events for that date range
- Try a broader search: "What events are this month?"

### Wrong dates
- The GPT calculates dates based on current date
- If testing, specify exact dates: "What events are on January 10th?"

---

## Updating the GPT

To update after changes:
1. Go to My GPTs → TangoVoice → Edit
2. Make changes
3. Save

---

**Created by**: Fulton (AI-GUILD Agent)
**Project**: TangoVoice
**Last Updated**: 2026-01-05
