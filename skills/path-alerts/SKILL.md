---
name: path-alerts
description: Get real-time NJ PATH train service alerts, delays, status updates, and line-specific information. Use when user asks about PATH trains, service disruptions, delays, specific lines (HOB-33, JSQ-33, NWK-WTC, HOB-WTC), or wants to check if PATH is running normally. Monitors official Port Authority alerts feed.
---

# NJ PATH Alerts

Get real-time service alerts and status for NJ PATH trains.

## Commands

**Check current alerts (all lines):**
```javascript
import { getAlertsMessage } from 'nj-path-alerts';
const message = await getAlertsMessage();
```

**Check alerts for specific line:**
```javascript
import { getAlertsMessage } from 'nj-path-alerts';
const message = await getAlertsMessage('HOB-33');
// Also works: 'JSQ-33', 'NWK-WTC', 'HOB-WTC'
```

**Get service status summary:**
```javascript
import { getStatusMessage } from 'nj-path-alerts';
const status = await getStatusMessage();
// Returns: ✅ Good Service or ⚠️ Delays for each line
```

**Get raw alerts data:**
```javascript
import { getPathAlerts, getAlertsForLine } from 'nj-path-alerts';

const allAlerts = await getPathAlerts();
const hobokenAlerts = await getAlertsForLine('HOB-33');
```

**Check for new alerts (for monitoring):**
```javascript
import { checkForNewAlerts } from 'nj-path-alerts';
const newAlerts = await checkForNewAlerts();
// Returns only alerts not seen before
// Useful for cron jobs that poll for updates
```

## Lines Covered

- **HOB-33**: Hoboken to 33rd Street via Hoboken
- **JSQ-33**: Journal Square to 33rd Street
- **NWK-WTC**: Newark to World Trade Center
- **HOB-WTC**: Hoboken to World Trade Center

## Stations

Newark, Harrison, Journal Square, Grove Street, Exchange Place, Newport, Hoboken, Christopher Street, 9th Street, 14th Street, 23rd Street, 33rd Street, World Trade Center

## Data Source

Uses official Port Authority API:
`https://www.panynj.gov/bin/portauthority/everbridge/incidents`

- No API key required
- Updates every 2-5 minutes during incidents
- Official source used by PATH website and apps

## Example Usage

**User:** "Are there any PATH delays?"
→ `getAlertsMessage()`

**User:** "Is the Hoboken PATH running?"
→ `getAlertsMessage('HOB-33')` or `getAlertsMessage('HOB-WTC')`

**User:** "PATH status?"
→ `getStatusMessage()`

**User:** "Any problems on the Newark line?"
→ `getAlertsMessage('NWK-WTC')`

**Monitoring setup:** Create a cron job that calls `checkForNewAlerts()` every 5 minutes, and sends notification when new alerts appear.
