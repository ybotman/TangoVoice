# TangoVoice API Reference

This document provides detailed information about the TangoTiempo Calendar API that powers TangoVoice.

## Base URL

```
https://calendarbeaf-prod.azurewebsites.net/api
```

## Authentication

**None required** for read operations. The API is public for GET requests.

---

## Application IDs (appId)

The API supports multiple calendar applications. TangoVoice uses:

| appId | Application | Description |
|-------|-------------|-------------|
| `"1"` | TangoTiempo | Boston/New England tango calendar (DEFAULT) |
| `"2"` | HarmonyJunction | Other dance styles |

**Always use appId="1"** for TangoVoice queries unless user specifies otherwise.

---

## Event Categories

Categories define the type of dance event:

| Category Name | Code | Description |
|---------------|------|-------------|
| Milonga | MIL | Social tango dance - the main event type dancers look for |
| Practica | PRA | Practice session - less formal, for working on technique |
| Class | CLS | Structured lesson with instructor |
| Workshop | WRK | Intensive multi-hour or multi-day learning session |
| Festival | FES | Multi-day event with classes, milongas, performances |
| Special Event | SPE | One-off events (performances, shows, anniversaries) |

**Note**: Category IDs are MongoDB ObjectIds. Query `/api/categories` to get current IDs.

---

## Location Hierarchy

Events and venues are organized by location:

```
Country → Region → Division → City
```

### Boston Area Structure

| Level | Example | Field Name |
|-------|---------|------------|
| Region | Northeast US | masteredRegionId, masteredRegionName |
| Division | Massachusetts | masteredDivisionId, masteredDivisionName |
| City | Boston, Cambridge, Somerville | masteredCityId, masteredCityName |

### Common Boston-Area Cities

- Boston
- Cambridge
- Somerville
- Brookline
- Newton
- Arlington
- Jamaica Plain

---

## Endpoints

### GET /events

Search and list events.

**Query Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| startDate | string (YYYY-MM-DD) | Filter events starting from this date |
| endDate | string (YYYY-MM-DD) | Filter events before this date |
| categoryId | string | Filter by category ObjectId |
| venueId | string | Filter by venue ObjectId |
| organizerId | string | Filter by organizer ObjectId |
| masteredRegionId | string | Filter by region |
| masteredCityId | string | Filter by city |
| search | string | Text search in title/description |
| isActive | boolean | Active events only (default: true) |
| appId | string | Application ID (default: "1") |

**Example Requests:**

```
# Events this weekend
GET /events?startDate=2026-01-10&endDate=2026-01-12&appId=1

# Search for specific event
GET /events?search=Tango%20Spark&startDate=2026-01-05&endDate=2026-01-05

# All milongas (need categoryId from /categories first)
GET /events?categoryId=<milonga-id>&startDate=2026-01-01&endDate=2026-01-31
```

**Response Fields:**

```json
{
  "events": [
    {
      "_id": "abc123",
      "title": "Friday Night Milonga",
      "description": "Weekly social dance...",
      "startTime": "2026-01-10T01:00:00.000Z",
      "endTime": "2026-01-10T05:00:00.000Z",
      "venueStartDisplay": "8:00 PM",
      "venueEndDisplay": "12:00 AM",
      "venueTZ": "America/New_York",
      "venueName": "Dance Studio Boston",
      "venueAddress": "123 Main St, Cambridge, MA",
      "categoryName": "Milonga",
      "ownerOrganizerName": "Boston Tango Society",
      "isCanceled": false,
      "isActive": true,
      "isRepeating": true,
      "masteredCityName": "Cambridge",
      "appId": "1"
    }
  ],
  "total": 1
}
```

### GET /events/{eventId}

Get single event details.

### GET /venues

List venues.

**Query Parameters:**
- search: Text search in venue name
- masteredCityId: Filter by city
- isActive: Active venues only

### GET /venues/{id}

Get venue details including full address and timezone.

### GET /organizers

List event organizers.

**Query Parameters:**
- search: Text search in organizer name
- masteredRegionId: Filter by region

### GET /categories

List all event categories with their ObjectIds.

**Response:**
```json
[
  { "_id": "...", "categoryName": "Milonga", "categoryCode": "MIL" },
  { "_id": "...", "categoryName": "Practica", "categoryCode": "PRA" },
  { "_id": "...", "categoryName": "Class", "categoryCode": "CLS" },
  { "_id": "...", "categoryName": "Workshop", "categoryCode": "WRK" },
  { "_id": "...", "categoryName": "Festival", "categoryCode": "FES" }
]
```

---

## Important Fields

### Time Fields

| Field | Description | Use For |
|-------|-------------|---------|
| startTime | UTC ISO timestamp | Calculations, filtering |
| endTime | UTC ISO timestamp | Calculations, filtering |
| venueStartDisplay | Local time string (e.g., "8:00 PM") | Display to user |
| venueEndDisplay | Local time string | Display to user |
| venueTZ | IANA timezone (e.g., "America/New_York") | Reference |

**Always use venueStartDisplay/venueEndDisplay for user-facing times!**

### Status Fields

| Field | Description | Action |
|-------|-------------|--------|
| isCanceled | Event is canceled | ALWAYS check - warn user if true |
| isActive | Event is active | Filter out inactive events |
| isFeatured | Promoted event | Can highlight in results |

### Recurring Events

| Field | Description |
|-------|-------------|
| isRepeating | True if recurring event |
| recurrenceRule | RFC 5545 RRULE string |

The API returns expanded occurrences, so each instance has its own startTime.

---

## Error Handling

| Status | Meaning |
|--------|---------|
| 200 | Success |
| 400 | Bad request (invalid parameters) |
| 404 | Resource not found |
| 500 | Server error |

Empty results return 200 with empty array - not an error.

---

## Rate Limits

No explicit rate limits, but be reasonable. The GPT's normal usage patterns are fine.

---

## Testing the API

Quick test:
```bash
curl "https://calendar-be-af.azurewebsites.net/api/health"
curl "https://calendar-be-af.azurewebsites.net/api/categories"
curl "https://calendar-be-af.azurewebsites.net/api/events?startDate=2026-01-05&endDate=2026-01-12"
```

---

**Last Updated**: 2026-01-05
**Maintained By**: Fulton (AI-GUILD Agent)
