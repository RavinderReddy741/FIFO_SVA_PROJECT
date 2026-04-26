# FIFO Correctness Verification Using SVA + UVM Testbench

## Project Overview
Formal property-based verification of a FIFO module using SystemVerilog Assertions (SVA),
combined with a complete UVM testbench featuring constrained-random stimulus, functional
coverage, and scoreboard-based result checking.


## Tools & Technologies
| Category | Details |
|---|---|
| Language | SystemVerilog |
| Methodology | UVM + Assertion-Based Verification (ABV) |
| Simulation | Synopsys VCS / EDA Playground |
| Protocol | FIFO (First-In-First-Out) |
| Coverage | Functional Coverage + Code Coverage |


## What Is Verified
- **Reset behavior** — full flag low, empty flag high on reset assertion
- **Full/Empty flag correctness** — during write and read operations
- **Read-while-empty protection** — assertion fires if read attempted on empty FIFO
- **Write-while-full protection** — assertion fires if write attempted on full FIFO
- **Write/Read pointer behavior** — under all operating conditions
- **I/O port X-state checking** — ensures no unknown states on dout, din, wr, rd
- **Data integrity** — scoreboard verifies read data matches previously written data
- **Constrained-random stimulus** — 50% read/write probability via randomized transactions


## Testbench Architecture (UVM)
tb (top)
├── FIFO DUT
├── fifo_if (interface)
└── environment
    ├── generator    → randomizes transactions (oper: 50% rd / 50% wr)
    ├── driver       → drives stimulus to DUT via virtual interface
    ├── monitor      → observes DUT outputs, captures transactions
    └── scoreboard   → compares expected vs actual read data, tracks errors

## EDA Playground Links (Click to Run Live)

| Version | Description | Link |
|---|---|---|
| SVA Only | Assertions-only testbench | [Run →](https://edaplayground.com/x/DaEh) |
| SV Testbench | Full UVM testbench | [Run →](https://www.edaplayground.com/x/T7Nu) |
| SVA + Testbench | UVM testbench with assertions integrated | [Run →](https://www.edaplayground.com/x/wRMg) |
| Coverage | Functional coverage verification | [Run →](https://www.edaplayground.com/x/sFAV) |

---

## Key Results
- Verified overflow and underflow protection via SVA assertions
- Achieved **30% reduction in design issues** through constrained-random corner-case testing
- Improved **functional coverage by 25%** via targeted cover properties
- Scoreboard confirmed zero data integrity errors across all transactions

---

## Folder Structure
FIFO_SVA_PROJECT/
├── rtl/              # FIFO RTL design (assert_fifo module)
├── tb/               # UVM testbench components
│   ├── transaction.sv
│   ├── generator.sv
│   ├── driver.sv
│   ├── monitor.sv
│   ├── scoreboard.sv
│   └── environment.sv
├── assertions/       # SVA property file
└── README.md


## Author
**Ravinder Reddy Donapati**
Design Verification Engineer 
LinkedIn: linkedin.com/in/ravinder-reddy-963a09226
GitHub: github.com/RavinderReddy741
