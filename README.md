# LiPo Battery Charger (KiCad)

> A 1S–4S Lithium-Polymer / Li-Ion **buck-boost battery charger**, designed in **KiCad 9**
> around the **Texas Instruments BQ25703A** SMBus charge controller.

[![EDA](https://img.shields.io/badge/EDA-KiCad%209-blue.svg)](https://www.kicad.org/)
[![Charge Controller](https://img.shields.io/badge/IC-TI%20BQ25703A-red.svg)](https://www.ti.com/product/BQ25703A)
[![Cells](https://img.shields.io/badge/Cells-1S%E2%80%934S-green.svg)]()
[![Status](https://img.shields.io/badge/Status-Schematic%20Complete-yellow.svg)](#-project-status)
[![License](https://img.shields.io/badge/License-Unspecified-lightgrey.svg)](#-license)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Hardware Specifications](#-hardware-specifications)
- [Power Stage & Signals](#-power-stage--signals)
- [Bill of Materials Highlights](#-bill-of-materials-highlights)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)
- [Project Status](#-project-status)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)

---

## 🔋 Overview

This repository contains a **KiCad 9** electronics design project for a multi-cell
Lithium-Polymer battery charger. The design is built around the **Texas Instruments
BQ25703A** — a **buck-boost, NVDC battery charge controller** with an **SMBus** host
interface, system power monitor (PSYS), and processor-hot (PROCHOT) output.

Because the BQ25703A uses a **buck-boost** power stage, the charger can operate when the
input voltage is **above, below, or equal to** the battery-pack voltage, making it suitable
for adapter inputs charging 1- to 4-cell packs. The project includes the schematic, custom
KiCad symbol and footprint libraries, and the bill of materials.

---

## ✨ Key Features

- ⚡ **Buck-Boost charging** — works with `V_IN` above, below, or equal to the battery voltage
- 🔢 **1S–4S** Li-Ion / Li-Polymer cell support
- 🖧 **SMBus / I²C control** (`SDA` / `SCL`) — programmable charge current and voltage limits
- 🔄 **OTG / boost output** (`EN_OTG`) — reverse power delivery from battery to system
- 🛡️ **NVDC power path** with system rail (`VSYS`) regulation
- 📊 **`PROCHOT_N`** processor-throttle and **`CHRG_OK`** status outputs
- 🔌 **+24 V class input** with full discrete N-/P-MOSFET power stage and inductor
- 🧩 **Self-contained KiCad libraries** — custom symbol + 32-WQFN footprint included

---

## 📐 Hardware Specifications

| Parameter | Value |
|---|---|
| Charge Controller | Texas Instruments **BQ25703ARSNR** |
| Package / Footprint | 32-WQFN (`RSN0032B`, 4 mm × 4 mm) |
| Topology | Synchronous Buck-Boost (NVDC) |
| Battery Chemistry | Li-Ion / Li-Polymer |
| Cell Configuration | 1S – 4S |
| Host Interface | SMBus / I²C |
| Nominal Input | +24 V rail (see schematic for range) |
| Power Inductor | 2.2 µH |
| Current-Sense Resistors | 10 mΩ (input/charge sense) |
| EDA Tool | KiCad 9 |

> Specifications reflect the schematic component values and the BQ25703A datasheet. Verify
> against the schematic and TI documentation before fabrication.

---

## 🔌 Power Stage & Signals

**Power stage** — discrete buck-boost converter driven by the BQ25703A:
- Multiple **N-channel MOSFETs** (high/low side switches) + **P-channel MOSFETs** (power-path / ACFET-RBFET)
- **2.2 µH** power inductor
- **10 mΩ** current-sense resistors for input and charge current regulation

**Key nets / signals:**

| Net | Description |
|---|---|
| `+24V` | Adapter / input supply rail |
| `VSYS` | Regulated system output rail |
| `BAT` | Battery pack connection |
| `SDA` / `SCL` | SMBus / I²C host communication |
| `EN_OTG` | Enable On-The-Go (boost / reverse) mode |
| `CHRG_OK` | Charge-good status output |
| `PROCHOT_N` | Processor-hot (active-low) throttle output |
| `ILIM_HIZ` | Input current limit / Hi-Z control |
| `CMPOUT` | Independent comparator output |
| `VDDA` | Analog supply |

---

## 🧾 Bill of Materials Highlights

The full BOM is maintained in **`BOM.xlsx`**. Key components include:

| Designator | Value / Part | Function |
|---|---|---|
| U_charger | `BQ25703ARSNR` | Buck-boost SMBus charge controller (32-WQFN) |
| Q (NMOS ×5) | N-Channel MOSFET | Buck-boost power switches |
| Q (PMOS ×2) | P-Channel MOSFET | Input / power-path FETs |
| L | 2.2 µH | Power inductor |
| R_sense | 10 mΩ | Input / charge current sense |
| C bulk | 10 µF (×17) | Input / output bulk decoupling |

---

## 📂 Repository Structure

```
Lipo_Charger/
├── lipo_Charger.kicad_pro            # KiCad project file (open this first)
├── lipo_Charger.kicad_sch            # Schematic
├── lipo_Charger.kicad_pcb            # PCB layout (not yet started)
├── Lipo_charger_sym.kicad_sym        # Custom schematic symbol library
├── Lipo_Charger_footprint.pretty/    # Custom footprint library
│   └── RSN0032B(1).kicad_mod         # BQ25703A 32-WQFN footprint
├── sym-lib-table                     # Project symbol library table
├── fp-lib-table                      # Project footprint library table
├── BOM.xlsx                          # Bill of Materials
├── doc/                              # Project documentation
│   └── README.md
└── lipo_Charger-backups/             # KiCad auto-save archives
```

---

## 🚀 Getting Started

### Prerequisites

- **[KiCad 9](https://www.kicad.org/)** or newer
- Custom symbol and footprint libraries are bundled and referenced via the project-local
  `sym-lib-table` / `fp-lib-table` — no manual library setup is required.

### Open the Project

1. Clone the repository:
   ```bash
   git clone https://github.com/kkjjkamal123/Lipo_Charger.git
   ```
2. Open **`lipo_Charger.kicad_pro`** in KiCad.
3. Launch the **Schematic Editor** to review the design.
4. The **PCB Editor** is currently empty — see [Project Status](#-project-status).

> ⚠️ **Safety note:** Lithium-polymer cells can be hazardous if mishandled. Validate the charge
> current, termination voltage, and cell count against your specific battery pack before powering
> a populated board.

---

## 📊 Project Status

| Stage | Status |
|---|---|
| Schematic | ✅ Complete |
| Symbol & footprint libraries | ✅ Complete |
| Bill of Materials | ✅ Drafted (`BOM.xlsx`) |
| PCB layout | ⬜ Not started |
| Manufacturing outputs (Gerbers) | ⬜ Pending layout |
| Hardware bring-up | ⬜ Pending |

---

## 📄 License

No license file is currently included in this repository. Until one is added, all rights are
reserved by the author. Consider adding **MIT** (for docs/software) or **CERN-OHL** (for the
hardware design) if you intend this to be open hardware.

---

## 🙏 Acknowledgements

- **Texas Instruments** — BQ25703A charge controller and reference documentation
- Built with **[KiCad](https://www.kicad.org/)**, the open-source EDA suite

---

<div align="center">
  <sub>Designed with ⚡ in KiCad 9</sub>
</div>
