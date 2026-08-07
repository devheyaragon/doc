# Assisted Living Monitor — User Guide

**Version 1.1 · August 2026**

---

## What It Does

The Assisted Living Monitor watches for unexpected inactivity in the home. It learns the resident's normal daily patterns, then raises an alert if no movement is detected during a time when the model predicts someone should be present. For a fuller explanation of how the underlying prediction model learns your home's patterns, see the main *Predictive Home Automation Service* documentation, §3.

Anomaly detection itself requires at least **7 days** of collected sensor data before it becomes active — a separate, smaller requirement specific to this feature, distinct from the main prediction model's own 14-day training window described in that document. Don't be surprised these two numbers differ; they answer different questions.

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
| Cross-room activity window | 30 min | How recently *another* monitored room must have shown activity to hold back an EMERGENCY (see below) |
| MQTT topic | `aragon/assisted_living/anomaly` | Where alerts are published |

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

> ⚠️ If Vacation Mode is **not** enabled, a monitored room stays silent for more than **2 hours**, **and no other monitored room has shown recent activity either**, the system **will** raise an EMERGENCY alert. If another room *has* shown recent activity, the alert is held back instead — see "How Detection Works" below for why, and how to see it happened anyway.

---

## How Detection Works

The system checks two conditions every few minutes for each monitored room:

1. **Forecast ≥ 0.70** — the AI model expects someone to be present right now
2. **Silence ≥ 30 min** — no motion has been detected for at least 30 minutes

If **both** conditions are met simultaneously, an alert is published to MQTT.

If silence continues beyond **2 hours**, the system checks one more thing before escalating: has **any other monitored room** shown genuine activity within the last 30 minutes? If so, the extended silence in this one room is treated as expected — the resident being active elsewhere in the home is itself evidence they're safe — and a low-severity `info` alert is published instead (see below), not an EMERGENCY. Only if no monitored room anywhere has shown recent activity does the severity escalate to **EMERGENCY**, regardless of the forecast score.

Repeated alerts for the same room — of either severity — are throttled to **once per hour** to avoid flooding. EMERGENCY and `info` alerts are throttled independently of each other, so a string of held-back `info` alerts can never delay a genuine EMERGENCY.

---

## MQTT Alerts in Node-RED

### Topic

```
aragon/assisted_living/anomaly
```

> Note the underscore in `assisted_living` — a common typo elsewhere is `assistedliving`, which will silently receive nothing.

### Example Payload — EMERGENCY

```json
{
  "alert_type": "absence_anomaly",
  "severity": "emergency",
  "room": "Bedroom",
  "forecast_probability": 1.0,
  "minutes_silent": 121.0,
  "threshold_used": 0.7,
  "silence_threshold_minutes": 30,
  "detection_method": "extended_absence_no_holiday",
  "timestamp": "2026-08-06T21:14:02.331Z",
  "message": "[EMERGENCY] Expected activity in Bedroom but no motion detected for 121 minutes."
}
```

### Example Payload — held-back alert (`info`)

Published instead of an EMERGENCY when Vacation Mode or cross-room corroboration explains the silence:

```json
{
  "alert_type": "absence_anomaly_suppressed",
  "severity": "info",
  "room": "Bedroom",
  "forecast_probability": 1.0,
  "minutes_silent": 121.0,
  "detection_method": "extended_absence_activity_elsewhere",
  "timestamp": "2026-08-06T21:14:02.331Z",
  "message": "[INFO] Bedroom silent for 121 minutes, but recent activity was confirmed in another monitored room — no action needed."
}
```

`detection_method` tells you *why* it was held back: `extended_absence_activity_elsewhere` (cross-room corroboration) or `extended_absence_holiday_mode` (Vacation Mode was on). There is no `vacation_mode` field on either payload — Vacation Mode's own on/off state is published separately, on `aragon/assisted_living/vacation_status`, not embedded in each alert.

### Suggested Node-RED Flow

```
[MQTT In]  →  [JSON]  →  [Switch: severity]
                            ├── "emergency"  →  [Notify / SMS / Email]
                            ├── "info"       →  [Log / Dashboard]
                            └── else         →  [Log / Dashboard]
```

1. Add an **MQTT In** node — set the topic to `aragon/assisted_living/anomaly`
2. Add a **JSON** node to parse the payload
3. Add a **Switch** node to route by `severity`:
   - `emergency` → push notification, SMS, or email
   - `info` → log to dashboard (optional: show the `detection_method` field so a caregiver can see *why* an expected alert didn't fire as EMERGENCY)
   - Other → log to dashboard or home automation system
4. If you want to distinguish a held-back alert caused by Vacation Mode from one caused by another room's activity, filter or branch on `detection_method` rather than a `vacation_mode` field — the payload doesn't carry the latter.

---

## Quick Reference

| Situation | What to do |
|---|---|
| Resident going on holiday | Enable **Vacation Mode** in the UI before they leave |
| Resident returns home | Disable **Vacation Mode** in the UI |
| Receiving too many alerts | Check alert throttle setting (default: 60 min) |
| No alerts arriving in Node-RED | Verify MQTT topic: `aragon/assisted_living/anomaly` (note the underscore) |
| Expected an EMERGENCY but got nothing | Check Event History for an `info`-severity alert on that room instead — another monitored room likely showed recent activity (or Vacation Mode was on); see `detection_method` in the alert for which |
| System just installed | Wait 7 days for anomaly detection to activate |

---

*The system requires at least 7 days of sensor data before anomaly detection becomes active. (This is separate from, and shorter than, the main prediction model's own 14-day training window — see the main documentation, §4, for that distinction.)*

*Version 1.1 — Corrections applied: MQTT topic corrected from `aragon/assistedliving/anomaly` to `aragon/assisted_living/anomaly` throughout (previous version would have silently received no alerts at all). Example payload corrected — `forecast` → `forecast_probability`, `silent_minutes` → `minutes_silent`, and the non-existent `vacation_mode` field removed (Vacation Mode state is published separately, on its own topic, not embedded in each alert); real fields (`alert_type`, `threshold_used`, `detection_method`, `timestamp`, `message`) added to match the actual payload. Added cross-room activity corroboration throughout — a real check the system performs before escalating to EMERGENCY, not previously documented — including its own held-back `info`-severity alert type, a new example payload, an updated Node-RED flow with a third routing branch, and a corrected Vacation Mode warning that no longer implies EMERGENCY is the only possible outcome of an unattended extended silence. Added a cross-reference to the main documentation for the underlying learning process, and a clarifying note on why this guide's "7 days" and the main documentation's "14 days" are different, correct figures rather than a contradiction.*
