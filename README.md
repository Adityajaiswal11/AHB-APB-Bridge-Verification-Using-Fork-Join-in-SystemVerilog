# AHB-APB-Bridge-Verification-Using-Fork-Join-in-SystemVerilog

This project demonstrates the functional verification of an AHB to APB Bridge using SystemVerilog with advanced concurrent process control via fork-join. The design mimics a basic bridge behavior where write and read requests from the AHB side are translated to APB memory transactions, and apb_ready is used to indicate transaction completion.

📌 Project Overview
The module under test (DUT) simulates a simplified AHB-to-APB bridge that:
Accepts address and data from an AHB-like interface.
Performs memory read/write on an internal APB-like memory.
Generates apb_ready to indicate transaction completion.
Returns read data through apb_rdata on read requests.

The testbench performs:
Write and read operations using separate tasks.
Concurrent monitoring using a fork-join_none structure to simulate real-world asynchronous observation.
Print-based logging to simulate waveform-independent debug visibility.

🧠 Key Concepts
SystemVerilog Task-Based Testing: Modular task definitions for reusable write and read operations.
Concurrency with Fork-Join: fork...join_none allows non-blocking background monitoring while transactions proceed.
Clock Generation: 10ns period (50MHz) clock is used.
AHB-Like Signaling: Signals like ahb_write, ahb_addr, ahb_wdata simulate AHB behavior.
APB Readiness: apb_ready ensures valid transaction synchronization before continuing.

📁 Project Structure
text
Copy
Edit
├── fj.v           # AHB to APB Bridge Design (DUT)
├── fj_tb.v        # Testbench with fork-join-based concurrency
├── README.md      # Project documentation
🛠️ Technologies Used
SystemVerilog (IEEE 1800)

Fork-Join concurrency for background tasks

Simulation Tools: Compatible with tools like:
Aldec Riviera Pro
Synopsys VCS
ModelSim / QuestaSim
Xilinx Vivado Simulator

🚀 How It Works
📌 DUT (fj.v)
Internal APB memory array (apb_mem[0:255]) stores 32-bit data.
On write (ahb_write = 1): Writes ahb_wdata to memory at ahb_addr[7:0].
On read (ahb_write = 0): Outputs data from memory at ahb_addr[7:0] on apb_rdata.
apb_ready is asserted for one cycle after each transaction.

🧪 Testbench (fj_tb.v)
Initializes AHB signals and resets the system.
Uses tasks to perform:
ahb_master_write(addr, data)
apb_slave_read(addr)
bus_monitor() logs bus state continuously for 10 cycles using fork...join_none.

✅ Example Output Log
less
Copy
Edit
[50] AHB Master: Writing A5A5A5A5 to Address 00000010
[60] Write to address 0x10 completed
[80] APB Slave: Read Request at Address 00000010
[90] APB Slave: Read Data from Address 00000010: A5A5A5A5
[100] Bus Monitor: Observing Transactions...
...
🧪 Test Coverage
Write → Read Consistency at different addresses.
Concurrency Safety via fork-join observation.
Reset Behavior validation.
Bus Timing Alignment with clock.

