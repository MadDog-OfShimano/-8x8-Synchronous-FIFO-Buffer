## This is a 8-entry × 8-bit Synchronous FIFO Buffer Project Implemented In Verilog 

---

## Project Structure

```
fifo_project/
├── fifo_buffer.v   # FIFO core module
├── fifo_tb.v       # Testbench (8 test scenarios)
└── README.md       # This file
```

---

## Features

- Overflow & underflow protection
- Simultaneous read and write support
- Zero-latency combinational status flags
- Circular buffer via natural pointer overflow
- Fully synchronous single clock domain
- Comprehensive 8-scenario testbench

---

## Requirements

- [Icarus Verilog](https://bleyer.org/icarus/) — for compilation and simulation
- [GTKWave](https://gtkwave.sourceforge.net) — for waveform viewing

### Install on Ubuntu/Debian
```bash
sudo apt install iverilog gtkwave
```

### Install on Windows
Download and run the installer from: https://bleyer.org/icarus/  
GTKWave is bundled with the installer.

### Install on Mac
```bash
brew install icarus-verilog gtkwave
```

---

## How to Run

**1. Compile**
```bash
iverilog -o fifo_sim.out fifo_tb.v fifo_buffer.v
```

**2. Run simulation**
```bash
vvp fifo_sim.out
```

**3. View waveform**
```bash
gtkwave fifo_wave.vcd
```

In GTKWave: expand `fifo_tb` in the left panel → select signals → click Append → press `Ctrl+Shift+F` to zoom fit.

---

## Signals

| Signal       | Direction | Width  | Description                        |
|--------------|-----------|--------|------------------------------------|
| clk          | Input     | 1-bit  | Clock — all updates on rising edge |
| rst          | Input     | 1-bit  | Active-high synchronous reset      |
| wr_en        | Input     | 1-bit  | Write enable                       |
| rd_en        | Input     | 1-bit  | Read enable                        |
| data_in      | Input     | 8-bit  | Data to write into FIFO            |
| data_out     | Output    | 8-bit  | Data read from FIFO                |
| full         | Output    | 1-bit  | High when FIFO is full (count==8)  |
| empty        | Output    | 1-bit  | High when FIFO is empty (count==0) |

---

## Test Scenarios

| # | Test                     | Expected Result                        |
|---|--------------------------|----------------------------------------|
| 1 | Reset                    | empty=1, full=0                        |
| 2 | Write 3 bytes            | Data stored, empty=0                   |
| 3 | Read 3 bytes             | Output AA→BB→CC in order               |
| 4 | Fill to capacity         | full=1 after 8 writes                  |
| 5 | Overflow attempt         | Write ignored, FIFO unchanged          |
| 6 | Drain completely         | Output 01→08 in order, empty=1         |
| 7 | Underflow attempt        | Read ignored, no corruption            |
| 8 | Simultaneous read+write  | Count stable, correct data in/out      |
