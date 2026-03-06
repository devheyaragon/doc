# Aqara FP300 — Installer Guide

---

## 1. Hardware Overview

The FP300 is a battery-powered multi-sensor combining:

- **60 GHz mmWave radar** — stationary presence detection
- **PIR sensor** — fast initial motion triggering
- **Temperature, humidity, and illuminance sensors**

It communicates over **Zigbee** (full feature set via Zigbee2MQTT) or Matter-over-Thread (limited parameter exposure). Always use **Zigbee mode** with Z2M for complete configuration control.

**Detection specifications:**
- Maximum detection range: **6 m**
- Field of view: **120°**
- Battery life: ~1 year (varies with polling intervals and room activity)

---

## 2. Physical Installation

### 2.1 Mounting Methods

| Method | Max Height | Notes |
|---|---|---|
| **Adhesive** | 2 m | Included in box; reversible |
| **Magnetic** | 2 m | Easiest to reposition during testing |
| **Expansion screws** | No limit | Required above 2 m |

> **Tip:** Use the magnetic mount during initial setup and testing. Only commit to adhesive or screws once you have validated coverage.

### 2.2 Height Guidelines

| Height | Requirement |
|---|---|
| **1.4 – 1.8 m** | Optimal for wall/corner mount |
| **~2.0 m** | Tilt sensor slightly downward toward activity zone |
| **> 2.0 m** | Screws mandatory; angle must compensate for height |
| **Ceiling** | Valid but reduces effective range; not recommended for rooms > 15 m² |

### 2.3 Placement Strategy

**Corner placement is strongly preferred** over flat-wall mounting. A corner position with the sensor facing the room center delivers the widest combined coverage and minimizes blind zones at room edges.

**Avoid placing near:**
- Air conditioning vents or outlets
- Ceiling fans or air purifiers
- Large metal or glass surfaces (reflections cause false detections)
- Positions where the detection cone crosses into adjacent rooms through walls

### 2.4 Near-Field Dead Zone

Detection below **1 m** from the sensor face is unreliable. Account for this when positioning — do not place the sensor where occupants regularly stand directly in front of it at close range.

### 2.5 Pre-Installation Recommendation

Before permanently fixing the sensor, use painter's tape or the magnetic mount to trial the position for **24–48 hours**. This reveals blind spots and interference sources before committing to a final installation.

---

## 3. Pairing with Zigbee2MQTT

1. In Z2M UI, click **Permit join**
2. Hold the sensor button for **5 seconds** until the LED flashes — this triggers pairing mode
3. The device will appear in Z2M as **`Aqara PS-S04D`**
4. Rename it in Z2M UI (e.g. `fp300_living_room`)
5. Verify the **Exposes** tab shows all parameters: `presence`, `temperature`, `humidity`, `illuminance`, `detection_range`, etc.

---

## 4. Configuration — Correct Order

> ⚠️ **Always apply all settings before running Spatial Learning.** The sensor calibrates its empty-room baseline against the active configuration. Changing settings after learning degrades accuracy and requires a new learning session.

---

### Step 1 — Set Detection Mode

| Z2M Parameter | Recommended Value | Notes |
|---|---|---|
| `presence_detection_options` | `mmwave` | Eliminates PIR-induced false triggers |
| `pir_detection_interval` | `60` sec | Only relevant if using `both`; limits PIR re-trigger rate |

Use `both` only if fast light-on response (< 1 sec) is a hard requirement. For all other cases, `mmwave` provides cleaner and more reliable presence data.

---

### Step 2 — Configure Sensitivity

| Z2M Parameter | Value | Notes |
|---|---|---|
| `motion_sensitivity` | See room profiles below | Core detection tuning |
| `ai_sensitivity_adaptive` | `ON` | Allows the sensor to self-tune over time |

---

### Step 3 — Set Absence Delay Timer

| Z2M Parameter | Range | Notes |
|---|---|---|
| `absence_delay_timer` | 10 – 300 sec | See room profiles below |

This timer defines how long after the last detected movement the sensor waits before declaring the room empty. Setting it too low causes false absences when occupants are sitting still.

---

### Step 4 — Restrict Detection Range

All 24 `detection_range_X` bands (each representing **0.25 m**) default to `true`, meaning the sensor scans the full 0–6 m including through walls. Restrict to your actual room depth.

**How to calculate:**
- Divide your room's maximum occupied depth (in metres) by 0.25 → number of bands to enable
- Always disable `detection_range_0` through `detection_range_3` (covers 0–1 m dead zone near mounting wall)
- Disable all bands beyond your room boundary

**Example — 4 m room, sensor in corner:**

| Bands | Range | Value |
|---|---|---|
| `detection_range_0` → `_3` | 0 – 1 m | `false` (dead zone) |
| `detection_range_4` → `_15` | 1 – 4 m | `true` |
| `detection_range_16` → `_23` | 4 – 6 m | `false` (beyond room) |

---

### Step 5 — Enable AI Interference Detection

| Z2M Parameter | Value | Notes |
|---|---|---|
| `ai_interference_source_selfidentification` | `ON` | Learns to ignore fans, AC drafts, convection |

---

### Step 6 — Run Spatial Learning

This is always the **last step**, after all other settings are saved.

1. **Empty the room completely**
2. In Z2M UI → Exposes tab → set `spatial_learning` → **Start Learning**
3. Stay out for a minimum of **60 seconds**
4. No confirmation appears in the UI or log — this is expected behavior
5. The sensor continues autonomous background learning indefinitely after the initial trigger

> If the room was occupied during any previous learning sessions, those baselines were contaminated. Each new session fully overwrites the previous one.

---

## 5. Room-Type Configuration Profiles

### 🛋️ Living Room

Scenario: extended occupancy, often seated and still; TV, fans, or AC common.

| Parameter | Value |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `medium` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `90` sec |
| Detection range | Limit to seating area depth (typically 3–5 m) |
| Mount position | Corner, 1.4–1.8 m, aimed at sofa/seating area |

---

### 🛏️ Bedroom

Scenario: sleeping occupant with minimal movement; critical to avoid false absence during sleep.

| Parameter | Value |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `high` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `180` sec |
| Detection range | Limit to bed distance (typically 2–4 m) |
| Mount position | Corner, 1.4–1.7 m, aimed directly at bed |

> **Note:** The FP300 does **not** support sleep monitoring (breathing/micro-movement detection). That feature is exclusive to the Aqara FP2. High sensitivity enables detection of very still occupants but is not equivalent to sleep monitoring.

---

### 🚿 Bathroom / Toilet

Scenario: short visits; fast on/off required; humidity sensor useful for fan automation.

| Parameter | Value |
|---|---|
| `presence_detection_options` | `both` |
| `motion_sensitivity` | `low` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `30` sec |
| Detection range | Tight — limit to room depth (typically 1–2.5 m) |
| Mount position | High corner above door, angled toward room interior |

> **Bonus:** Use the built-in humidity sensor to trigger an extractor fan when `humidity > 70%`, independent of presence detection.

---

### 🚶 Hallway / Corridor

Scenario: brief transit only; high risk of through-wall detection into adjacent rooms.

| Parameter | Value |
|---|---|
| `presence_detection_options` | `both` |
| `motion_sensitivity` | `low` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `OFF` |
| `absence_delay_timer` | `15` sec |
| Detection range | Match corridor length exactly; no extra margin |
| Mount position | End wall facing along corridor length, or high corner |

---

### 💼 Home Office / Study

Scenario: single occupant seated at desk for extended periods; light automation common.

| Parameter | Value |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `medium` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `60` sec |
| Detection range | Limit to desk distance (typically 1.5–3 m) |
| Mount position | Corner beside or above desk, aimed at workstation |

---

## 6. Sensitivity Quick Reference

| Room depth | Recommended sensitivity |
|---|---|
| < 4 m | `low` |
| 4 – 7 m | `medium` |
| > 7 m | `high` |

---

## 7. The Button

| Action | Function |
|---|---|
| **Single short press** | Checks Zigbee hub connection and verifies pairing status |
| **Hold for 5+ seconds** | Factory reset / re-pairing (also used to switch Zigbee ↔ Thread) |
| **Short press 10 times** | Forces network rejoin |

---

## 8. Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| Ghost detections at night | PIR reacting to thermal changes or drafts | Switch to `mmwave` only; enable `ai_interference_source_selfidentification` |
| False absences while sitting still | Sensitivity too low or `absence_delay_timer` too short | Increase `motion_sensitivity`; raise timer to ≥ 60 sec |
| Through-wall detections | Detection range not restricted | Disable `detection_range` bands beyond room boundary |
| Persistent false detections after full setup | Spatial Learning was run with room occupied | Re-run Spatial Learning with room completely empty |
| No presence detected close to sensor | Near-field dead zone (< 1 m) | Reposition sensor; keep `detection_range_0` – `_3` disabled |
| Sensor reports absent immediately after leaving | `absence_delay_timer` too short | Increase to appropriate room-type value |
