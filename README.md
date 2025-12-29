# IoT Gateway Processor: RTL-to-GDS with Security, Power & Latency Focus

## Overview

This repository contains the complete RTL-to-GDS implementation of an MCU-class System-on-Chip (SoC) designed for IoT gateway applications. The design emphasizes security, power efficiency, and low-latency operation.

### Key Features

- **Processor Core**: RV32I/M (RISC-V 32-bit Integer with Multiplication/Division extensions)
- **Security Peripherals**:
  - AES-128 hardware accelerator for encryption/decryption
  - SHA-256 secure boot hash verification stub
- **Power Domain**: Single clock domain, single voltage domain
- **Target Frequency**: 50–100 MHz
- **Design Goal**: Full ASIC implementation from RTL to GDSII

## Tool Stack

This project uses the Cadence digital and custom design flow:

### Digital Flow
- **Simulation**: Xcelium (RTL and gate-level simulation)
- **Synthesis**: Genus (logic synthesis)
- **Place & Route**: Innovus (physical implementation)
- **Timing Analysis**: Tempus (static timing analysis)
- **Power Analysis**: Voltus (power integrity and analysis)
- **Equivalence Checking**: Conformal (logical equivalence verification)

### Custom/Analog
- **Schematic & Layout**: Virtuoso (for custom macros and memory compilers)

## 4-Week Project Plan Summary

1. **Week 1**: Architecture definition, RTL development, and initial verification
   - Finalize specifications (PDK/node, bus protocol, peripherals)
   - Develop RV32I/M core, AES-128, SHA-256 modules
   - Create directed testbenches and run RTL simulations

2. **Week 2**: Synthesis, DFT insertion, and timing closure
   - Synthesize design with Genus
   - Insert scan chains and ATPG patterns
   - Perform multi-corner timing analysis
   - Run formal verification (Conformal LEC)

3. **Week 3**: Physical implementation and power analysis
   - Floorplanning and placement (Innovus)
   - Clock tree synthesis and routing
   - IR drop and EM analysis (Voltus)
   - Timing closure across PVT corners

4. **Week 4**: Final verification, signoff checks, and GDSII generation
   - Gate-level simulation with back-annotated SDF
   - Security-focused negative testing
   - Final LVS/DRC checks
   - GDSII stream-out

See [docs/project.md](docs/project.md) for detailed milestones and deliverables.

## Quickstart

### Simulation

```bash
# Setup (placeholder - to be filled)
cd tb/
# Run directed tests
# make sim TEST=reset_boot
# make sim TEST=aes_mmio
# make sim TEST=bad_image_rejection
```

### Synthesis

```bash
# Setup (placeholder - to be filled)
cd scripts/
# Run synthesis
# genus -f synthesis_flow.tcl
```

For detailed build and verification instructions, see [docs/checklists.md](docs/checklists.md).

## Repository Structure

```
.
├── README.md              # This file
├── LICENSE                # MIT License
├── docs/                  # Documentation
│   ├── project.md         # Full project plan and specifications
│   └── checklists.md      # Weekly exit criteria and signoff checklist
├── rtl/                   # RTL source files (to be populated)
│   └── .gitkeep
├── tb/                    # Testbenches and verification
│   └── .gitkeep
└── scripts/               # Build and flow automation scripts
    └── .gitkeep
```

## Documentation

- **[Project Plan](docs/project.md)**: Complete 4-week project documentation with architecture, verification plan, implementation flow, and risk mitigation strategies.
- **[Checklists](docs/checklists.md)**: Weekly exit criteria and final signoff-lite checklist.

## Contributing

This is an educational/demonstration project. Contributions, suggestions, and feedback are welcome via issues and pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.