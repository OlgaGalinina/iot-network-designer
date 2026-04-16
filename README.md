# IoT Network Designer

An interactive IoT radio/network technology comparison tool designed based on the materials of our MSc/PhD course on Wireless IoT (Tampere University, finland).

This project is a learning and intuition-building aid for comparing wireless IoT technologies. It helps users reason about trade-offs between latency, range, scale, data rate, power, deployment complexity, and mobility.

It is not a precise engineering optimizer. For professional use, we recommend treating the output as a structured first-pass design suggestion, then validate with vendor data, link budgets, regulations, measurements, and standards.

---

## What it covers

The scope is radio and networking technologies only. It intentionally avoids app-layer stacks.

Current coverage: 42 technologies 

| Category | Technologies |
|---|---|
| Unlicensed IoT, short range | RFID, NFC, Bluetooth LE, ZigBee, Thread, Z-Wave, 6LoWPAN, ANT/ANT+, EnOcean, WirelessHART, UWB, WiGig |
| Unlicensed IoT, medium range | DASH7, Wi-SUN, Ingenu RPMA, Weightless-N/P/W, Wirepas Mesh, ISA100.11a, DECT-2020 NR |
| Unlicensed IoT, long range LPWAN | LoRa/LoRaWAN, NB-Fi, Telensa, MIoTy |
| Wi-Fi variants | Wi-Fi HaLow, 802.11af White-Fi, 802.11p DSRC/V2X, Wi-Fi 5/6/6E/7/8 |
| Cellular (3GPP) | EC-GSM-IoT, NB-IoT, LTE-M, LTE Cat-1/4, 5G mMTC, 5G URLLC, 5G RedCap, Private 5G |
| Satellite / NTN | NB-IoT via NTN, Iridium / Globalstar |

---

## How to use

1. Open `index.html` in a browser.
2. Pick a scenario preset or adjust the requirement sliders manually.
3. Set hard constraints such as no cellular operator, mobile devices, private / no cloud, and outdoor / harsh environment.
4. Use category filters to hide whole families of technologies.
5. Read the grouped table:
    - <span style="color:green"><b>Fit</b></span>: all graded requirements match or exceed the target.  
    - <span style="color:orange"><b>Can be used with caveats</b></span>: one close miss.  
    - <span style="color:red"><b>Poor fit</b></span>: a hard blocker, a major miss, or too many close misses.
6. Select 2-4 non-poor-fit technologies and click Compare for side-by-side trade-off reasoning.
7. Switch to Design Reasoning mode to see the PHY / MAC / Architecture reasoning blocks and a compact explanation of downgraded alternatives.

The star display in the Battery column is a qualitative energy-suitability indicator, not a lifetime estimate.

---

## Modes

### Designer

Designer mode is the main comparison view. It shows technologies grouped into Fit, Caveat, and Poor Fit buckets. Users can select comparable technologies and open the side-by-side comparison panel.

### Design reasoning (less tested)

Design Reasoning mode keeps the same scenario inputs but adds a reasoning panel:

- PHY blocks such as Sub-GHz PHY, CSS, UNB, OFDM / OFDMA, NB PHY, and mmWave
- MAC blocks such as ALOHA, CSMA/CA, TDMA, OFDMA UL, PSM, eDRX, and handover
- Architecture blocks such as unlicensed spectrum, licensed operator, star, mesh, and self-deployable
- A technology-level explanation of why notable alternatives were downgraded or excluded

The block layer is scenario-level reasoning. The technology reasoning layer explains individual technology trade-offs.

---

## User inputs

The main sliders are qualitative requirement levels:

| Slider | Meaning |
|---|---|
| Latency | From max 1 ms to best effort |
| Coverage | From any range to global |
| Number of devices | From any scale to massive deployments |
| Data rate | From any rate to at least 1 Gbps |
| Power / battery | Any power, battery-powered ok, or long-term battery |
| How to deploy? | Anyone, needs operator SIM, or needs specialist install |

Power / battery is intentionally broad:

| Value | Meaning |
|---|---|
| Any power | Energy is not a deciding constraint |
| Battery-powered ok | Rechargeable or managed battery devices are acceptable |
| Long-term battery | Unattended low-power sensing is preferred |

Technology-side battery scores are also qualitative:

| Score | Meaning |
|---|---|
| 5 | Long-term / multi-year low-power operation is feasible in the right duty cycle |
| 4 | Strong battery suitability |
| 3 | Moderate battery suitability |
| 2 | Rechargeable / frequent charging likely |
| 1 | Best with continuous power or very managed rechargeable designs |

---

## Result buckets

The app uses strict graded mismatch logic for the main requirements.

For each graded dimension:

| Requirement match | Result |
|---|---|
| Technology is exact or better | Fit for that dimension |
| Technology misses by 1 grade | Caveat mismatch |
| Technology misses by 2+ grades | Poor mismatch |

Overall bucket assignment:

| Bucket | Rule |
|---|---|
| Fit | No hard blockers, no poor mismatches, no caveat mismatches |
| Can be used with caveats | No hard blockers, no poor mismatches, exactly 1 caveat mismatch |
| Poor fit | Any hard blocker, any poor mismatch, or 2+ caveat mismatches |

Score dots are secondary. They help sort and compare, but they do not decide the bucket.

Current hard blockers:

- No cellular operator selected while the technology requires operator-managed access
- Private / no cloud selected while the technology cannot realistically satisfy that deployment
- Category hidden by the category filter, which removes the technology from visible results

Other requirement gaps are usually expressed as trade-offs unless they are two or more grades away.

---

## Reasoning style

The tool tries to explain design trade-offs instead of treating technologies as simply passing or failing.

Examples of intended reasoning:

- LoRa supports long-term battery operation by accepting smaller payloads, lower throughput, and looser latency.
- Wi-Fi can operate on battery-powered devices, but higher data rates and more active links usually mean more frequent charging than low-power IoT radios.
- LTE-M can run on battery-powered devices, but mobility and cellular overhead reduce battery life relative to static LPWAN sensing.
- NB-IoT supports wide-area low-power sensing, but operator dependence makes it a poor fit when self-deployment is required.

---
---

## Developer guide

The application is intentionally plain HTML, CSS, and JavaScript in `index.html`. Do not convert it into a framework app unless the project direction changes.

Important top-level structures:

| Structure | Purpose |
|---|---|
| `TECHS` | Technology metadata and qualitative capability grades |
| `SCENARIOS` | Preset slider/toggle states |
| `REASONING` | Compact technology-level strengths, weaknesses, caveats, and trade-off text |
| `BLOCKS` | Scenario-level PHY / MAC / Architecture reasoning blocks |
| `state` | Current UI state: sliders, toggles, mode, selected compare IDs |
| `BUCKETS` | Labels and short explanations for result groups |

Important functions:

| Function | Purpose |
|---|---|
| `evaluateTech(t)` | Central source of truth for visibility, score, bucket, hard blockers, reasons, and trade-off text |
| `passes(t)` | Compatibility wrapper that returns true for visible non-poor results |
| `fitScore(t)` | Compatibility wrapper for the secondary score |
| `updateDisplay()` | Updates slider labels, evaluates all technologies, groups table rows, and updates compare controls |
| `updateBlocks()` | Updates the scenario-level reasoning blocks |
| `updateTechReasoning(evals)` | Shows compact why-downgraded notes in reasoning mode |
| `runCompare()` | Builds the side-by-side comparison using `evaluateTech()` results |

---

## Evaluation logic

`evaluateTech(t)` should remain the single source of truth for classification. New UI surfaces, compare logic, and reasoning panels should consume its structured return object instead of rebuilding separate elimination logic.

It returns:

| Field | Meaning |
|---|---|
| `tech` | Original technology object |
| `visible` | Whether category filters allow it to appear |
| `score` | 0-5 display score for sorting and dots |
| `bucket` | `fit`, `caveat`, or `poor` |
| `hardFail` | Hard blocker reasons |
| `fitReasons` | Exact-or-better requirement matches |
| `caveatReasons` | One-grade misses and soft trade-offs |
| `poorReasons` | Two-or-more-grade misses or strong conflicts |
| `tradeoffText` | Human-readable trade-off explanation |
| `unlockText` | Short strength text for future UI use |

The current graded dimensions are:

- latency
- range
- device scale
- data rate
- battery requirement
- deployment complexity

Helper functions such as `rangeGrade(t)` and `batteryGrade(t)` translate technology metadata into the same qualitative grades used by the UI.

When changing the model:

- Keep hard blockers rare and explicit.
- Keep score secondary to bucket logic.
- Use caveats for close misses only.
- Send two-or-more-grade misses to Poor Fit.
- Prefer trade-off wording over failure wording.
- Avoid false precision, especially for battery life, range, and capacity.

---

## Technology metadata notes

`TECHS` uses compact qualitative fields:

| Field | Meaning |
|---|---|
| `range_km` | Representative range class input, not a guarantee |
| `latency_max` | Qualitative latency grade, lower is stricter |
| `devices_max` | Qualitative scale grade |
| `datarate_max` | Qualitative data-rate grade |
| `battery` | Qualitative energy-suitability score from 1 to 5 |
| `noop` | Can be used without a cellular/operator-managed service |
| `private` | Can realistically satisfy private / no-cloud assumptions |
| `mobile` | Supports moving devices or mobility assumptions |
| `outdoor` | Suitable for outdoor or harsh deployments |
| `deploy` | Deployment complexity grade |

These values are deliberately representative. If you improve them, prefer small, justified changes and update reasoning text when the user-facing explanation would otherwise become misleading.

---

## Adding or updating technologies

1. Add or update the entry in `TECHS`.
2. Add compact `REASONING` metadata for strengths, weaknesses, trade-offs, and common caveats.
3. Check whether `BLOCKS` should teach anything new about the technology family.
4. Confirm `evaluateTech()` already handles the dimensions you need.
5. Test a few scenarios in both "Designer" and "Design reasoning" modes.
6. Compare against nearby technologies to make sure buckets feel consistent.

Do not add app-layer technologies unless the project scope changes.

---

## Academic Context

This tool is designed to support student learning in the course "COMM.SYS.710 Internet of Things Wireless Communications" at Tampere University, Finland.

The taxonomy follows a range- and architecture-oriented view of IoT wireless networking, with additional practical categories for Wi-Fi variants, cellular, and satellite / NTN.

---

## Standards References

Key documents underlying the technology data in this tool:

- 3GPP TS 22.261, Service requirements for the 5G system
- 3GPP TR 45.820, Cellular system support for ultra-low complexity and low throughput IoT
- 3GPP TS 36.888, Study on provision of low-cost MTC UEs
- IEEE 802.15.4-2020, Low-Rate Wireless Networks
- IEEE 802.11ah-2016, Wi-Fi HaLow
- IETF RFC 4944, Transmission of IPv6 Packets over IEEE 802.15.4 Networks
- ITU-R M.2083, IMT-2020 vision
- ETSI TS 103 636, DECT-2020 NR
- IEC 62591, WirelessHART
- IEC 62734, ISA100.11a

---

## Contributing

Contributions are welcome, especially:

- Corrections to technology metadata
- Better trade-off explanations
- Missing radio/network technologies within the current scope
- Scenario presets that improve teaching value
- Small UI improvements that preserve the single-file architecture

When contributing, keep the tool lightweight, qualitative, and readable.
