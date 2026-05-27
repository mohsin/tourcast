# Tourcast

A Claude Code routine that monitors major international artists for tour dates in India, Southeast Asia, the Middle East, and East Asia — and emails you only when something new is found.

## How it works

1. Every Monday, a remote Claude agent runs `routine_prompt.md`
2. It searches music news sources for tour announcements in the target regions
3. It reads `artist_log.json` from this repo to skip already-reported artists
4. If it finds anything new (or new dates for a known artist), it updates the log and sends an email summary
5. If nothing is new, it stops silently

## Setup

### 1. Fork or clone this repo

```bash
git clone https://github.com/YOUR_USERNAME/tourcast.git
cd tourcast
```

### 2. Get a GitHub Personal Access Token

Generate one at https://github.com/settings/tokens/new with `repo` scope (read + write). You'll embed this in the routine prompt — it never touches this repo.

### 3. Connect Gmail to Claude Code

Go to https://claude.ai/customize/connectors and connect your Gmail account so the routine can send email.

### 4. Create the routine in Claude Code

Open `routine_prompt.md`, replace `YOUR_GITHUB_PAT` and `YOUR_GITHUB_USERNAME` with your values, then paste the full prompt into a new Claude Code routine at https://claude.ai/code/routines.

Suggested schedule: `30 3 * * 1` (Mondays at 9am IST / 3:30am UTC)

Allowed tools: `WebSearch`, `WebFetch`, `Bash`

### 5. Run it

Trigger a manual run from the routines page to verify everything works before the first scheduled run.

## Files

| File | Purpose |
|------|---------|
| `artist_log.json` | Tracks all found artists and their reported dates. Updated by the routine each run. |
| `routine_prompt.md` | The agent prompt. Copy this into your Claude Code routine with your credentials filled in. |

## artist_log.json schema

```json
{
  "last_checked": "2026-05-28",
  "artists": [
    {
      "name": "Artist Name",
      "first_seen": "2026-05-28",
      "last_updated": "2026-05-28",
      "tour_name": "World Tour 2026",
      "entries": [
        {
          "city": "Mumbai",
          "country": "India",
          "venue": "DY Patil Stadium",
          "date": "October 2026",
          "source_url": "https://..."
        }
      ],
      "notified": true
    }
  ]
}
```

## Target regions

- India (any city)
- Southeast Asia: Singapore, Bangkok, Jakarta, Kuala Lumpur, Manila, Ho Chi Minh City, Hanoi
- Middle East: Dubai, Abu Dhabi, Doha, Riyadh, Muscat
- East Asia: Tokyo, Seoul, Hong Kong, Taipei, Beijing, Shanghai
