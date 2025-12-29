# Project Checklists

This document provides concise weekly exit checklists and a final signoff-lite checklist to track progress and ensure quality gates are met throughout the 4-week project.

## Week 1 Exit Checklist: RTL Development & Initial Verification

**Date Completed:** __________

### RTL Implementation
- [ ] RV32I/M core RTL completed (ALU, register file, control unit, PC, decoder)
- [ ] AES-128 encryption/decryption module implemented
- [ ] SHA-256 secure boot hash verification stub implemented
- [ ] Memory controllers (instruction memory, data memory) implemented
- [ ] Bus interconnect logic completed
- [ ] Top-level SoC integration module created
- [ ] All RTL code follows coding standards and style guidelines

### Specification Finalization
- [ ] PDK/technology node selected and documented
- [ ] Target frequency finalized (50–100 MHz range)
- [ ] Bus protocol type chosen (AHB-Lite, APB, or custom)
- [ ] SRAM compiler availability confirmed and sizes defined
- [ ] DFT strategy documented (scan coverage, MBIST, etc.)
- [ ] Additional peripherals list finalized

### Testbench Development
- [ ] Reset and boot sequence test (reset_boot_test) written and passing
- [ ] RV32I instruction suite tests written and passing
- [ ] M extension (multiply/divide) tests written and passing
- [ ] AES-128 MMIO register access test (aes_mmio_test) written and passing
- [ ] AES-128 encryption/decryption functional tests written and passing
- [ ] SHA-256 hash computation tests written and passing
- [ ] Bad firmware image rejection test (bad_image_rejection_test) written and passing
- [ ] Basic assertions added to critical control paths

### Simulation & Verification
- [ ] All directed tests pass in Cadence Xcelium
- [ ] No fatal simulation errors or warnings
- [ ] RTL simulation waveforms reviewed and verified
- [ ] Code coverage >80% on processor core
- [ ] Code coverage >80% on security peripherals (AES, SHA-256)
- [ ] Initial functional coverage metrics collected

### Documentation
- [ ] RTL modules documented with inline comments
- [ ] Test plan summary documented in testbench README
- [ ] Simulation results logged and reviewed

---

## Week 2 Exit Checklist: Synthesis & DFT Insertion

**Date Completed:** __________

### Synthesis (Cadence Genus)
- [ ] Design constraints file created (SDC format)
- [ ] Clock period constraint set for target frequency
- [ ] Input/output delay constraints defined
- [ ] Synthesis script (TCL) developed and tested
- [ ] RTL successfully synthesized to gate-level netlist
- [ ] Synthesis QoR (Quality of Results) report reviewed
- [ ] Area, power, and timing estimates acceptable
- [ ] Critical path analysis completed

### DFT Insertion
- [ ] DFT strategy implemented (scan chain insertion)
- [ ] Scan chains configured and inserted
- [ ] ATPG (Automatic Test Pattern Generation) patterns generated
- [ ] Scan chain integrity verified (scan shift test)
- [ ] DFT coverage meets target (e.g., >95%)

### Static Timing Analysis (Cadence Tempus)
- [ ] Multi-corner timing analysis performed (SS, TT, FF)
- [ ] Setup timing: WNS (Worst Negative Slack) = 0 on all corners
- [ ] Setup timing: TNS (Total Negative Slack) = 0 on all corners
- [ ] Hold timing: No hold violations
- [ ] Clock domain crossing checks performed (if multiple clocks exist)
- [ ] Timing reports documented and reviewed

### Formal Verification (Cadence Conformal)
- [ ] LEC setup script created
- [ ] RTL vs. synthesized netlist comparison performed
- [ ] All comparison points matched
- [ ] LEC report shows equivalence (no mismatches)
- [ ] Any LEC warnings investigated and resolved

### Deliverables
- [ ] Synthesized gate-level netlist (Verilog format)
- [ ] Synthesis report with area/timing/power metrics
- [ ] DFT insertion report with scan coverage
- [ ] Multi-corner timing reports
- [ ] LEC passing report

---

## Week 3 Exit Checklist: Physical Implementation & Power Analysis

**Date Completed:** __________

### Floorplanning (Cadence Innovus)
- [ ] Die size and aspect ratio defined
- [ ] Floorplan created with core area and I/O ring
- [ ] Hard macros (SRAM blocks) placed
- [ ] Power grid (power/ground stripes) planned
- [ ] I/O pad ring configured
- [ ] Floorplan congestion analysis performed

### Placement & Routing
- [ ] Standard cell placement completed (coarse and refined)
- [ ] Clock tree synthesis (CTS) performed
- [ ] Clock tree quality metrics acceptable (skew, insertion delay)
- [ ] Global routing completed
- [ ] Detailed routing completed
- [ ] Routing DRC violations resolved (or minimal)
- [ ] Filler cells inserted
- [ ] Metal fill added for density rules

### Timing Closure
- [ ] Post-route parasitic extraction completed (SPEF)
- [ ] Post-route STA performed on all PVT corners
- [ ] Setup timing: WNS = 0 on all corners (SS, TT, FF, with voltage/temp variations)
- [ ] Setup timing: TNS = 0 on all corners
- [ ] Hold timing: No hold violations on all corners
- [ ] Clock tree skew within acceptable limits
- [ ] Timing optimization completed (buffering, resizing, etc.)

### Power Analysis (Cadence Voltus)
- [ ] Static power analysis performed
- [ ] Dynamic power analysis performed (average and peak)
- [ ] IR drop analysis completed on power grid
- [ ] Worst-case IR drop within acceptable limits (e.g., <5% VDD)
- [ ] Electromigration (EM) analysis performed on power grid
- [ ] EM violations resolved or mitigated
- [ ] Power optimization steps applied if necessary

### Deliverables
- [ ] Placed and routed design database (Innovus format)
- [ ] DEF (Design Exchange Format) file
- [ ] LEF (Library Exchange Format) file
- [ ] Post-route netlist (Verilog)
- [ ] Parasitic extraction file (SPEF)
- [ ] Clock tree synthesis report
- [ ] Multi-corner post-route timing reports
- [ ] IR drop and EM analysis reports

---

## Week 4 Exit Checklist: Final Verification & Signoff

**Date Completed:** __________

### Gate-Level Simulation (Cadence Xcelium)
- [ ] Gate-level netlist testbench setup completed
- [ ] SDF (Standard Delay Format) timing file back-annotated
- [ ] All directed tests from Week 1 re-run at gate-level
- [ ] Gate-level simulation results match RTL simulation (functionally equivalent)
- [ ] Timing-accurate simulation waveforms reviewed
- [ ] No setup/hold violations observed in gate-level simulation
- [ ] Simulation logs reviewed for errors/warnings

### Security-Focused Testing
- [ ] Secure boot positive test: valid firmware image boots successfully (GL sim)
- [ ] Secure boot negative test: bad/tampered image rejected (GL sim)
- [ ] AES-128 key and data integrity tests passing (GL sim)
- [ ] SHA-256 hash verification corner cases tested
- [ ] Side-channel resistance basic checks performed (if applicable)

### Final Formal Verification
- [ ] LEC performed: post-route netlist vs. RTL
- [ ] All comparison points matched (no mismatches)
- [ ] LEC report documented and reviewed
- [ ] Any LEC warnings investigated and closed

### Physical Verification
- [ ] Design Rule Check (DRC) performed
- [ ] DRC report shows zero violations (or acceptable waiver)
- [ ] Layout vs. Schematic (LVS) performed
- [ ] LVS report shows zero errors (clean match)
- [ ] Antenna rule checks performed
- [ ] Antenna violations resolved or diodes added
- [ ] Density checks (metal fill) passing

### GDSII Generation
- [ ] Final GDSII stream-out performed
- [ ] GDSII file inspected with layout viewer (e.g., Virtuoso, Calibre)
- [ ] GDSII layer mapping verified
- [ ] GDSII file size and integrity checked
- [ ] GDSII ready for fabrication handoff

### Documentation
- [ ] Final project report written
- [ ] All verification results summarized
- [ ] Known issues and limitations documented
- [ ] Design performance summary (frequency achieved, area, power)
- [ ] Lessons learned documented

---

## Final Signoff-Lite Checklist

This checklist provides a high-level summary of all critical deliverables and quality gates for final project signoff.

**Project Name:** IoT Gateway Processor (RV32I/M with AES-128 and SHA-256)

**Signoff Date:** __________

### RTL & Verification (Week 1)
- [ ] ✅ All RTL modules complete and documented
- [ ] ✅ Directed tests passing (reset/boot, AES MMIO, bad image rejection)
- [ ] ✅ RTL simulation clean (no fatal errors)
- [ ] ✅ Code coverage >80% on critical modules
- [ ] ✅ Assertions in place for security-critical paths

### Synthesis & Timing (Week 2)
- [ ] ✅ Synthesis clean and QoR acceptable
- [ ] ✅ Multi-corner STA: WNS = 0, TNS = 0 (post-synthesis)
- [ ] ✅ DFT insertion complete with >95% scan coverage
- [ ] ✅ LEC pass: RTL vs. synthesized netlist

### Physical Design & Power (Week 3)
- [ ] ✅ Design placed and routed successfully
- [ ] ✅ Multi-corner STA: WNS = 0, TNS = 0 on all PVT corners (post-route)
- [ ] ✅ No hold violations on any corner
- [ ] ✅ IR drop analysis pass (worst IR drop <5% VDD)
- [ ] ✅ EM analysis pass (no violations or mitigated)

### Final Verification (Week 4)
- [ ] ✅ Gate-level simulation with SDF: all tests pass
- [ ] ✅ Security negative tests pass (bad image rejection at GL)
- [ ] ✅ LEC pass: post-route netlist vs. RTL
- [ ] ✅ DRC clean (zero violations)
- [ ] ✅ LVS clean (zero errors)

### Deliverables
- [ ] ✅ Complete RTL source code (rtl/)
- [ ] ✅ Testbenches and verification suite (tb/)
- [ ] ✅ Synthesized and post-route netlists
- [ ] ✅ Timing reports (all corners)
- [ ] ✅ Power reports (static, dynamic, IR drop, EM)
- [ ] ✅ GDSII file generated and verified
- [ ] ✅ Final project documentation and report

### Risk & Issue Closure
- [ ] ✅ All critical risks mitigated or resolved
- [ ] ✅ No open high-severity issues
- [ ] ✅ Known limitations documented

---

**Signoff Approvals:**

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Designer/Engineer | __________ | __________ | __________ |
| Verification Lead | __________ | __________ | __________ |
| Physical Design Lead | __________ | __________ | __________ |
| Project Manager | __________ | __________ | __________ |

---

**Notes:**

Use these checklists throughout the project to track progress and ensure that each week's deliverables meet the defined exit criteria. The final signoff-lite checklist serves as a summary for project completion and handoff.
