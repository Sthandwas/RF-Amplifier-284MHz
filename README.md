<!--
Suggested repo name: RF-Amplifier-284MHz
GitHub "About" line: Single-stage 284 MHz narrowband RF amplifier (BFP420, 50 Ω): AWR simulation, PCB build and VNA measurements.
Topics: rf-engineering, rf-amplifier, cadence-awr, bfp420, electronics, pcb, analog-design
Put the three images in an /images folder in the repo.
-->

# Narrowband RF Amplifier (284 MHz)

A single-stage narrowband RF amplifier designed for a 50 Ω system at 284 MHz using an Infineon BFP420 transistor. The design was simulated in AWR Design Environment, built on a PCB and measured with a vector network analyser (VNA).

Course project: RF Engineering 3B (RFEB 301), Practical Assessment 2, Department of Electronic and Computer Engineering, Durban University of Technology, October 2026.

## Results

| Parameter | Target | AWR simulation | Measured (VNA) | Target met? |
|---|---|---|---|---|
| S21 forward gain | ≥ 22 dB | 26.75 dB | 21.07 dB | No (0.93 dB short) |
| S11 input reflection | ≤ −10 dB | −27.32 dB | −16.16 dB | Yes |
| S22 output reflection | ≤ −10 dB | −28.37 dB | −16.17 dB | Yes |
| Rollett stability factor K | > 1 | 1.2012 | not measured | Yes (simulated) |

The built amplifier met both matching targets but measured 21.07 dB of gain, 5.68 dB below the simulation and 0.93 dB below the 22 dB target. Possible causes are PCB track losses, component tolerances, soldering, and differences between real components and the models used in AWR. These are untested hypotheses and would need further measurement to confirm.

Measurements were taken with an E5063A VNA centred at 284 MHz with a span of about 400 MHz.

![AWR simulated frequency response](images/AWR-Simulated-Frequency-Response.png)

## Design summary

**Transistor and bias point.** BFP420 biased at VCE = 2.5 V and IC = 8 mA from a 5 V supply, using the manufacturer's S-parameter file for that operating point.

**Stability.** The transistor alone was potentially unstable at 284 MHz (K = 0.150). A shunt resistor from the collector to ground was tuned in AWR until K exceeded 1 (marginal stability at 97 Ω). Since 97 Ω is not an E12 value and 100 Ω gives K below 1, an 82 Ω resistor was used. This gave K = 1.158 and a maximum available gain of 27.07 dB. After adding the bias circuit, K rose to 1.2012 and the determinant condition was also met, so the amplifier is simulated to be unconditionally stable at 284 MHz.

**Matching.** L-network input and output matching designed in AWR for source and load reflection coefficients ΓS = 0.850 ∠ 38.5° and ΓL = 0.613 ∠ 71.4°.

| Component | Role | Value |
|---|---|---|
| C1 | Input series matching and DC block | 3.55 pF |
| L1 | Input shunt matching | 44.19 nH |
| L2 | Output shunt matching and RF choke (feeds collector DC) | 24.68 nH |
| C2 | Output series matching and DC block | 8.28 pF |
| R1 | Stability resistor (collector to ground) | 82 Ω |

![AWR amplifier circuit](images/AWR-Amplifier-Circuit.png)

**Bias network.** Collector-feedback bias with R2 = 2.2 kΩ, R3 = 18 kΩ, R4 = 120 Ω and R5 = 270 Ω. Two 1000 pF capacitors are used: C3 in series with L1 so DC does not flow to ground through it, and C4 with R5 to decouple the supply line.

![DC bias circuit](images/DC-Bias-Circuit.png)

## Limitations and future work

- Stability was verified in simulation at 284 MHz only. A VNA gain and return-loss measurement does not confirm stability of the physical amplifier, and stability checks across the full operating range would be needed.
- Include the actual component models (rather than ideal parts) in AWR to close the gap between simulated and measured gain.
- Check the PCB tracks and layout for losses, and consider an RF shield to reduce radiation.

<!-- TODO: add a photo of the built PCB and the VNA screenshot (images/pcb-photo.jpg, images/vna-s21.png) and embed them here with ![...](images/...) -->

## Tools

AWR Design Environment (simulation), E5063A vector network analyser (measurement), PCB fabrication.

## Reference

S. D. MacPherson, *An Introduction to Linear Radio Frequency Design*, 2020.

## Author

Nontsikelelo Fundiswa Linda, BEng Tech Electronic and Computer Engineering, Durban University of Technology.
