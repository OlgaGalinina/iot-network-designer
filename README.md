# iot-network-designer
Interactive IoT wireless technology comparison and network design tool (~40 most popular technologies)

---

## What it does

You describe your deployment scenario (latency requirement, coverage area, number of devices, power budget, spectrum preference) and the tool filters and ranks multiple IoT wireless technologies accordingly. You can then select 2-4 technologies and compare them side by side.

The tool covers the full landscape: short-range (BLE, ZigBee, Thread, UWB, WirelessHART), medium-range (DASH7, Wi-SUN, Wirepas), LPWAN (LoRaWAN, NB-Fi, MIoTy), Wi-Fi variants (HaLow, 802.11af, Wi-Fi 5/6/7), cellular (EC-GSM-IoT, NB-IoT, LTE-M, 5G mMTC/URLLC/RedCap, Private 5G), satellite/NTN, and industrial protocols (ISA100.11a, DECT-2020 NR).

---

## How to use

1. Pick a scenario preset (smart factory, agriculture, asset tracking, etc.) or set sliders manually
2. Toggle hard constraints: battery-powered, no cellular operator, mobile devices, private deployment
3. The table filters and ranks technologies in real time. Green fit-score dots show how well each matches
4. Check 2–4 technologies and click **Compare** for a side-by-side breakdown with elimination reasoning

---

## Design notes

This tool is intentionally a **curated comparison with stated assumptions**, not a design authority. IoT technology performance (range, battery life, capacity) is highly deployment-dependent. The values shown are representative baselines for typical deployments; always validate against your specific environment, vendor datasheets, and applicable standards documents.

**Taxonomy note:** the tool distinguishes between formal standards (IEEE/3GPP/IETF), multi-vendor ecosystems (ZigBee, LoRaWAN), vendor/proprietary technologies (Wirepas, Ingenu RPMA), and protocol frameworks (6LoWPAN). This matters when evaluating interoperability and long-term vendor dependency.

**Key KPI philosophy:** for 5G service classes, the defining metrics are used rather than headline throughput numbers. mMTC is characterized by connection density (1M devices/km², ITU IMT-2020); URLLC by latency (≤1ms U-plane) and reliability (99.9999%, 3GPP TS 22.261); RedCap by reduced device complexity and ≤20MHz bandwidth.

---

## Academic context

This tool is developed alongside the course **COMM.SYS.710 Internet of Things Wireless Communications** at Tampere University, Finland. The technology classification follows the course's range-based taxonomy (short / medium / long-range LPWAN / cellular) with additional industry-pragmatic categories.


If you find an error or a missing technology, please let us know.
Contributions welcome, especially corrections to technology parameters and additions of missing protocols.


---

## Technology coverage (37 total)

| Category | Technologies |
|---|---|
| Short range <100m | RFID, NFC, Bluetooth LE, ZigBee, Thread, Z-Wave, 6LoWPAN, ANT/ANT+, EnOcean, WirelessHART, UWB, WiGig |
| Medium range 100m–5km | DASH7, Wi-SUN, Ingenu RPMA, Weightless-N/P/W, Wirepas Mesh |
| LPWAN unlicensed | LoRa/LoRaWAN, NB-Fi, Telensa, MIoTy |
| Wi-Fi variants | 802.11ah (HaLow), 802.11af (White-Fi), 802.11p (V2X), Wi-Fi 5/6/6E/7 |
| Cellular (3GPP) | EC-GSM-IoT, NB-IoT, LTE-M, LTE Cat-1/4, 5G mMTC, 5G URLLC, 5G RedCap, Private 5G |
| Satellite / NTN | NB-IoT via NTN (R17), Iridium/Globalstar |
| Industrial | ISA100.11a, DECT-2020 NR |


---

## Standards references

Key documents underlying the technology data in this tool:

- 3GPP TS 22.261 — Service requirements for the 5G system
- 3GPP TR 45.820 — Cellular system support for ultra-low complexity and low throughput IoT (EC-GSM-IoT)
- 3GPP TS 36.888 — Study on provision of low-cost MTC UEs (NB-IoT origin)
- IEEE 802.15.4-2020 — Low-Rate Wireless Networks
- IEEE 802.11ah-2016 — Wi-Fi HaLow
- IETF RFC 4944 — Transmission of IPv6 Packets over IEEE 802.15.4 Networks (6LoWPAN)
- ITU-R M.2083 — IMT-2020 vision (mMTC/URLLC KPIs)
- ETSI TS 103 636 — DECT-2020 NR
- IEC 62591 — WirelessHART
- IEC 62734 — ISA100.11a
