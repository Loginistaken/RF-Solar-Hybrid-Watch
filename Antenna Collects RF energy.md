RF energy-harvesting system integrated into a self-powered watch.

1️⃣ How Much Energy Radio Waves Carry

Radio waves do carry energy, but the amount available in the environment is usually very small.
Typical power densities:

Urban areas near cell towers: ~0.1–1 milliwatt per square meter (mW/m²).

Wi-Fi hotspots: ~0.01–0.1 mW/m².

Broadcast FM/AM radio: even lower per unit area.

So if your watch had an antenna of, say, 10 cm² (0.001 m²), at 1 mW/m² it would capture ~1 microwatt of power.

For reference:

A quartz watch needs ~1 µW.

A smartwatch needs ~10–100 mW.

This means:

Basic watches could, in theory, run off ambient RF.

Smartwatches would need much stronger RF fields or additional harvesting (like solar or kinetic).

2️⃣ Harnessing This Energy

This is done with RF energy harvesting:

.

Matching circuit: Tunes antenna to specific frequencies for max energy.

Rectifier (diode-based): Converts RF (AC) into DC.

Energy storage: Small capacitor or supercapacitor to buffer energy.

Voltage booster/regulator: Brings the voltage to usable levels for the watch circuit.

This technology already exists in RFID tags, remote sensors, and battery-less IoT devices.

3️⃣ Materials for Each Part of the Watch
Part	Material	Why
Antenna (in the strap or case)	Copper foil, silver ink, or conductive polymer	High conductivity for efficient capture of RF. Flexible or embedded in watch band.
Rectifier Circuit	Schottky diodes (GaAs or silicon), micro-PCBs	Schottky diodes have very low forward voltage, ideal for tiny RF currents.
Energy Storage	Thin-film lithium microbattery or solid-state capacitor (tantalum or ceramic)	Small, safe, high-cycle life.
Voltage Regulator	CMOS low-power regulator IC	Regulates the harvested voltage to ~1.5–3V for the quartz movement.
Watch Movement	Ultra-low-power quartz movement	Consumes ≤1 µW, ideal for harvested energy.
Display	E-ink or mechanical hands	E-ink uses negligible power; hands powered by quartz motor use low pulses.
Case	Stainless steel or ceramic with non-metallic window for RF	RF transparent materials (ceramic, polymer) allow better reception.
