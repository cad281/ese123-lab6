# ESE 123 Lab 6: Operational Amplifiers

**Interactive page:** https://cad281.github.io/ese123-lab6/

All labs: [Lab 1: DC Power & Ohm's Law](https://cad281.github.io/ese123-lab1/) · [Lab 2: Digital Multimeter](https://cad281.github.io/ese123-lab2/) · [Lab 3: SPICE & Soldering](https://cad281.github.io/ese123-lab3/) · [Lab 4: Function Generator & Scope](https://cad281.github.io/ese123-lab4/) · [Lab 5: Capacitors & Scope](https://cad281.github.io/ese123-lab5/) · [Lab 6: Operational Amplifiers](https://cad281.github.io/ese123-lab6/)

A TA-written guide for ESE 123 students: **what** you do in Lab 6, **why** it matters and **how** to do it. The interactive page animates op-amp feedback, designs your −4.7 amplifier from standard resistor values, simulates the bench scope (including clipping), and models the transimpedance amplifier on your board.

---

## What this lab is about

| You will… | Using… |
|---|---|
| (Pre-lab) Design an **inverting amplifier** with gain −4.7 from standard 5% resistors | E24 value table |
| (Pre-lab) Simulate the project's **non-inverting** amplifier: DC gain and **half-power frequency** | LTspice `.dc` and `.ac` |
| Build and measure the inverting amplifier; find the output **limits** | LM741 on ±15 V, generator, scope |
| Test the board's **transimpedance amplifier** (current in, voltage out) | R11 0.07 Ω, U2, supply as a current source, DMM |
| Solder parts for next week | RP1, RP2, XU1 socket, Q5; remove R1–R5 |

## Why it matters

- **Op amps let resistors set the gain.** The chip's own gain is huge and unreliable, but with feedback the circuit's gain depends only on two resistor values, which are precise and stable.
- Two simple rules (**V+ = V−** and **no input current**) solve almost every op-amp circuit you'll see in this course and in ESE 211/273.
- Your project uses a **transimpedance amplifier** to measure USB charging current. Today you calibrate it: XTP should equal 2.281 Ω × current.
- Every real amplifier has **limits**. Knowing where it clips tells you the largest input you can handle.

## How to do it, step by step

### Pre-lab
1. **Inverting design:** A_V = −R_f/R_in = −4.7. Pick R_in, then R_f = 4.7 × R_in, both standard values between 1 kΩ and 1 MΩ. Example: **R_in = 10 kΩ, R_f = 47 kΩ** (also 1k/4.7k, 100k/470k). Draw and save the schematic.
2. **Non-inverting DC gain:** 1 + R1/R2 = 1 + 150k/4.75k = **32.58** (ignore C1 at DC).
3. **LTspice:** generic `opamp` part, source with AC amplitude 1 mV, directive `.lib opamp.sub` then `.dc V1 -0.2 0.2 0.01`. Cursor at V(in) = 0.1 V → **V(out) ≈ 3.258 V**.
4. Change to `.ac dec 10 0.1 1000`, linear axis, no phase. **V0 ≈ 32.58 mV** at 0.1 Hz; **V_HP = V0/√2 ≈ 23.04 mV**; cursor to where V(out) = V_HP → **≈ 10.6 Hz**.

### Part 1: build the inverting amplifier (steps 1–9)
1. Get an LM741. The **dimple marks pin 1**. Straddle the plugboard groove.
2. Pins: **2 = − input, 3 = + input, 4 = −V supply, 6 = output, 7 = +V supply**; leave 1, 5, 8 unconnected.
3. Supply **channel 2 + → pin 7**, channel 2 − → ground (green). **Channel 3 − → pin 4**, channel 3 + → ground. This makes +15 V and −15 V.
4. Ground → pin 3. **R_in** from the generator's red lead to pin 2; **R_f** from pin 6 to pin 2. Generator black lead to ground.

### Part 2: measurements (steps 10–24)
1. **CH1** on the generator side of R_in (input), **CH2** on pin 6 (output). Grounds to the ground pigtail.
2. Both supply channels **15 V, 100 mA limit**, outputs on. Both should be **CV at about 2 mA**. CC means a wiring fault.
3. Generator: **ramp, 50% symmetry (triangle), 1 kHz, 1 Vpp**. CH1 should show **≈ 2 Vpp** (high-impedance load, as in Lab 4).
4. Add **AC RMS – N Cycles** for CH1 and CH2. Output should be an **inverted** triangle. **Gain = −V_out,rms / V_in,rms ≈ −4.7.** Capture the screen.
5. Raise the generator amplitude until both tips of the output triangle go **flat**. Cursors on the flat spots: **max ≈ +14 V, min ≈ −13 to −14 V**. Back to 1 Vpp, everything off, take it apart.

### Part 3: transimpedance amplifier (steps 25–45)
1. Solder **R11 (0.07 Ω, larger, marked 0.07)**, trim. DMM: **< 2 Ω** between any screw hole and the USB-C shell.
2. Board on the fixture; fixture off, adapter in, fixture on (LED). DMM: **VDD = 5 ± 0.25 V**; **XTP ≈ 1 mV** (offset). Fixture off.
3. USB-to-banana cable: supply channel 2 **+ → USB connector**, **− → GND loop**. Limits **1 V, 250 mA**. Output on: it goes **CC** (R11 is tiny).
4. Fixture on. Record supply current and **XTP** at **250, 500, 750, 1000 mA**. Expect **0.570, 1.140, 1.711, 2.281 V**.
5. Everything off; return the cable.

### Part 4: soldering for next week (steps 46–53)
1. **Cut R1–R5 flush** with the board.
2. **RP1** (1 kΩ SIP): white-dot end in the hole with the white square. Bend the end leads, solder.
3. **RP2** (220 Ω DIP): bend each pin row against the bench until parallel; align the notch; solder.
4. **XU1** (40-pin socket): notch to the outline (pin 1 end); all pins through. **Solder one pin, check it's flat**, then the other 39.
5. **Q5** (2N3906): **flat side matches the outline**, body ~5 mm above the board. One leg, straighten, then the rest. Trim.
6. Clean up, wash hands, **answer the quiz on Brightspace**.

## What you should observe, and why

| Observation | Why it happens |
|---|---|
| Input ≈ 2 Vpp for a 1 Vpp setting | Generator assumes 50 Ω; R_in is kilohms. |
| Output is an inverted, 4.7× bigger triangle | A_V = −R_f/R_in. Feedback keeps V− at 0 V ("virtual ground"). |
| Supplies in CV at ~2 mA | That's the 741's own operating current. CC means a short. |
| Output tips flatten at ≈ +14 / −13.5 V | The op amp can't output beyond its supply limits; the golden rules stop applying. |
| Supply goes CC when driving R11 | 0.07 Ω would need 14 A to reach 1 V, so the current limit takes over. |
| XTP ≈ 2.281 V per amp | 0.07 Ω × 32.58 gain. |
| (Pre-lab) Same R and C give low-pass here, high-pass in Lab 5 | C7 now sits across the feedback resistor: at high f it shorts R12 and the gain drops toward 1. |

## Formula sheet (what, when, example)

### Voltage gain

```
AV = Vout / Vin
```

**Use it when:** Describing any voltage amplifier. Unitless. Negative means the output is inverted.

**Example:** Vin 0.577 V rms, Vout 2.71 V rms, inverted → AV = −4.70.

### Op-amp golden rules (with negative feedback)

```
V+ = V−     Iinto inputs = 0
```

**Use it when:** Solving any op-amp circuit in its linear region. They fail when the output clips.

**Example:** Inverting amp: V+ = 0, so V− = 0 (“virtual ground”).

### Open-loop behaviour

```
Vout = A (V+ − V−),  A ≈ 10⁵ – 10⁶
```

**Use it when:** Understanding why ΔV must be tiny: 10 V out needs only ~10–100 µV in.

**Example:** A = 200 000, Vout = 5 V → ΔV = 25 µV.

### Inverting amplifier

```
AV = − Rf / Rin
```

**Use it when:** Pre-lab design and checking your measured gain. Rf goes from output to −input; Rin from the signal to −input; +input to ground.

**Example:** Rin = 10 kΩ, Rf = 47 kΩ → −4.7.

### Non-inverting amplifier

```
AV = 1 + R1 / R2
```

**Use it when:** Signal into +input; R1 output → −input, R2 −input → ground. Gain ≥ 1, not inverted.

**Example:** R12 = 150 kΩ, R13 = 4.75 kΩ → 1 + 31.58 = 32.58.

### Transimpedance

```
AZ = Vout / Iin   (units: Ω)
VXTP = IUSB · R11 · (1 + R12/R13) = 2.281 Ω · IUSB
```

**Use it when:** Predicting XTP for each test current (steps 40–43).

**Example:** 500 mA → 0.07 × 0.5 = 35 mV → × 32.58 = 1.140 V.

### Sense resistor voltage and power

```
V = I · R11,   P = I² · R11
```

**Use it when:** Checking the input to U2 and that R11 stays within its 3 W rating.

**Example:** 1 A → 70 mV and 0.07 W.

### RMS of a triangle wave

```
Vrms = Vp / √3
```

**Use it when:** Predicting the scope’s AC RMS for the triangle input.

**Example:** 2 Vpp at the input → 1/1.732 = 0.577 V rms.

### Gain from RMS readings

```
AV = ± Vout,rms / Vin,rms  (minus if inverted)
```

**Use it when:** Step 21. RMS is always positive, so add the sign by looking at the waves.

**Example:** 2.712 / 0.577 → −4.70.

### Output limits (saturation)

```
VNEG,max ≤ Vout ≤ VPOS,max  (≈ −13.5 V … +14 V for a 741 on ±15 V)
```

**Use it when:** Step 23 and spotting clipping. Max input before clipping ≈ Vlimit/|AV|.

**Example:** 14 V / 4.7 ≈ 3 V peak at the input before the top flattens.

### Half-power point

```
PHP = P0/2  →  VHP = V0/√2 ≈ 0.707 V0
```

**Use it when:** Characterising a filter (pre-lab steps 10–12). Find where the AC sweep drops to 70.7% of its passband value.

**Example:** V0 = 32.58 mV → VHP = 23.04 mV at ≈ 10.6 Hz.

### Corner frequency of the feedback RC

```
fc ≈ 1 / (2π R12 C7)
```

**Use it when:** Estimating the low-pass corner when the capacitor is across the feedback resistor.

**Example:** 150 kΩ × 100 nF → 10.6 Hz.

### Generator into a high-impedance load

```
Vactual ≈ 2 × Vset
```

**Use it when:** Step 17: the 1 Vpp setting gives about 2 Vpp at Rin.

**Example:** Set 1 Vpp → measure ~2 Vpp on CH1.

### Standard 5% (E24) values

```
1.0 1.1 1.2 1.3 1.5 1.6 1.8 2.0 2.2 2.4 2.7 3.0 3.3 3.6 3.9 4.3 4.7 5.1 5.6 6.2 6.8 7.5 8.2 9.1 × 10ⁿ
```

**Use it when:** Choosing resistor values for the pre-lab design.

**Example:** 47 kΩ is standard (4.7 × 10⁴); 48 kΩ is not.

### Percent error

```
(measured − expected) / expected × 100%
```

**Use it when:** Comparing measured gain with −Rf/Rin and XTP with 2.281 Ω × I.

**Example:** Measured −4.62 vs −4.70 → −1.7%.


## Practice questions

Same concepts and formulas as the Brightspace quiz, with new numbers (not the real quiz questions). Try each one before opening the answer.

**Q1.** Design an inverting amplifier with gain −3.3 using standard values. Give one exact pair.

<details><summary>Show answer</summary>

**Rin = 10 kΩ, Rf = 33 kΩ**. AV = −Rf/Rin. 3.3 is an E24 value, so 10k/33k (or 1k/3.3k, 100k/330k) is exact.

</details>

**Q2.** A non-inverting amplifier has R1 = 39 kΩ (feedback) and R2 = 3.9 kΩ (to ground). What is the gain?

<details><summary>Show answer</summary>

**11**. AV = 1 + R1/R2 = 1 + 39/3.9 = 11.

</details>

**Q3.** An inverting amplifier has Rin = 4.7 kΩ and Rf = 47 kΩ. The input is +0.3 V DC. What is the output?

<details><summary>Show answer</summary>

**−3.0 V**. AV = −47/4.7 = −10. Vout = −10 × 0.3 = −3.0 V.

</details>

**Q4.** An op amp on ±12 V can swing to ±10.5 V. With a gain of −10, what is the largest input peak before clipping?

<details><summary>Show answer</summary>

**1.05 V peak**. |Vin| ≤ 10.5/10 = 1.05 V. Bigger inputs flatten the output’s tips.

</details>

**Q5.** The scope shows CH1 RMS = 0.577 V and CH2 RMS = 2.71 V, and the waves are upside down relative to each other. What gain do you report?

<details><summary>Show answer</summary>

**−4.70**. |AV| = 2.71/0.577 = 4.70. Inverted, so AV = −4.70.

</details>

**Q6.** A transimpedance stage uses a 0.1 Ω sense resistor and a non-inverting gain set by 100 kΩ and 4.7 kΩ. What is AZ, and the output at 600 mA?

<details><summary>Show answer</summary>

**2.23 Ω; 1.34 V**. Gain = 1 + 100/4.7 = 22.28. AZ = 0.1 × 22.28 = 2.228 Ω. Vout = 2.228 × 0.6 = 1.337 V.

</details>

**Q7.** A filter’s passband output is 50 mV. What output marks the half-power frequency?

<details><summary>Show answer</summary>

**35.4 mV**. VHP = V0/√2 = 50/1.414 = 35.4 mV (power is halved, voltage drops to 70.7%).

</details>

**Q8.** A 47 nF capacitor sits across a 220 kΩ feedback resistor. Estimate the corner frequency.

<details><summary>Show answer</summary>

**15.4 Hz**. fc ≈ 1/(2π × 220×10³ × 47×10⁻⁹) = 15.4 Hz.

</details>

**Q9.** How much power does a 0.05 Ω sense resistor dissipate at 2 A?

<details><summary>Show answer</summary>

**0.2 W**. P = I²R = 4 × 0.05 = 0.2 W.

</details>

**Q10.** For the inverting amplifier, the + input is grounded. What voltage do you expect at the − input while it is working linearly?

<details><summary>Show answer</summary>

**0 V (virtual ground)**. Golden rule: V− = V+ = 0 V. That’s why the current through Rin is simply Vin/Rin.

</details>


## Report and quiz guide

- **Pre-lab report:** inverting schematic (4), DC sweep plot (2), calculated gain with work (2), AC sweep plot (2), half-power frequency (2).
- **In lab:** scope capture with both RMS values and your gain; max/min output from cursors; XTP vs current table compared with 2.281 Ω × I.
- **Quiz:** on Brightspace. Use the formula sheet: inverting/non-inverting gain, golden rules, transimpedance, triangle RMS, half-power point, E24 values.

---
*Made for ESE 123 students by a TA. Simulated values are ideal; real parts and the 741 will differ by a few percent.*
