# Assisted Living Monitor — User Guide

**Version 1.0 · March 2026**

---

## What It Does

The Assisted Living Monitor watches for unexpected inactivity in the home. It learns the resident's normal daily patterns over 7 days, then raises an alert if no movement is detected during a time when the model predicts someone should be present.

You interact with the system in two ways:
- **The Monitor web UI** — to view status, adjust settings, and manage Vacation Mode
- **Node-RED** — to receive and act on MQTT alerts

---

## The Web UI

### Detection Settings

These read-only values are visible in the **Settings** panel:

| Setting | Default | Meaning |
|---|---|---|
| Forecast threshold | 0.70 | Minimum confidence that someone should be present |
| Silence threshold | 30 min | Inactivity duration before an anomaly is raised |
| Sensor health | 2 h | Extended absence that triggers an EMERGENCY alert |
| Alert throttle | 60 min | Minimum gap between repeated alerts for the same room |
| MQTT topic | `aragon/assistedliving/anomaly` | Where alerts are published |

### Event History

The **Events** panel shows a timestamped log of all anomaly alerts, including room, severity, and duration of silence.

### Vacation Mode

Enable Vacation Mode whenever the resident is away from home to prevent false alerts.

**To enable:**
1. Open the **Assisted Living** section in the web UI
2. Toggle **Vacation Mode → ON**
3. Optionally enter a reason (e.g. *"Holiday – returns 20 March"*)

**To disable:**
- Toggle **Vacation Mode → OFF** when the resident is back home

> ⚠️ If Vacation Mode is **not** enabled and the home is empty, the system **will** raise EMERGENCY alerts after 2 hours of silence.

---

## How Detection Works

The system checks two conditions every few minutes for each monitored room:

1. **Forecast ≥ 0.70** — the AI model expects someone to be present right now
2. **Silence ≥ 30 min** — no motion has been detected for at least 30 minutes

If **both** conditions are met simultaneously, an alert is published to MQTT.

If silence continues beyond **2 hours**, the severity escalates to **EMERGENCY** — regardless of the forecast score.

Repeated alerts for the same room are throttled to **once per hour** to avoid flooding.

---

## MQTT Alerts in Node-RED

### Topic

```
aragon/assistedliving/anomaly
```

### Example Payload

```json
{
  "severity": "EMERGENCY",
  "room": "Bedroom",
  "forecast": 1.000,
  "silent_minutes": 121,
  "vacation_mode": false
}
```

### Suggested Node-RED Flow

```
[MQTT In]  →  [Switch: severity == "EMERGENCY"]  →  [Notify / SMS / Email]
               └── [else]                         →  [Log / Dashboard]
```

1. Add an **MQTT In** node — set the topic to `aragon/assistedliving/anomaly`
2. Add a **JSON** node to parse the payload
3. Add a **Switch** node to route by `severity`:
   - `EMERGENCY` → push notification, SMS, or email
   - Other → log to dashboard or home automation system
4. Optionally filter on `vacation_mode == false` to suppress alerts during holidays

---

## Quick Reference

| Situation | What to do |
|---|---|
| Resident going on holiday | Enable **Vacation Mode** in the UI before they leave |
| Resident returns home | Disable **Vacation Mode** in the UI |
| Receiving too many alerts | Check alert throttle setting (default: 60 min) |
| No alerts arriving in Node-RED | Verify MQTT topic: `aragon/assistedliving/anomaly` |
| System just installed | Wait 7 days for the model to complete its learning period |

---

*The system requires at least 7 days of sensor data before anomaly detection becomes active.*
