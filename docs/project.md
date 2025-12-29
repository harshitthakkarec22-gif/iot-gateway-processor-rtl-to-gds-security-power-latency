# IoT Gateway Processor - Complete Project Documentation

## Summary

This document outlines the complete 4-week project plan for designing, verifying, and implementing an MCU-class System-on-Chip (SoC) from RTL to GDSII. The design targets IoT gateway applications with an emphasis on security features, power efficiency, and low-latency operation.

**Project Scope:**
- RV32I/M processor core (RISC-V 32-bit with Integer and Multiply/Divide extensions)
- AES-128 hardware encryption/decryption peripheral
- SHA-256 secure boot hash verification stub
- Single clock domain, single voltage domain
- Target operating frequency: 50–100 MHz
- Complete ASIC flow using Cadence tool suite

## Architecture

### System Overview

The IoT Gateway Processor SoC consists of the following major components:

#### 1. Processor Core
- **ISA**: RV32I/M (RISC-V base integer instruction set with M extension)
- **Pipeline**: [TO-DO: Specify pipeline depth - e.g., 3-stage, 5-stage]
- **Features**: 
  - 32 general-purpose registers (x0-x31)
  - Program counter and control/status registers
  - Integer arithmetic, logical, and control flow instructions
  - Hardware multiply/divide support (M extension)

#### 2. Memory Subsystem
- **Instruction Memory**: [TO-DO: Specify SRAM size and availability - e.g., 32KB SRAM]
- **Data Memory**: [TO-DO: Specify SRAM size and availability - e.g., 16KB SRAM]
- **Boot ROM**: Small ROM for secure boot sequence initialization
- **Memory Interface**: [TO-DO: Specify bus type - e.g., AHB-Lite, APB, or custom]

#### 3. Security Peripherals

##### AES-128 Hardware Accelerator
- **Algorithm**: AES (Advanced Encryption Standard) with 128-bit key
- **Modes**: ECB (Electronic Codebook) mode, extensible to CBC/CTR
- **Interface**: Memory-mapped I/O (MMIO) registers
- **Registers**:
  - Key input registers (128-bit)
  - Data input/output registers (128-bit blocks)
  - Control register (encrypt/decrypt, start, done status)
- **Performance Target**: Single-block encryption/decryption within specified cycle budget

##### SHA-256 Secure Boot Stub
- **Algorithm**: SHA-256 cryptographic hash function
- **Purpose**: Verify firmware image integrity during boot sequence
- **Implementation**: Stub/placeholder for secure boot verification flow
- **Registers**:
  - Message input registers
  - Hash output registers (256-bit)
  - Control/status registers

#### 4. System Integration
- **Bus Architecture**: [TO-DO: Select bus protocol - AHB-Lite, APB, Wishbone, or custom]
- **Clock Domain**: Single global clock
- **Voltage Domain**: Single voltage supply
- **Reset Strategy**: Synchronous reset with proper assertion/deassertion
- **Interrupt Controller**: [TO-DO: Specify if needed and configuration]

#### 5. I/O and Peripherals
- **GPIO**: [TO-DO: Specify number of GPIO pins if required]
- **UART**: [TO-DO: Specify if UART is included for debug/communication]
- **Additional Peripherals**: [TO-DO: Define any additional required peripherals]

### Block Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     IoT Gateway SoC                         │
│                                                             │
│  ┌──────────────┐          ┌────────────┐                 │
│  │   RV32I/M    │◄────────►│  Inst Mem  │                 │
│  │   Core       │          │  (SRAM)    │                 │
│  └──────┬───────┘          └────────────┘                 │
│         │                                                  │
│         │ [TO-DO: Bus Type]                               │
│         │                                                  │
│  ┌──────┴───────────────────────────────────┐            │
│  │         System Bus / Interconnect         │            │
│  └──┬────────┬───────────┬──────────┬───────┘            │
│     │        │           │          │                     │
│ ┌───▼───┐ ┌─▼────────┐ ┌▼────────┐ ┌▼──────────┐        │
│ │ Data  │ │  AES-128 │ │ SHA-256 │ │   Boot    │        │
│ │ Mem   │ │  Accel.  │ │  Stub   │ │   ROM     │        │
│ │(SRAM) │ │  (MMIO)  │ │  (MMIO) │ │           │        │
│ └───────┘ └──────────┘ └─────────┘ └───────────┘        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Verification Plan

### Verification Strategy

The verification approach combines directed testing, assertions, and coverage-driven verification to ensure functional correctness and security properties.

#### 1. Directed Tests

**Core Functionality:**
- **reset_boot_test**: Verify proper reset behavior and boot sequence
  - Assert all registers initialize to known states
  - Confirm program counter starts at reset vector
  - Verify boot ROM access and initial instruction fetch

- **instruction_suite_test**: Execute representative instruction sequences
  - RV32I base instructions (ADD, SUB, AND, OR, XOR, shifts, etc.)
  - M extension instructions (MUL, MULH, DIV, REM)
  - Load/store operations (LW, SW, LH, SH, LB, SB)
  - Branch and jump instructions (BEQ, BNE, JAL, JALR)

- **memory_access_test**: Validate memory subsystem
  - Instruction and data memory read/write operations
  - Address boundary conditions
  - Access timing and handshaking

**AES-128 Peripheral Tests:**
- **aes_mmio_test**: Memory-mapped I/O register access
  - Write keys to AES key registers
  - Write plaintext to input registers
  - Read ciphertext from output registers
  - Verify control register behavior

- **aes_encryption_test**: Functional encryption validation
  - Use known plaintext/key pairs with expected ciphertext
  - Test multiple data blocks
  - Verify encryption completes within cycle budget

- **aes_decryption_test**: Functional decryption validation
  - Use known ciphertext/key pairs with expected plaintext
  - Verify inverse operation correctness

**SHA-256 Secure Boot Tests:**
- **sha256_hash_test**: Hash computation validation
  - Compute hash of known messages
  - Compare against expected SHA-256 outputs
  - Test various message lengths

- **secure_boot_pass_test**: Valid firmware image verification
  - Load valid firmware image with correct hash
  - Verify boot sequence proceeds successfully

- **bad_image_rejection_test**: Invalid firmware rejection
  - Load corrupted or tampered firmware image
  - Verify boot sequence halts or enters safe state
  - Ensure processor does not execute untrusted code

#### 2. Assertions

**Protocol Assertions:**
- Bus protocol compliance (handshake signals, valid states)
- Memory access timing constraints
- Register read/write sequencing

**Security Assertions:**
- Key registers are write-protected during operation
- Sensitive data paths do not leak information
- Boot verification completes before code execution

**Functional Assertions:**
- No illegal instruction decoding
- ALU operations produce correct results
- Branch/jump target address calculations are valid

#### 3. Coverage Metrics

- **Code Coverage**: Line, branch, and FSM state coverage targets >95%
- **Functional Coverage**: Instruction mix, corner cases, security scenarios
- **Toggle Coverage**: Signal activity on critical paths

### Testbench Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Testbench (tb/)                      │
│                                                         │
│  ┌────────────┐    ┌──────────────┐    ┌────────────┐ │
│  │  Stimulus  │───►│  DUT (SoC)   │───►│  Checker   │ │
│  │  Generator │    │              │    │  Scoreboard│ │
│  └────────────┘    └──────────────┘    └────────────┘ │
│                                                         │
│  ┌────────────────────────────────────────────────┐   │
│  │  Assertions (SVA) & Coverage Monitors          │   │
│  └────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

## Implementation Flow

### Week 1: Architecture & RTL Development

**Objectives:**
- Finalize system specifications and architecture
- Develop RTL for core and peripherals
- Create initial testbenches
- Run RTL simulations

**Activities:**
1. **Specification Completion:**
   - [TO-DO: Select PDK/technology node - e.g., 28nm, 22nm, 14nm]
   - [TO-DO: Define target frequency - 50MHz, 75MHz, or 100MHz]
   - [TO-DO: Choose bus protocol - AHB-Lite, APB, or custom]
   - [TO-DO: Confirm SRAM compiler availability and sizes]
   - [TO-DO: Define DFT strategy - scan chains, MBIST, etc.]
   - [TO-DO: List additional peripherals beyond AES-128 and SHA-256]

2. **RTL Coding (rtl/):**
   - RV32I/M core implementation
   - AES-128 encryption/decryption module
   - SHA-256 hash computation stub
   - Memory controllers and bus interconnect
   - Top-level SoC integration

3. **Testbench Development (tb/):**
   - Directed test cases (reset, instructions, AES, SHA-256)
   - Bus functional models (BFMs) for memory
   - Assertion modules
   - Coverage collectors

4. **RTL Simulation:**
   - Use Cadence Xcelium for functional simulation
   - Run all directed tests
   - Debug and iterate on RTL
   - Achieve initial functional correctness

**Deliverables:**
- Complete RTL codebase
- Directed testbenches with passing tests
- Simulation log reports
- Initial coverage metrics

### Week 2: Synthesis & DFT

**Objectives:**
- Synthesize RTL to gate-level netlist
- Insert Design-for-Test (DFT) structures
- Perform timing analysis
- Run formal verification

**Activities:**
1. **Synthesis (Cadence Genus):**
   - Read RTL and technology libraries
   - Set design constraints (clock period, input/output delays)
   - Optimize for area, power, and timing
   - Generate gate-level netlist

2. **DFT Insertion:**
   - [TO-DO: Specify DFT approach - full scan, partial scan, MBIST]
   - Insert scan chains for testability
   - Generate ATPG patterns for manufacturing test
   - Verify scan chain integrity

3. **Static Timing Analysis (Cadence Tempus):**
   - Multi-corner analysis (SS, TT, FF corners)
   - Setup and hold time verification
   - Clock domain crossing checks
   - Timing path optimization (TNS = 0, WNS = 0)

4. **Formal Verification (Cadence Conformal):**
   - Logical Equivalence Checking (LEC)
   - Compare RTL to synthesized netlist
   - Ensure functional equivalence

**Deliverables:**
- Synthesized gate-level netlist
- DFT-ready design with scan chains
- Timing reports (setup/hold, all corners)
- LEC passing report

### Week 3: Physical Implementation & Power Analysis

**Objectives:**
- Floorplan and place-and-route the design
- Perform clock tree synthesis
- Analyze power integrity and IR drop
- Close timing across all corners

**Activities:**
1. **Floorplanning (Cadence Innovus):**
   - Define die size and aspect ratio
   - Place hard macros (SRAM blocks)
   - Power grid planning
   - I/O pad placement

2. **Placement & Routing:**
   - Standard cell placement
   - Clock tree synthesis (CTS)
   - Global and detailed routing
   - Filler cell insertion

3. **Power Analysis (Cadence Voltus):**
   - Static and dynamic power estimation
   - IR drop analysis on power grid
   - Electromigration (EM) checks
   - Power optimization if needed

4. **Timing Closure:**
   - Post-route STA with parasitic extraction
   - Multi-PVT corner analysis (SS, TT, FF + voltage/temperature variations)
   - Fix timing violations (buffering, resizing, etc.)
   - Converge to WNS = 0, TNS = 0 on all corners

**Deliverables:**
- Placed and routed design (DEF/LEF files)
- Clock tree synthesis report
- Power analysis reports (IR drop, EM)
- Timing closure reports (all PVT corners)

### Week 4: Final Verification & Signoff

**Objectives:**
- Run gate-level simulations with timing
- Perform security-focused testing
- Execute final signoff checks
- Generate GDSII

**Activities:**
1. **Gate-Level Simulation:**
   - Simulate gate-level netlist with Xcelium
   - Back-annotate SDF timing for accuracy
   - Run directed tests and regression suite
   - Verify functional equivalence to RTL simulation

2. **Security Testing:**
   - Negative tests for secure boot (tampered images)
   - AES key/data integrity checks
   - Side-channel resistance (basic checks)
   - Fault injection scenarios (if applicable)

3. **Formal LEC:**
   - Final LEC run (post-route netlist vs. RTL)
   - Ensure no logic changes during physical implementation

4. **Physical Verification:**
   - Design Rule Check (DRC)
   - Layout vs. Schematic (LVS)
   - Antenna rule checks
   - Address any violations

5. **GDSII Generation:**
   - Stream out final layout
   - Prepare design for fabrication handoff

**Deliverables:**
- Gate-level simulation passing all tests
- Final LEC report
- DRC/LVS clean reports
- GDSII file
- Final project documentation

## Milestones

| Week | Milestone | Completion Criteria |
|------|-----------|---------------------|
| 1 | RTL & Testbench Complete | All RTL modules coded, directed tests passing in Xcelium |
| 2 | Synthesis & DFT Complete | Gate-level netlist generated, DFT inserted, timing analyzed, LEC passing |
| 3 | Physical Implementation Complete | Design placed/routed, CTS done, timing closed on all corners, power analyzed |
| 4 | Final Verification & GDSII | Gate-level sims pass, security tests pass, DRC/LVS clean, GDSII generated |

## Entry and Exit Criteria

### Entry Criteria (Project Start)
- [ ] Cadence tool licenses available (Xcelium, Genus, Innovus, Tempus, Voltus, Conformal, Virtuoso)
- [ ] PDK and technology libraries accessible
- [ ] Project specifications and requirements defined
- [ ] Development environment set up

### Exit Criteria (Week 1 - RTL Development)
- [ ] All RTL modules implemented and documented
- [ ] Directed tests written and passing
- [ ] RTL simulation logs reviewed, no fatal errors
- [ ] Code coverage >80% on critical modules

### Exit Criteria (Week 2 - Synthesis & DFT)
- [ ] Synthesis completed with acceptable QoR (Quality of Results)
- [ ] DFT structures inserted and verified
- [ ] Timing analysis shows WNS ≥ 0, TNS = 0 on synthesis
- [ ] LEC confirms RTL-to-netlist equivalence

### Exit Criteria (Week 3 - Physical Implementation)
- [ ] Place and route completed
- [ ] Clock tree synthesized with acceptable skew
- [ ] Timing closed on all PVT corners (WNS = 0, TNS = 0)
- [ ] IR drop and EM analysis pass design rules

### Exit Criteria (Week 4 - Final Signoff)
- [ ] Gate-level simulation with SDF passes all tests
- [ ] Security negative tests (bad image rejection) pass
- [ ] Final LEC pass
- [ ] DRC clean (zero violations)
- [ ] LVS clean (zero violations)
- [ ] GDSII file generated and verified

## Deliverables

### Documentation
- [ ] Architecture specification document (this document)
- [ ] RTL code with inline comments
- [ ] Testbench and test plan documentation
- [ ] Synthesis, timing, power, and verification reports
- [ ] Final project report with results summary

### RTL & Netlist
- [ ] SystemVerilog RTL source files (rtl/)
- [ ] Synthesized gate-level netlist (Verilog)
- [ ] Post-route netlist with parasitic extraction

### Verification Artifacts
- [ ] Testbenches (tb/)
- [ ] Simulation logs and waveforms (VCD/FSDB)
- [ ] Coverage reports
- [ ] LEC reports

### Physical Design Files
- [ ] DEF (Design Exchange Format) files
- [ ] LEF (Library Exchange Format) files
- [ ] GDSII stream file
- [ ] Timing (SDF) and parasitic (SPEF) files

### Reports
- [ ] Synthesis QoR report
- [ ] Timing analysis reports (multi-corner)
- [ ] Power analysis reports (static, dynamic, IR drop, EM)
- [ ] DRC/LVS reports

## Risks and Mitigations

### Risk 1: Timing Closure Challenges
- **Risk**: Design may not meet timing at target frequency on all corners
- **Impact**: High (could require frequency reduction or significant redesign)
- **Mitigation**:
  - Start with conservative frequency target (50 MHz)
  - Perform early synthesis to identify critical paths
  - Budget extra time in Week 3 for timing optimization
  - Consider pipelining or architectural changes if needed

### Risk 2: SRAM Compiler Availability
- **Risk**: SRAM compiler for chosen PDK may not be available or configured
- **Impact**: Medium (could delay memory subsystem implementation)
- **Mitigation**:
  - [TO-DO: Confirm SRAM availability early in Week 1]
  - Have fallback plan: use register arrays or behavioral models
  - Work with foundry/vendor to expedite compiler access

### Risk 3: DFT Complexity
- **Risk**: DFT insertion may increase area/power or complicate timing
- **Impact**: Medium (affects testability and manufacturing cost)
- **Mitigation**:
  - [TO-DO: Choose DFT strategy early - e.g., 95% scan coverage]
  - Plan for area/power overhead in synthesis targets
  - Use incremental DFT insertion and verify at each step

### Risk 4: Security Verification Gaps
- **Risk**: Security features (AES, secure boot) may have subtle bugs
- **Impact**: High (security vulnerabilities are critical)
- **Mitigation**:
  - Use known test vectors for AES-128 (NIST test suite)
  - Implement comprehensive negative tests (bad image rejection)
  - Add assertions for security-critical paths
  - Consider third-party security review if time permits

### Risk 5: Tool License Availability
- **Risk**: Cadence tool licenses may have limited seats or availability
- **Impact**: Medium (could cause workflow delays)
- **Mitigation**:
  - Reserve licenses in advance
  - Schedule tool usage efficiently (off-peak hours if needed)
  - Use batch jobs for long-running tasks (synthesis, P&R)

### Risk 6: PDK and Library Issues
- **Risk**: Technology libraries may have incomplete or incorrect data
- **Impact**: High (incorrect timing/power analysis)
- **Mitigation**:
  - [TO-DO: Validate PDK and libraries in Week 1]
  - Cross-check library data with foundry documentation
  - Report and resolve any discrepancies early

## To-Do Items (Specification Completion)

The following items require definition and decision before or during Week 1:

- [ ] **PDK/Technology Node**: Select fabrication process (e.g., TSMC 28nm, Intel 22nm, etc.)
- [ ] **Target Frequency**: Finalize operating frequency (50 MHz, 75 MHz, 100 MHz)
- [ ] **Bus Protocol**: Choose interconnect standard (AHB-Lite, APB, Wishbone, custom)
- [ ] **SRAM Availability**: Confirm SRAM compiler availability and sizes (e.g., 32KB instruction, 16KB data)
- [ ] **DFT Strategy**: Define DFT approach (scan coverage %, MBIST for memories, boundary scan, etc.)
- [ ] **Additional Peripherals**: Determine if UART, GPIO, timers, or other peripherals are required
- [ ] **Voltage and Temperature Corners**: Define PVT corners for analysis (e.g., 0.9V/125°C, 1.0V/25°C, 1.1V/-40°C)
- [ ] **Package and I/O**: Specify package type and I/O pad ring configuration

## Conclusion

This project provides a comprehensive exercise in ASIC design, covering the complete flow from architectural definition through RTL implementation, verification, synthesis, physical design, and final signoff. The inclusion of security features (AES-128 and SHA-256 secure boot) adds real-world relevance and complexity. By following this structured 4-week plan and adhering to the defined milestones and exit criteria, the project aims to deliver a functional, verified, and manufacturable IoT gateway processor SoC.
