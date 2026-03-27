# Homestead Monitoring

Desert-ready automated soil moisture monitoring for a porch garden using:
- **Olla irrigation** (passive watering)
- **ESP32** (active monitoring brain)
- **Solar power** (off-grid)
- **ntfy** (phone push alerts)

This project is designed for hot, dry climates where plants can decline quickly if the root zone dries out. The goal is to avoid crop loss by getting early warnings before stress becomes visible.

## 1) Project Goals

- Keep fruit and veggie root zones in a healthy moisture range.
- Alert when moisture falls below tolerance so you can refill the Olla before plants wilt.
- Run reliably outdoors in high heat, UV, and dust.
- Start with one bed/zone and support future expansion.

## 2) How the System Works

1. A capacitive soil sensor measures root-zone moisture near (not touching) the Olla.
2. ESP32 reads sensor values on a schedule.
3. Readings are filtered and converted to percent moisture using your dry/wet calibration.
4. If moisture is below threshold for a sustained period, ESP32 sends an `ntfy` push alert.
5. You refill the Olla reservoir and the system confirms recovery.
  ![How Olla Irrigation Works](how-olla-irrigation-works.png)
  
Olla = passive water delivery.  
ESP32 monitor = active warning system.

## 3) Recommended Shopping List (BOM)

## Core Electronics (Required)

- **ESP32 Dev Board**
  - Example: DOIT DevKit V1, NodeMCU-32S
  - Buy: [DOIT DevKit V1](https://www.amazon.com/ESP32-WROOM-32-Development-ESP-32S-Bluetooth-forArduino/dp/B08PCPJ12M)
    - Requirements: 3.3V logic, Wi-Fi support, accessible ADC pins
- **Waterproof Capacitive Soil Moisture Sensor**
  - Buy 2x DFRobot waterproof sensors:
    - [DFRobot SEN0308](https://www.dfrobot.com/product-2054.html)
    - Key specs: IP65 body, analog output (0-3V), 3.3-5.5V supply, 1.5m cable.
- **DS18B20 Waterproof Temperature Probe** (optional but strongly recommended)
  - Example: [5pcs DS18B20 Temp Sensor](https://www.amazon.com/HiLetgo-DS18B20-Temperature-Stainless-Waterproof/dp/B00M1PM55K)
  - Use for soil or shaded ambient temperature context
- **IP65+ Enclosure**
  - Example: [8x6x4 IP67 Enclosure](https://www.amazon.com/YETLEBOX-Waterproof-Electrical-Stainless-Enclosure/dp/B0BZHGCBTH)
  - UV-stable plastic, cable glands, gasketed lid
- **Outdoor-rated wiring and heat-shrink**
  - UV-resistant cable jacket preferred

## Solar Power (Required for Your Setup)

- **Solar panel:** 6V to 12V, 10W typical starter size
  - Example [10W 12V Solar Panel](https://www.amazon.com/Newpowa-Polycrystalline-Efficiency-Module-Marine/dp/B00W80N8TA)
- **Battery:** LiFePO4 6.4V or 12.8V pack (capacity based on autonomy goal)
  - Example: [LiFePO4 6V](https://www.amazon.com/LiFePO4-Rechargeable-Phosphate-Emergency-Terminals/dp/B09WYF8GP7)
- **Charge controller:** Compatible with panel + LiFePO4 chemistry
- **Buck converter:** Stable 5V output for ESP32 input
- **Inline fuse + disconnect switch** for safety and serviceability

## Mechanical / Garden Materials

- Probe mounting stake or holder to keep depth consistent
- Cable protection (split loom or conduit where exposed)
- Mulch (straw/wood chips): 2-4 inch layer to reduce evaporation
- Shade strategy for electronics enclosure (porch post, underside shade, or small sun shield)

## 4) Placement Guidelines (Critical for Accuracy)

- Place moisture probe **3-6 inches from Olla body** in active root zone.
- Probe depth should match the typical root depth of target crops.
- Do not place probe too close to Olla wall (will read artificially wet).
- Route cables away from direct afternoon sun if possible.
- Mount enclosure shaded and elevated from splash zone.

## 5) Firmware Logic (High Level)

The core loop should use:
- periodic sampling (for example every 5-15 minutes),
- rolling average (reduce noise),
- hysteresis (prevent alert chatter),
- cooldown timer (prevent notification spam).

Pseudo logic:

```cpp
readRawMoisture();
moisturePct = mapToPercent(raw, dryCal, wetCal);
smoothed = movingAverage(moisturePct);

if (smoothed < lowThreshold && cooldownExpired) {
  sendNtfy("Low moisture: refill Olla soon");
  markAlertSent();
}

if (smoothed > recoveryThreshold) {
  clearLowState();
}
```

Recommended first thresholds (to tune):
- `lowThreshold`: 20-30%
- `recoveryThreshold`: 30-40%
- `cooldown`: 4-12 hours depending on crop sensitivity

## 6) Calibration Procedure

Do this before deployment, and repeat seasonally:

1. **Dry reference**
   - Place probe in dry potting mix representative of your bed.
   - Record 20-50 readings; average = `dryCal`.
2. **Wet reference**
   - Saturate same mix to field capacity (not standing water).
   - Record 20-50 readings; average = `wetCal`.
3. **Map conversion**
   - Convert each raw reading to % between dry and wet bounds.
4. **Field tune**
   - Observe 7-10 days with real weather and adjust threshold for each crop group.

## 7) Alerting with ntfy

Use a private topic and optional access token.

Basic HTTP publish pattern:

```bash
curl -H "Title: Garden Alert" \
     -H "Priority: high" \
     -d "Moisture below threshold. Refill Olla." \
     https://ntfy.sh/<your-topic>
```

ESP32 implementation sends equivalent HTTPS requests.

Recommended notifications:
- Low moisture alert
- Critical moisture alert (optional second threshold)
- Recovery notification after refill
- Sensor fault warning (out-of-range / disconnected)
- Battery low warning (if battery telemetry added)

## 8) Power Budget (Starter Sizing Method)

Estimate:
- ESP32 active current x active minutes/day
- sleep current x sleep minutes/day
- sensor and regulator overhead
- weather margin (cloudy days)

Design target:
- 3+ days autonomy without sun
- panel sized to recharge daily use plus recovery margin

If unsure, oversize panel/battery in desert heat where efficiency drops.

## 9) Build and Commissioning Checklist

## Bench Test (Indoor)
- Wire ESP32 + sensor + temp probe.
- Verify stable ADC readings.
- Verify Wi-Fi reconnect after reboot.
- Trigger test alerts to phone.

## Dry Run (No Soil)
- Simulate dry/wet transitions manually.
- Validate threshold + cooldown behavior.

## Field Install
- Place sensor at target depth.
- Install mulch.
- Seal enclosure and glands.
- Observe readings over first week.

## First Week Tuning
- Note refill times and moisture minima.
- Adjust thresholds to catch low moisture earlier.
- Confirm no repeated spam alerts.

## 10) Desert Hardening Best Practices

- Keep enclosure out of direct peak sun.
- Use UV-resistant materials for all exposed wiring.
- Add drip loop at cable entry points.
- Keep all joints sealed and strain-relieved.
- Re-check calibration if soil composition changes.
- Maintain mulch layer; replace as it decomposes.

## 11) Project Roadmap

## Phase 1 - Documentation and procurement
- Finalize BOM and source parts.
- Prepare wiring diagram and pin map.

## Phase 2 - Single-zone prototype
- Build one sensor node and validate alerts.

## Phase 3 - Field reliability
- Tune for seasonal heat swings.
- Add battery telemetry and low-voltage alerts.

## Phase 4 - Expansion
- Multi-zone monitoring (multiple probes)
- Optional dashboard (Home Assistant/Grafana)
- Optional automated refill assist (if desired later)

## 12) Repository Structure (Planned)

```text
homestead-monitoring/
  README.md
  .gitignore
  firmware/
    esp32-olla-monitor.ino
  docs/
    BOM.md
    commissioning.md
    wiring.md
```

## 13) What to Build Next

1. Create `docs/BOM.md` with exact product links and quantities.
2. Create `docs/wiring.md` with ESP32 pin map and power path.
3. Create `firmware/esp32-olla-monitor.ino` starter with:
   - calibration constants,
   - rolling average filter,
   - threshold/hysteresis logic,
   - ntfy HTTPS notification function.