**Formal Property Verification for FIFO Correctness Using SystemVerilog Assertions and a Comprehensive Testbench**


SYSTEM_VERILOG_ASSERTIONS:

A SystemVerilog project focusing on property-based verification of a FIFO module, ensuring correct behavior during various operations through robust SystemVerilog assertions.
The code includes assertions to check various properties of the FIFO behavior during different scenarios.
Here is a summary of the key components and functionalities:

1. **FIFO Module (assert_fifo):**
   - Inputs:
     - clk: Clock signal.
     - rst: Reset signal.
     - wr: Write enable signal.
     - rd: Read enable signal.
     - din: Data input.
     - dout: Data output.
     - empty: Empty flag.
     - full: Full flag.

   - Assertions:
     - **(1) Reset Status:**
       - Checks that when the reset (rst) is asserted, the full signal is low (1'b0) and the empty signal is high (1'b1).

     - **(2) Full and Empty Operation:**
       - Checks the behavior of the `full` and `empty` flags during different scenarios, including write and read operations.

     - **(3) Read While Empty:**
       - Asserts that if the FIFO is empty, reading (`rd`) is not allowed.

     - **(4) Write While Full:**
       - Asserts that if the FIFO is full, writing (`wr`) is not allowed.

     - **(5) Write+Read Pointer Behavior:**
       - Checks the behavior of write and read pointers under different conditions.

     - **(6) State of I/O Ports:**
       - Verifies that the I/O ports (`dout`, `rst`, `wr`, `rd`, `din`) are not in the unknown state.

     - **(7) Data Matching:**
       - Verifies that the data read from the FIFO matches the data previously written.
      
EDA PLAYGROUND LINK FOR ONLY ASSERTION : https://edaplayground.com/x/DaEh

2. **Testbench Module (tb):**

   - Inputs and Outputs:
     - clk: Clock signal.
     - rst: Reset signal.
     - wr: Write enable signal.
     - rd: Read enable signal.
     - din: Data input.
     - dout: Data output.
     - empty: Empty flag.
     - full: Full flag.

   - Tasks:
     - write(): Simulates writing to the FIFO.
     - read(): Simulates reading from the FIFO.

   - Initial Block:
     - Initializes signals and triggers certain events.

   - Main Block:
     - Instantiates the FIFO module (`FIFO`) and binds it to the assertion module (`assert_fifo`).
     - Defines clock toggling behavior.

   - Assertions and Finish:
     - Sets up assertion-related configurations.
     - Creates a VCD file for waveform dumping.
     - Finishes simulation after a certain duration.

Overall, this  code provides a testbench environment to verify the functionality and properties of the FIFO module through the use of assertions and simulation events.


TESTBENCH ARCHITECTURE:

1. FIFO Module (`FIFO`):

- Represents a basic First-In-First-Out memory module.
- **Ports:**
  - `clk`: Clock signal.
  - `rst`: Reset signal.
  - `wr`: Write enable.
  - `rd`: Read enable.
  - `din`: 8-bit data input.
  - `dout`: 8-bit data output.
  - `empty`: Indicates whether the FIFO is empty.
  - `full`: Indicates whether the FIFO is full.
- **Internal Variables:**
  - `wptr`: Write pointer.
  - `rptr`: Read pointer.
  - `cnt`: Counter for the number of elements in the FIFO.
  - `mem`: Array to store data.
- **Behavior:**
  - Performs write (`wr`) and read (`rd`) operations based on the clock edge.
  - Includes logic for handling resets and managing pointers.

2. FIFO Interface (`fifo_if`):

- Defines the communication signals between modules.
- **Signals:**
  - `clock`: Clock signal.
  - `rd`: Read enable.
  - `wr`: Write enable.
  - `full`: Flag indicating the FIFO is full.
  - `empty`: Flag indicating the FIFO is empty.
  - `data_in`: 8-bit data input.
  - `data_out`: 8-bit data output.
  - `rst`: Reset signal.

3. Transaction Class (`transaction`):

- Represents a transaction with random properties.
- **Properties:**
  - `oper`: Random bit for operation control (read or write).
  - `rd`, `wr`: Control bits for read and write.
  - `data_in`: 8-bit data input.
  - `full`, `empty`: Flags for full and empty status.
  - `data_out`: 8-bit data output.
- **Constraint:**
  - `oper` is randomized with a 50% probability of being 1 or 0.

4. Generator Class (`generator`):

- Generates transactions and communicates through a mailbox.
- **Properties:**
  - `tr`: Transaction object.
- **Tasks:**
  - `run()`: Repeatedly generates transactions, randomizes them, and puts them in the mailbox.

5. Driver Class (`driver`):

- Drives random stimuli to the FIFO based on received transactions.
- **Properties:**
  - Virtual interface (`fif`): Interface to the FIFO.
  - `mbx`: Mailbox for communication.
  - `datac`: Transaction object.
- **Tasks:**
  - `reset()`: Resets the DUT.
  - `write()`: Writes data to the FIFO.
  - `read()`: Reads data from the FIFO.
  - `run()`: Applies random stimuli to the DUT.

6. Monitor Class (`monitor`):

- Observes and captures FIFO status based on interface signals.
- **Properties:**
  - Virtual interface (`fif`): Interface to the FIFO.
  - `mbx`: Mailbox for communication.
  - `tr`: Transaction object.
- **Tasks:**
  - `run()`: Continuously monitors the FIFO and puts captured information in the mailbox.

7. Scoreboard Class (`scoreboard`):

- Compares expected and actual behavior of the FIFO.
- **Properties:**
  - `mbx`: Mailbox for communication.
  - `tr`: Transaction object.
  - `din`: Array to store written data.
  - `temp`: Temporary data storage.
  - `err`: Error count.
- **Tasks:**
  - `run()`: Continuously checks and stores transactions, compares read data with expected data.

8. Environment Class (`environment`):

- Composes the generator, driver, monitor, and scoreboard classes.
- **Properties:**
  - `gen`, `drv`, `mon`, `sco`: Instances of generator, driver, monitor, and scoreboard.
  - `gdmbx`, `msmbx`: Mailboxes for generator-driver and monitor-scoreboard communication.
  - `fif`: Interface to the FIFO.
- **Tasks:**
  - `pre_test()`: Resets the DUT.
  - `test()`: Orchestrates the test flow.
  - `post_test()`: Displays error count after the test.

9. Testbench (`tb`):

- Instantiates the FIFO module and creates an environment to run the test.
- **Modules:**
  - `FIFO`: The FIFO module.
  - `fifo_if`: The FIFO interface.
- **Initial Block:**
  - Sets up initial conditions.
- **Clock Toggle:**
  - Toggles the clock every 10 time units.
- **Environment Run:**
  - Calls the `run()` task of the environment.
- **Test Execution:**
  - The generator generates transactions, and the driver, monitor, and scoreboard run concurrently.
- **Simulation Termination:**
  - Simulation terminates after the specified number of transactions.
  - Displays the error count from the scoreboard.

This testbench follows a modular and structured approach, making it clear and maintainable for verifying the functionality of the FIFO module.
EDA PLAYGROUNG LINK FOR SV TESTBENCH : https://www.edaplayground.com/x/T7Nu

EDA PLAYGROUND LINK FOR SV TESTBENCH WITH INCLUDING ASSERTIONS IN IT : https://www.edaplayground.com/x/wRMg

EDA PLAGROUND LINK FOR COVERAGE VERIFICATION: https://www.edaplayground.com/x/sFAV
