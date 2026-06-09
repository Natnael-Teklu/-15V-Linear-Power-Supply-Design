# ±15V Linear Power Supply Design

**Course:** ENES 205 — Electrical Circuits  
**School:** Howard Community College  
**Author:** Natnael Teklu  
**Semester:** Spring 2026  

---

## Overview

This project is the design and simulation of a high-performance linear power supply that converts standard 120Vac 60Hz wall power into a regulated ±15Vdc output. The design targets low ripple, low noise, and stable output suitable for powering sensitive analog and electronic circuits.

---

## Problem Statement

Design a linear power supply meeting the following specifications:

| Requirement | Target |
|---|---|
| Input | 120Vac, 60Hz |
| Output | ±15Vdc |
| Output accuracy | ±1% |
| Minimum load current | 250mA |
| Output ripple | <0.1% pk-pk |
| Output stability | ±0.01% for >1 hour |
| Transformer type | Toroidal (noise rejection) |

---

## Final Design — Concept C

Three design concepts were evaluated. The selected design (Concept C) uses:

- **Toroidal transformer** modeled as three coupled inductors (L1=9H primary, L2=L3=333mH secondary halves, K=1 coupling coefficient)
- **Full-bridge rectifier** using four 1N4007 diodes
- **LT1963** low-noise positive voltage regulator (1.5A, 75dB PSRR)
- **LT3015** low-noise negative voltage regulator (1.5A, 75dB PSRR)
- **Feedback resistor networks** (R=1.37kΩ, R=121Ω) setting output to ±15V

### Why Concept C was selected over alternatives:
- Concept A (LM317 + two-diode rectifier) only regulated the positive rail
- Concept B (bridge rectifier + LM7815/LM7915) did not satisfy the toroidal transformer requirement
- Concept C satisfies all requirements with superior noise performance and full ±15V regulation

---

## Key Calculations

**Transformer turns ratio:**
```
N = Vsec / Vpri = 18 / 120 = 0.15
```

**Peak rectified voltage:**
```
Vrect = Vsec_peak - 2(0.7V) = 25.46 - 1.4 = 24.06V
```

**Filter capacitor ripple (pre-regulation):**
```
Vripple = Iload / (f × C) = 0.250 / (120 × 0.0047) = 0.443V pk-pk
```

**LT1963 output voltage:**
```
Vout = Vref × (1 + R5/R6) = 1.21 × (1 + 1370/121) = 14.91V ≈ 15V
```

**Output ripple after regulation (75dB PSRR):**
```
Vripple_out = 0.443 / 10^(75/20) = 78.8μV → 0.0005% (well below 0.1% requirement)
```

---

## Simulation Results

Simulated in **LTspice** using transient analysis (.tran 120):

- Positive output: **+15V** (stable flat line)
- Negative output: **−15V** (stable flat line)
- Pre-regulation voltage: **±23V** (sufficient headroom above dropout)

---

## Bill of Materials (Estimated)

| Component | Part | Qty | Unit Cost | Total |
|---|---|---|---|---|
| Toroidal Transformer | Triad VPT24-2080 | 1 | $18.50 | $18.50 |
| Bridge Rectifier Diodes | 1N4007 | 4 | $0.15 | $0.60 |
| Filter Capacitor 4.7mF | Nichicon UVZ | 2 | $2.50 | $5.00 |
| Bypass Capacitor 10pF | Murata GRM | 2 | $0.10 | $0.20 |
| LT1963 Regulator | LT1963AET-15 | 1 | $5.50 | $5.50 |
| LT3015 Regulator | LT3015EDD | 1 | $6.00 | $6.00 |
| Feedback Resistor 1.37kΩ | Yageo | 2 | $0.10 | $0.20 |
| Feedback Resistor 121Ω | Yageo | 2 | $0.10 | $0.20 |
| Output Capacitor 10pF | Murata | 2 | $0.10 | $0.20 |
| Series Resistor 5Ω | Yageo | 2 | $0.15 | $0.30 |
| Fuse 500mA | Littelfuse | 1 | $1.50 | $1.50 |
| PCB Fabrication | OSH Park | 1 | $10.00 | $10.00 |
| **Total** | | | | **$48.20** |

---

## Repository Contents

```
├── README.md
├── schematic/
│   └── power_supply.asc          # LTspice schematic file
├── simulation/
│   └── simulation_results.png    # Transient analysis output
├── report/
│   └── Nteklu_Final_Design_Report.docx
└── poster/
    └── Nteklu_PowerSupply_Poster.pptx
```

---

## Tools Used

- **LTspice** — Schematic capture and simulation
- **Falstad Circuit Simulator** — Initial concept prototyping
- **Microsoft Word** — Design report
- **Microsoft PowerPoint** — Design poster

---

## References

- LT1963 Datasheet: https://www.analog.com/media/en/technical-documentation/data-sheets/1963fc.pdf
- LT3015 Datasheet: https://www.analog.com/media/en/technical-documentation/data-sheets/3015fb.pdf
- 1N4007 Datasheet: https://www.vishay.com/docs/88503/1n4001.pdf
- DigiKey 1N4007: https://www.digikey.com/en/products/detail/onsemi/1N4007/965162
- LTspice: https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html

---

## What I Learned

- How to model a toroidal transformer in LTspice using coupled inductors and the K directive
- How to properly enable voltage regulator shutdown pins (SHDN) in simulation
- The difference between two-diode center-tapped rectifiers and four-diode full-bridge rectifiers
- How to calculate ripple rejection using PSRR and verify against design requirements
- How to select feedback resistors for adjustable voltage regulators (LT1963/LT3015)
