# SPI Master Module in Verilog

## Overview
This project implements an SPI (Serial Peripheral Interface) master module in Verilog to communicate with an SPI slave device. The SPI master transmits an 8-bit data frame (`0xA5`) to the slave device and continuously reads data back from it. The design operates in SPI mode 0 (CPOL = 0, CPHA = 0) and is configured to run with an SPI clock (`sclk`) of 1 MHz, derived from a 50 MHz input clock.

## Features
- Sends a pre-defined 8-bit data frame (`0xA5`) to the slave.
- Receives an 8-bit data frame from the slave.
- Operates in SPI Mode 0 (CPOL = 0, CPHA = 0).
- Designed to work with a 50 MHz input clock, divided down to generate a 1 MHz SPI clock.

## Project Structure
- **spi_master.v**: Contains the Verilog code for the SPI master module.
- **spi_master_tb.v**: Testbench for simulating and verifying the SPI master module.
- **README.md**: Project documentation.

## Files
| File Name         | Description                                    |
| ----------------- | ---------------------------------------------- |
| `spi_master.v`    | Verilog module implementing the SPI master     |
| `spi_master_tb.v` | Testbench for the SPI master module simulation |
| `README.md`       | Project documentation                          |

## Design Specifications
- **SPI Mode**: Mode 0 (CPOL = 0, CPHA = 0)
- **Clock Frequency**: 1 MHz for `sclk`
- **Data Frame**: 8-bit (`0xA5`)
- **Chip Select (CS)**: Active low
- **Signals**: `sclk`, `mosi`, `miso`, `cs`
- **Input Clock**: 50 MHz

## Module Ports
| Port   | Direction | Description              |
| ------ | --------- | ------------------------ |
| `clk`  | Input     | 50 MHz clock input       |
| `rst`  | Input     | Active high reset        |
| `sclk` | Output    | SPI clock output (1 MHz) |
| `mosi` | Output    | Master Out Slave In      |
| `miso` | Input     | Master In Slave Out      |
| `cs`   | Output    | Chip Select, active low  |

## Functional Description
The SPI master module operates with a simple state machine that manages the communication with the SPI slave.

1. **IDLE State**: Initializes the chip select (`cs`) to low, prepares data for transmission, and moves to the `TRANSFER` state.
2. **TRANSFER State**:
   - Toggles `sclk` at a frequency of 1 MHz to synchronize data transmission and reception.
   - Sends each bit of `data_out` on `mosi` on the falling edge of `sclk`.
   - Captures data from `miso` on the rising edge of `sclk` and stores it in `data_in`.
   - When all 8 bits are transmitted and received, transitions to the `DONE` state.
3. **DONE State**: Deactivates `cs` and returns to `IDLE`.

## Testbench (`spi_master_tb.v`)
The testbench simulates the SPI master module and verifies its functionality:
- Generates a 50 MHz clock.
- Activates reset and initializes the simulation.
- Simulates data on the `miso` line to mimic responses from an SPI slave.

## Simulation and Synthesis
1. Add `spi_master.v` and `spi_master_tb.v` to your FPGA design software (e.g., Vivado).
2. Run the behavioral simulation to verify the SPI master module.
3. Use the following steps in Vivado:
   - **Add Sources**: Add `spi_master.v`.
   - **Add Simulation Sources**: Add `spi_master_tb.v`.
   - **Run Simulation**: Perform behavioral simulation and check waveforms.

## Expected Results
- **`sclk`** toggles at 1 MHz.
- **`cs`** is active low during data transmission.
- **`mosi`** transmits the `0xA5` data frame (10100101) bit-by-bit on the falling edge of `sclk`.
- **`miso`** simulates receiving data on the rising edge of `sclk`.

## Future Scope
Potential improvements could include:
- Supporting different SPI modes (CPOL/CPHA settings).
- Adding parameterized data frames and frame sizes.
- Implementing dynamic clock scaling.

## Author
- **Name**: Shubham Kishor Vispute
- **BITS ID**: 2024HT01026

## License
This project is open for educational and non-commercial purposes.

## Acknowledgments
Special thanks to the course instructors for their guidance and support.

---