# Snap Fit Charger

**Ultra-Compact Modular USB-C Charger with Isolated Flyback Topology**

![Status](https://img.shields.io/badge/status-MVP%20Development-yellow)
![Power](https://img.shields.io/badge/power-20W%20USB--PD-blue)
![Safety](https://img.shields.io/badge/safety-IEC%2062368--1-green)

---

## 📋 Project Overview

Snap Fit Charger is an innovative ultra-compact AC-DC power adapter designed for maximum portability and universal compatibility. The project combines cutting-edge power electronics with modular snap-fit mechanical design to create a truly versatile charging solution.

**Key Innovation**: Snap-fit modular plug system allows travelers to swap regional adapters without carrying multiple chargers.

### Target Specifications

- **Input**: 90–264 V AC, 50/60 Hz (Universal)
- **Output**: USB-C with USB-PD 3.0
  - 5V @ 3A (15W)
  - 9V @ 2.22A (20W)
  - 12V @ 1.67A (20W)
- **Maximum Power**: 20W
- **Topology**: High-frequency isolated Flyback converter
- **Dimensions**: Target < 40mm × 40mm × 25mm
- **Efficiency**: >85% at rated load
- **Safety**: IEC 62368-1 compliant

---

## 🎯 Problem Statement

**Current Pain Points:**
1. Travelers need multiple chargers for different regions
2. Bulky chargers occupy premium luggage space
3. Proprietary charging standards create e-waste
4. Non-modular designs cannot be repaired

**Our Solution:**
A palm-sized, modular USB-C charger with interchangeable snap-fit plug adapters, universal input voltage, and compliance with USB Power Delivery standards.

---

## 🏗️ Repository Structure

```
snap-fit-charger/
├── README.md                          # This file
├── LICENSE                            # MIT License
│
├── hardware/
│   ├── pcb/
│   │   ├── README.md                  # PCB design overview
│   │   ├── schematic/
│   │   │   ├── snapfit_schematic.pdf
│   │   │   ├── snapfit_schematic.kicad_sch
│   │   │   └── block_diagram.png
│   │   ├── layout/
│   │   │   ├── snapfit_layout.kicad_pcb
│   │   │   ├── gerbers/
│   │   │   └── 3d_view.png
│   │   ├── bom/
│   │   │   ├── mvp_charger_power_bom.csv
│   │   │   └── sourcing_notes.md
│   │   └── design_rules/
│   │       ├── creepage_clearance.md
│   │       └── thermal_design.md
│   │
│   └── mechanical/
│       ├── enclosure/
│       │   ├── snapfit_case_top.step
│       │   ├── snapfit_case_bottom.step
│       │   └── assembly.pdf
│       └── plug_adapters/
│           ├── type_a_us.step           # US/North America
│           ├── type_c_eu.step           # Europe
│           ├── type_g_uk.step           # UK
│           └── snap_mechanism.pdf
│
├── firmware/
│   ├── usb_pd_config/
│   │   ├── stusb4500_config.h
│   │   └── pd_profiles.c
│   └── README.md
│
├── docs/
│   ├── standards/
│   │   ├── IEC_62368-1_summary.md
│   │   ├── USB_PD_3.0_spec.pdf
│   │   └── EMC_requirements.md
│   ├── design_notes/
│   │   ├── architecture_overview.md
│   │   ├── component_selection.md
│   │   └── power_budget.xlsx
│   ├── user_manual/
│   │   ├── quick_start.md
│   │   └── safety_warnings.md
│   └── competition/
│       └── honpe_challenge_submission.md
│
├── tests/
│   ├── electrical/
│   │   ├── efficiency_test.md
│   │   ├── load_regulation.md
│   │   └── test_results.csv
│   ├── thermal/
│   │   ├── thermal_test_plan.md
│   │   └── ir_images/
│   ├── emc/
│   │   └── conducted_emissions.md
│   └── safety/
│       ├── hipot_test.md
│       └── insulation_resistance.md
│
├── manufacturing/
│   ├── assembly_instructions.md
│   ├── pcb_stackup.pdf
│   ├── pick_and_place/
│   └── inspection_checklist.md
│
└── media/
    ├── renders/
    │   ├── product_hero.png
    │   └── exploded_view.png
    ├── photos/
    └── videos/
```

---

## 🔧 Hardware Architecture

### Power Stage Blocks

1. **AC Input & EMI Filter**
   - SMD Fuse (F1) - 2A/250V
   - MOV (MOV1) - 275VAC surge protection
   - X-cap (CX1) + Y-caps (CY1, CY2) - EMI suppression
   - Common-mode choke (L1) - Differential/common-mode attenuation

2. **Primary Flyback Converter**
   - Controller IC (U1): HF500-15 (PSR with integrated MOSFET)
   - Startup network: R1, C3, C4
   - Primary winding of T1

3. **Isolation Transformer**
   - Planar transformer (T1) - 1500Vrms reinforced insulation
   - Centered placement for optimal creepage
   - Isolation slots in PCB

4. **Secondary Synchronous Rectification**
   - SR Controller (U2): UCC24610D
   - Low Rds(on) MOSFET (Q1)
   - LC output filter: L2 (4.7µH) + C5/C6 (22µF each)
   - Feedback divider: R2, R3

5. **USB-PD Controller & Output**
   - USB-PD IC (U3): STUSB4500QTR
   - CC line resistors (R4, R5) - Profile detection
   - Output filtering: C7, C8
   - USB-C connector (J1)

### PCB Stack-up (4 layers)

- **Layer 1**: Primary-side signals (high voltage)
- **Layer 2**: Primary ground plane (isolated)
- **Layer 3**: Secondary ground plane
- **Layer 4**: Secondary-side signals (low voltage)

**Isolation**: ≥3.2mm creepage/clearance between primary and secondary

---

## 🎨 Mechanical Innovation

### Snap-Fit Plug System

The modular plug adapter system allows users to:
- Swap plug types in seconds without tools
- Carry only needed regional adapters
- Replace damaged plugs individually
- Reduce e-waste by keeping the power module

**Mechanism Features:**
- Spring-loaded locking tabs
- Polarized alignment guides
- Gold-plated AC contacts rated for 10,000+ insertions
- Compact storage: adapters stack together

---

## 📊 Bill of Materials (Key Components)

| Ref | Component | Part Number | Qty | Function |
|-----|-----------|-------------|-----|----------|
| U1 | Flyback Controller | HF500-15 | 1 | Primary-side regulation |
| U2 | SR Controller | UCC24610D | 1 | Synchronous rectification |
| U3 | USB-PD Controller | STUSB4500QTR | 1 | Power delivery negotiation |
| T1 | Planar Transformer | Custom | 1 | Galvanic isolation |
| Q1 | Sync Rectifier FET | TBD (30V, <10mΩ) | 1 | Secondary rectification |
| L1 | Common-Mode Choke | TBD | 1 | EMI filtering |
| L2 | Output Inductor | 4.7µH, 3A | 1 | Output filtering |
| J1 | USB-C Connector | GCT USB4105 | 1 | Power output |

*Full BOM available in `/hardware/pcb/bom/`*

---

## 🚀 Development Roadmap

### Phase 1: MVP (Current)
- [x] Initial schematic design
- [x] Component selection
- [ ] PCB layout (4-layer)
- [ ] Design rule verification
- [ ] Thermal simulation

### Phase 2: Prototype
- [ ] PCB fabrication (5 units)
- [ ] Component procurement
- [ ] Assembly and bring-up
- [ ] Basic electrical testing
- [ ] Snap-fit enclosure 3D printing

### Phase 3: Validation
- [ ] Full electrical characterization
- [ ] Thermal testing
- [ ] Pre-compliance EMC testing
- [ ] Safety testing (hipot, insulation)
- [ ] Mechanical durability testing

### Phase 4: Production Readiness
- [ ] Design optimization based on tests
- [ ] Certification (CE, FCC, UL)
- [ ] Manufacturing partner selection
- [ ] Tooling for injection molding
- [ ] Pilot production run

---

## 🧪 Testing Strategy

### Electrical Tests
- Input voltage range (85-270V AC)
- Load regulation (0-100% load)
- Line regulation
- Efficiency mapping
- Transient response
- Short circuit protection

### Thermal Tests
- Ambient temperature rise testing
- Component temperature mapping (IR camera)
- Derating verification
- Continuous operation stress test

### Safety & Compliance
- Insulation resistance (>5MΩ)
- Hipot testing (primary-secondary, 3000V AC)
- Creepage/clearance verification
- Flammability (UL94 V-0)

### EMC Tests
- Conducted emissions (CISPR 32)
- Radiated emissions
- ESD immunity
- Surge immunity

---

## 🎓 Design for Honpe Challenge

This project is being developed for submission to the **Honpe Challenge 2026** - Consumer Device Innovation Competition.

### Competition Alignment

**Problem Identified**: Travel inconvenience and e-waste from non-modular chargers

**Target User**: 
- International travelers
- Digital nomads
- Students studying abroad
- Business professionals

**Context of Use**: 
- Hotels, airports, cafés, coworking spaces
- Multiple devices per user (phone, tablet, laptop accessories)
- Frequent regional travel

**Device Objective**: 
Provide a single, compact charging solution that adapts to any region through snap-fit modularity while maintaining safety and efficiency.

**Innovation Points**:
1. Snap-fit modular plug system (mechanical)
2. Ultra-compact flyback topology (electrical)
3. Universal input voltage (90-264V AC)
4. USB-PD compatibility (software)

*See `/docs/competition/` for full submission materials*

---

## 👥 Team

**Project Lead**: [Your Name]
- University: [Your University]
- Major: [Your Major]
- Email: [Your Email]

**Team Members**:
- [Member 2 Name] - [Role]
- [Member 3 Name] - [Role]

---

## 📚 References & Standards

- IEC 62368-1: Audio/video, information and communication technology equipment - Safety requirements
- USB Power Delivery Specification Rev 3.0
- CISPR 32: Electromagnetic compatibility of multimedia equipment
- UL 60950-1: Safety of information technology equipment
- Power Integrations Application Notes
- Texas Instruments Synchronous Rectification Design Guide

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Note**: This is an open hardware project. Commercial use requires proper safety certification and compliance testing.

---

## 🤝 Contributing

We welcome contributions! Areas where help is needed:
- PCB layout review
- Thermal simulation
- Mechanical design optimization
- Documentation improvements
- Testing and validation

Please read our contribution guidelines before submitting pull requests.

---

## 📞 Contact

**Project Email**: [your-email@example.com]

**Honpe Challenge Contact**: honpechallenge@honpe.mx

**Repository**: [GitHub/GitLab URL]

---

## ⚠️ Safety Warning

**This device operates with potentially lethal AC voltages.**

- Only qualified personnel should attempt to build or modify this design
- Always use isolation transformers during testing
- Ensure proper enclosure and insulation before connecting to mains
- Follow all applicable electrical safety codes and regulations
- This design is provided for educational purposes - commercial use requires full compliance testing and certification

---

## 🙏 Acknowledgments

- Honpe Prototyping México for organizing the challenge
- Power electronics community for open-source references
- Component manufacturers for detailed datasheets and application notes

---

**Last Updated**: October 2025  
**Project Status**: MVP Development Phase  
**Next Milestone**: PCB Layout Completion
