<div align="center">

# CMOS Flash ADC with TMCC-Based Comparator

[![Cadence](https://img.shields.io/badge/Tool-Cadence%20EDA-red.svg)](https://www.cadence.com/)
[![Technology](https://img.shields.io/badge/Process-UMC_65nm-blue.svg)](/)
[![Type](https://img.shields.io/badge/Design-Analog%20IC-green.svg)](/)
[![ADC](https://img.shields.io/badge/Resolution-3--bit-orange.svg)](/)

**Transistor-level schematics for a 3-bit Flash ADC, designed in Cadence Virtuoso**

[Overview](#overview) | [Architecture](#architecture) | [Schematics](#design-stages) | [Specifications](#specifications) | [Repository Contents](#repository-contents) | [Author](#author)

</div>

---

## About

This project was developed at **CSIR-CEERI, Pilani** (April 2025 - June 2025) during an Advanced VLSI Design research program. It is a 3-bit Flash ADC built entirely at the transistor level (no standard cell library) in Cadence Virtuoso.

| | |
|---|---|
| **Institution** | CSIR-CEERI, Pilani |
| **Duration** | April 2025 - June 2025 |
| **Team Size** | 2 |
| **Tools** | Cadence EDA (Virtuoso, ADE L, Spectre) |

## Overview

The design implements a **3-bit Flash Analog-to-Digital Converter** using a **Threshold-Modulated Current Comparator (TMCC)** front end followed by custom transistor-level CMOS logic gates that convert the resulting thermometer code into a 3-bit binary output.

What was built:
- A bank of 7 TMCC comparators to produce a thermometer-coded representation of the analog input
- Custom transistor-level CMOS AND gates to restore the comparator outputs to clean logic levels
- Custom transistor-level CMOS OR gates, combined into an 8-to-3 thermometer-to-binary encoder
- A top-level schematic integrating the comparator array, logic stages, and encoder into a complete Flash ADC

---

## Architecture

<div align="center">
<img src="images/schematics/flash_adc_full.jpeg" alt="Flash ADC Full Schematic" width="800"/>

*Top-level Flash ADC schematic in Cadence Virtuoso*
</div>

### System Block Diagram

```
                                    +------------------+
     Analog Input  ──────────────>  |  TMCC Comparator |
         (Vin)                      |     Array (7x)   |
                                    +--------+---------+
                                             |
                                    Thermometer Code (T1-T7)
                                             |
                                             v
                                    +------------------+
                                    |   AND Gate Array |
                                    | (Level Restore)  |
                                    +--------+---------+
                                             |
                                             v
                                    +------------------+
                                    |  8-to-3 Encoder  |
                                    | (OR Gate Network)|
                                    +--------+---------+
                                             |
                                    Binary Output (B2, B1, B0)
                                             |
                                             v
                                       3-bit Digital
```

---

## Design Stages

### 1. TMCC Comparator Stage

The **Threshold-Modulated Current Comparator (TMCC)** stage compares the analog input against 7 reference thresholds to produce a thermometer-coded output. The comparator schematics themselves are not included as separate images in this repository (see [Repository Contents](#repository-contents)); the comparator array appears as part of the full schematic above.

| Feature | Description |
|---------|-------------|
| **Function** | Analog voltage comparison |
| **Output** | Thermometer-encoded signals |
| **Count** | 7 comparators for 3-bit resolution |

### 2. Logic Conversion Stage

<div align="center">
<img src="images/schematics/7input_and_gate.jpeg" alt="AND Gate" width="700"/>

*Custom transistor-level AND gate used for logic-level restoration*
</div>

- Comparator outputs are converted to valid digital levels using custom CMOS AND gates
- Each gate is built from individual pMOS/nMOS transistors rather than a standard-cell library

### 3. Encoder Stage

<div align="center">
<table>
<tr>
<td><img src="images/schematics/4input_or_gate.jpeg" alt="4-Input OR Gate" width="400"/></td>
<td><img src="images/schematics/8to3_encoder.jpeg" alt="8-to-3 Encoder" width="400"/></td>
</tr>
<tr>
<td align="center"><em>Custom 4-input OR gate (transistor level)</em></td>
<td align="center"><em>8-to-3 thermometer-to-binary encoder, built from three 4-input OR gates</em></td>
</tr>
</table>
</div>

The encoder converts the 7-bit thermometer code into a 3-bit binary output:

| Thermometer Code | Binary Output |
|------------------|---------------|
| 0000000 | 000 |
| 0000001 | 001 |
| 0000011 | 010 |
| 0000111 | 011 |
| 0001111 | 100 |
| 0011111 | 101 |
| 0111111 | 110 |
| 1111111 | 111 |

---

## Top-Level Integration

<div align="center">
<img src="images/schematics/top_level_mux.jpeg" alt="Top Level Integration" width="700"/>

*Top-level schematic combining the AND-gate case logic with the 8-to-3 encoder*
</div>

<div align="center">
<img src="images/schematics/encoder_with_dac.jpeg" alt="Encoder with DAC" width="700"/>

*Encoder section with buffer/inverter stages and a DAC block used for feedback*
</div>

---

## Specifications

| Parameter | Value |
|-----------|-------|
| **Resolution** | 3-bit |
| **Technology** | UMC 65nm CMOS |
| **Supply Voltage** | 1.2 V |
| **Comparator Type** | TMCC (Threshold-Modulated Current) |
| **Logic Style** | Custom transistor-level CMOS |
| **Number of Comparators** | 7 |
| **Quantization Levels** | 8 |

## Tools and Technology

| Category | Specification |
|----------|---------------|
| **Design Environment** | Cadence Virtuoso (ADE L) |
| **Simulation Engine** | Spectre |
| **Technology Node** | UMC 65nm CMOS |
| **Supply Voltage** | 1.2 V |
| **Logic Type** | Custom CMOS (pMOS + nMOS), no standard cells |

---

## Repository Contents

This repository contains schematic screenshots documenting the design, not the underlying Cadence project files. The Virtuoso libraries, netlists, testbenches, and UMC 65nm PDK are not included (institutional/tool licensing).

```
Flash-ADC/
├── README.md
└── images/
    └── schematics/
        ├── flash_adc_full.jpeg      # Top-level ADC schematic
        ├── encoder_with_dac.jpeg    # Encoder section with DAC feedback
        ├── 4input_or_gate.jpeg      # OR gate, transistor level
        ├── 8to3_encoder.jpeg        # Thermometer-to-binary encoder
        ├── 7input_and_gate.jpeg     # AND gate, transistor level
        └── top_level_mux.jpeg       # Top-level integration
```

## Status

- Schematic design is complete for all blocks shown above (comparators, AND-gate level restoration, OR-gate encoder, top-level integration).
- No simulation waveforms, testbench files, or layout are included in this repository.

## Future Enhancements

- [ ] Add physical layout (place & route)
- [ ] Include simulation waveforms and testbench results
- [ ] Extend to higher resolution (4-bit, 6-bit)
- [ ] Power and timing characterization

---

## Author

**Debtonu Bose**
B.Tech Electronics and Communication Engineering
Vellore Institute of Technology (2021-2025)

[![GitHub](https://img.shields.io/badge/GitHub-DarkDragoXE-black?logo=github)](https://github.com/DarkDragoXE)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-debtonu--bose-blue?logo=linkedin)](https://linkedin.com/in/debtonu-bose)

## Acknowledgments

- CSIR-CEERI, Pilani, for access to Cadence EDA tools and research guidance
- Advanced VLSI Design Workshop (March 2025) for foundational training in analog and mixed-signal design
