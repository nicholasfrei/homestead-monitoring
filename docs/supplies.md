# Bill of Materials (BOM)

Supplies for a single-zone Olla irrigation monitoring setup. Quantities reflect one bed with one ESP32 node and two moisture sensor probes.

---

## Traditional Gardening Supplies

| Item | Qty | Link | Notes |
|------|-----|------|-------|
| Metal Planter (galvanized raised bed) | 1 | [Land Guard Galvanized Planter](https://www.amazon.com/Land-Guard-Galvanized-Planter-Vegetables/dp/B09C8HR4Z9) | 4ft x 2ft x 1ft raised bed |
| Bagged Wood Mulch (1.5 cu ft bags) | 2 | [Earthgro Brown Shredded Mulch](https://www.homedepot.com/p/Earthgro-Earthgro-1-5-cu-ft-Brown-Wood-Shredded-Bagged-Mulch-88659180/311613385) | 2–4 inch layer over bed |
| All-Natural Garden Soil (2 cu ft bags) | 3 | [Kellogg Garden Organics All-Natural Garden Soil](https://www.homedepot.com/p/Kellogg-Garden-Organics-All-Natural-Garden-Soil-Organic-Soil-For-Flowers-and-Vegetables-2-cf-ft-OMRI-Listed-6850/205617876) | Organic garden soil |
| Olla / Terra Cotta Irrigation Pot with Lid | 2 | [Classic Olla Watering Pot with Lid](https://www.amazon.com/Olla-Company-Classic-Large-Irrigation/dp/B0BTRKQHSR) | Passive subsurface watering |

---

## Core Electronics

| Item | Qty | Link | Notes |
|------|-----|------|-------|
| ESP32 Dev Board (DOIT DevKit V1 or NodeMCU-32S) | 1 | [DOIT DevKit V1](https://www.amazon.com/ESP32-WROOM-32-Development-ESP-32S-Bluetooth-forArduino/dp/B08PCPJ12M) | 3.3V logic, Wi-Fi, accessible ADC pins required |
| Waterproof Capacitive Soil Moisture Sensor (DFRobot SEN0308) | 2 | [DFRobot SEN0308](https://www.dfrobot.com/product-2054.html) | IP65, analog 0–3V output, 3.3–5.5V supply, 1.5m cable; capacitive avoids corrosion |
| DS18B20 Waterproof Temperature Probe | 1 | [HiLetgo DS18B20 5-pack](https://www.amazon.com/HiLetgo-DS18B20-Temperature-Stainless-Waterproof/dp/B00M1PM55K) | Optional but recommended; soil/ambient temp context for calibration |
| IP67 Weatherproof Enclosure (8×6×4 in) | 1 | [YETLEBOX IP67 Enclosure](https://www.amazon.com/YETLEBOX-Waterproof-Electrical-Stainless-Enclosure/dp/B0BZHGCBTH) | UV-stable plastic, cable glands, gasketed lid |
| Outdoor-Rated Wiring + Heat Shrink (assorted) | 1 set | — | UV-resistant jacket; standard PVC degrades quickly in desert sun |

---

## Solar Power

| Item | Qty | Link | Notes |
|------|-----|------|-------|
| 12V 10W Solar Panel | 1 | [Newpowa 10W Poly Panel](https://www.amazon.com/Newpowa-Polycrystalline-Efficiency-Module-Marine/dp/B00W80N8TA) | 6–12V acceptable; 10W is a good starter size |
| LiFePO4 Battery (6V or 12V pack) | 1 | [NERMAK LiFePO4 6V](https://www.amazon.com/NERMAK-Rechargeable-Phosphate-Emergency-Terminals/dp/B0BX3GL6FY) | LiFePO4 preferred over Li-ion for thermal stability in heat |
| Solar Charge Controller | 1 | — | Must support LiFePO4 chemistry and match panel voltage |
| DC-DC Buck Converter (to 5V regulated) | 1 | — | Stable 5V output for ESP32 supply |
| Inline Fuse + Disconnect Switch | 1 set | — | Safety and serviceability |

---

## Miscellaneous / Hardware

| Item | Qty | Notes |
|------|-----|-------|
| Cable Protection (split loom or conduit) | as needed | Wherever cables are exposed to sun or abrasion |
| Probe Mounting Stake or Holder | 1–2 | Keep sensor at consistent depth in root zone |
| Enclosure Shade Mount | 1 | Porch post, shelf underside, or sun shield — keep enclosure out of direct afternoon sun |
| Cable Glands (if not included with enclosure) | 3–4 | Weatherproof cable entry points |
| Zip Ties, Velcro Straps | as needed | Cable management inside and outside enclosure |

---

## Notes

- Quantities above are for **one zone** (one bed, one ESP32 node, two moisture probes).
- For multi-zone expansion, additional ESP32 boards and sensor pairs are needed per zone.
- Links are provided as examples; equivalent products are acceptable as long as specs match.
- See `docs/wiring.md` for pin assignments and power path details.
