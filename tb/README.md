# Testbench Directory

This directory contains testbenches and verification infrastructure for the IoT Gateway Processor SoC.

## Directed Tests

The verification strategy includes the following directed tests:

### Core Tests
- **reset_boot_test**: Verifies proper reset behavior and boot sequence
  - Checks register initialization
  - Validates reset vector and initial PC
  - Confirms boot ROM access

### AES-128 Peripheral Tests
- **aes_mmio_test**: Tests memory-mapped I/O register access
  - Write/read key registers
  - Write/read data input/output registers
  - Control register functionality

- **aes_encryption_test**: Validates encryption functionality
  - Uses known test vectors (NIST test suite)
  - Verifies correct ciphertext output

- **aes_decryption_test**: Validates decryption functionality
  - Uses known test vectors
  - Verifies correct plaintext output

### Security Tests
- **bad_image_rejection_test**: Tests secure boot with invalid firmware
  - Loads tampered/corrupted firmware image
  - Verifies that boot sequence rejects the image
  - Ensures processor does not execute untrusted code

- **secure_boot_pass_test**: Tests secure boot with valid firmware
  - Loads valid firmware with correct SHA-256 hash
  - Verifies successful boot sequence

### SHA-256 Tests
- **sha256_hash_test**: Tests hash computation
  - Computes hashes of known messages
  - Compares against expected SHA-256 outputs

## Assertions

Testbenches include SystemVerilog Assertions (SVA) for:
- Bus protocol compliance
- Memory access timing
- Security property checks
- Register access sequencing

## Coverage

Coverage metrics collected include:
- Code coverage (line, branch, FSM state)
- Functional coverage (instruction mix, corner cases)
- Toggle coverage on critical signals

## Running Tests

```bash
# To be implemented - placeholder
# Example usage:
# make sim TEST=reset_boot
# make sim TEST=aes_mmio
# make sim TEST=bad_image_rejection
```

See the main [README](../README.md) and [project documentation](../docs/project.md) for more details.
