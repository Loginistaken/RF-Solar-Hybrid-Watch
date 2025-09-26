# MEMS RF Hybrid Watch: Updated System Canvas

## 1. Primary Oscillator: MEMS Super-TCXO

- **What:** Replace traditional quartz crystal with a MEMS Super-TCXO (Temperature Compensated Xtal Oscillator), e.g. from SiTime.
- **Why:** MEMS Super-TCXOs are much smaller, more robust, and offer superior stability across temperature compared to quartz, especially when digitally compensated.
- **How:** The MEMS TCXO runs continuously, drawing ultra-low power (typically <1μA to a few μA).

## 2. Energy Buffering & Duty Cycling

- **What:** Integrate a small supercapacitor or thin-film rechargeable battery as a buffer.
- **Why:** RF energy harvesting is intermittent and low, so accumulating energy for bursts of higher-power operations (like syncing) is essential.
- **How:** The buffer steadily charges from harvested RF (RF-DC converter), powers the oscillator, and allows for brief activation of high-draw components (radio, CSAC, etc.).

## 3. Optional High-Precision Sync

- **What:** Add a secondary, high-precision time reference (CSAC or GNSS/GPS/NTP radio).
- **Why:** Even the best oscillators drift over weeks/months. Episodically syncing to a reference ensures long-term absolute accuracy.
- **How:** Only activate the secondary reference when the energy buffer is full, e.g., once per day/week, to minimize average energy consumption.

## 4. Digital Temperature Compensation & Calibration

- **What:** Integrate a microcontroller with a temperature sensor.
- **Why:** Both MEMS and TCXO can drift with temperature. Digital compensation routines can correct this nearly for free, computationally.
- **How:** The microcontroller reads temperature, applies a correction curve to oscillator ticks, and keeps the timebase accurate.

## 5. Low-Power System Strategy

- **What:** Keep all non-essential systems (UI, display, radios) asleep most of the time.
- **Why:** Power is precious! Only the oscillator and microcontroller run continuously; everything else is event-driven.
- **How:** Use a low-power display (E-ink, mechanical hands, etc.) that only updates on user interaction or time events.

## 6. System Diagram

```
[RF Energy Harvester]───>[Energy Buffer (supercap)]
                                 │
                                 │
                          [MEMS Super-TCXO]
                                 │
                          [Microcontroller w/ Temp Sensor]
                                 │
           ┌──────────────┬──────┴────────┬─────────────┐
           │              │                │             │
    [Low-Power Display] [Radio/GNSS]   [CSAC Module]  [Other]
          (asleep)        (only on      (rarely on)
                          for sync)
```

---

## Comparison: Quartz vs. MEMS RF Hybrid

| Feature                   | Traditional Quartz Watch    | MEMS RF Hybrid (This Design)     |
|---------------------------|----------------------------|----------------------------------|
| Oscillator                | Quartz                     | MEMS Super-TCXO                  |
| Power Source              | Battery/capacitor          | RF harvesting + buffer           |
| Temperature Compensation  | Analog (basic) or none     | Digital, precise                 |
| Long-term Accuracy        | Good, but drifts           | Excellent, with periodic resync  |
| High-Precision Sync       | Not present                | Optional (CSAC, GNSS, etc.)      |
| Display                   | LCD/hands                  | E-ink/hands (low-power)          |
| Energy Use                | Low                        | Ultra-low, event-driven          |
| Feasibility (2025)        | Mature, robust             | Technically feasible, challenging|

---

## Does It Work? Is the Idea Plausible?

- **Base Feasibility:**  
  - MEMS TCXO chips (e.g., SiTime) are commercially available and are used in demanding applications.
  - RF energy harvesting is real, but the harvested power is *very* limited (nW–μW range in typical ambient conditions).
  - Supercapacitors/thin-film batteries can buffer enough energy for brief operations (e.g., radio sync).

- **Challenges:**  
  - **Energy Budget:** Keeping the system ultra-low power is critical. Modern MEMS TCXOs and microcontrollers can run within the μW envelope. Display and radios must be OFF almost always.
  - **RF Environment:** Energy harvesting is highly dependent on ambient RF power density (urban = better, rural = harder).
  - **High-Precision Sync:** CSACs are expensive and draw much more power when warm-up. GNSS or Bluetooth sync is more practical, but still requires careful duty cycling.
  - **Size/Integration:** All components must be miniaturized; SiP or advanced PCB design required.

- **Plausibility (2025):**  
  - **Yes, in principle:** All core components exist. The main constraint is energy: continuous timekeeping is possible, but frequent sync/display updates are limited by what can be harvested and stored.
  - **Practicality:** For watches in RF-rich environments (e.g., cities, offices), this could work well. For remote/rural, less so unless supplemented with solar or kinetic energy.

- **Conclusion:**  
  - **A MEMS Super-TCXO, digitally compensated, energy-buffered, and occasionally resynced with an absolute reference, powered by RF harvesting, is technically plausible with 2025 components—but requires careful design and user expectations for limited display and sync frequency.**

---

## References

- SiTime MEMS Super-TCXO: [SiTime Product Page](https://www.sitime.com/products/super-tcxo)
- Energy Harvesting: [TI RF energy harvesting](https://www.ti.com/energy-harvesting/rf.html)
- CSAC: [Microchip Chip-Scale Atomic Clock](https://www.microchip.com/en-us/products/clocks-timing/atomic-clocks/chip-scale-atomic-clocks)
- E-ink displays: [E Ink Technology](https://www.eink.com/)
Quick conclusion (TL;DR)

The MEMS RF-hybrid architecture is sound. The big limiting factor is raw harvested power: with a wrist-sized antenna you should expect fractions of a microwatt to ≈1 µW in typical urban conditions — enough to keep a very low-power timebase alive only if that timebase truly runs in the single-µW range and you aggressively duty-cycle everything else. To enable occasional higher-power ops (sync, display refresh, short radio bursts) further needed a buffer (supercap / thin-film cell) and/or a second ambient harvester (tiny solar or kinetic) to make the system practical.

Energy reality — worked numbers

(doing the arithmetic explicitly)

Antenna area: 10 cm² = 0.001 m²

Ambient RF power density (typical urban range): 0.1 – 1.0 mW/m²

Power captured (before conversion) = power_density × area:

Low case: 0.1 mW/m² × 0.001 m² = 0.0001 mW = 0.1 µW

High case: 1.0 mW/m² × 0.001 m² = 0.001 mW = 1.0 µW

Rectifier + matching + losses (realistic efficiency): 20% – 50%

Harvested usable power range ≈ 0.02 µW – 0.5 µW from that antenna in those conditions.

Compare to components:

Ultra-low MEMS oscillator or MCU in deep sleep can sometimes approach ~1 µW or a few µW.

Many MEMS Super-TCXO parts advertise microamp current; at ~1.8–3.0 V that’s ≈1.8–3 µW per µA (so 1 µA ≈ 1.8 µW at 1.8 V).

Conclusion: continuous operation of a MEMS TCXO alone will likely exceed the power you can harvest continuously from a 10 cm² strap antenna in typical conditions. That means: rely on buffering, duty cycling, hybrid harvesting, or larger/more efficient antenna designs.

Practical design changes & recommendations (what to do now)

Make the antenna a priority

Use a multi-band rectenna (2.4 GHz Wi-Fi + cellular mid bands + FM if practical). Fold the antenna into the strap, 

bezel, and underside (textile printed traces) to maximize area without enlarging the case.

Use flexible printed metal or silver-ink textile traces so the strap is the antenna.

Design a proper RF front end

Matching network (tuned for the chosen bands) — essential for maximizing captured power.

Use a low-threshold Schottky rectifier or a matched diode-bridge rectifier optimized for microwatt inputs. Consider synchronous or active rectifier techniques for better low-level efficiency.

Consider rectenna (antenna + rectifier integrated as one flexible element).

Energy harvesting PMIC

Use a dedicated energy-harvester PMIC that can operate with tens of millivolts and micro-amps, provide cold-start, and manage charging to the buffer (supercap/thin film cell). It should include boost regulation and low-voltage cut-offs.

Energy buffer sizing: example

To support a 50 mW radio burst that lasts 1 second, required energy = 0.05 J (since 50 mW × 1 s = 0.05 J).

Energy in capacitor: 
E=0.5×C×(V12−V22)
E=0.5×C×(V
1
2
	​

−V
2
2
	​

). For V₁=3.0 V → V₂=1.8 V, solving gives C ≈ 17 mF (≈ 20 mF to leave margin).

So a 10–50 mF supercap is a realistic buffer choice for short bursts; thin-film rechargeable cells can be used for longer or repeated ops.

Choose the oscillator carefully

MEMS Super-TCXO (recommended) for low drift & small size. But be conservative on its continuous current draw — assume a few µW.

Add digital temperature compensation (MCU + temp sensor) to reduce required oscillator precision and power.

Duty-cycle & system logic

Keep display, radios, and high-precision sync OFF most of the time.

The oscillator + a tiny MCU in deep sleep do the time counting. Wake only on interaction or when buffer reaches a threshold for sync.

Example duty cycle: oscillator always on, MCU wakes <1% duty, display updates <1% duty.

Hybrid harvesting (strongly recommended)

Add a small solar cell under the dial (translucent or using micro-cells around indices) — 

indoor solar can provide tens to hundreds of µW under bright indoor light; outdoors it’s far greater. This dramatically raises reliability.

Optionally add piezo/kinetic harvesting in the strap for motion energy.

Optional high-precision sync strategy

 To improve CSAC accuracy: only power CSAC episodically after buffer charges. 
 
 CSAC warm-up and run consumes tens–hundreds of mW and is feasible only for very short, infrequent syncs.

More practical: GNSS or BLE time sync for occasional re-calibration (cheaper and lower warm-up than CSAC).

Component selection types

Antenna: printed copper / silver ink textile traces.

Rectifier: low-Vf Schottky diodes or matched diode bridge for low input power.

PMIC: energy-harvesting PMIC with cold-start at low voltage, boost converter.

Buffer: 10–50 mF supercap or thin-film rechargeable cell.

Oscillator: MEMS Super-TCXO.

MCU: ultra-low-power MCU with deep sleep microamp range and RTC tick support.

Display: e-ink or mechanical hands.

Example energy scenarios (how the watch behaves)

Continuous timekeeping only (no display, no sync):

If harvested + solar/kinetic hybrid can deliver ~2–5 µW continuous, the oscillator + MCU in deep sleep can run indefinitely.

Occasional sync (radio/GNSS for ~1–5 seconds):

Buffer charges over hours; when full, wake radio for short sync. This keeps average power low.

Frequent display updates / Bluetooth streaming:

Not feasible on RF only. Hybrid (solar + RF + buffer) might support sparse notifications.

Next practical steps (prototype roadmap)

Build a rectenna test piece: print a strap antenna + simple diode rectifier → measure DC output in realistic settings. (This tells you real harvested power for your location.)

Pick a PMIC & buffer: get an energy-harvest PMIC dev kit and a 22 mF supercap. Test cold-start and charge times.

Choose MEMS oscillator & MCU dev boards: measure the continuous current draw in your target configuration.

Integrate & iterate: connect the harvested DC → PMIC → buffer → DC-DC → oscillator + MCU. Implement duty cycling and a simple sync routine.

Add hybrid solar cell if harvested RF alone is insufficient.
