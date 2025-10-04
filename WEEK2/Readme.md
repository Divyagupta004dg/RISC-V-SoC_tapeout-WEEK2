#  VSDBabySoC — RISC-V + PLL + DAC System-on-Chip Simulation

This project implements a simplified **System-on-Chip (SoC)** that integrates a RISC-V CPU core (`RVMYTH`), a **PLL**, and a **DAC**.  
The design is simulated using **Icarus Verilog** and visualized in **GTKWave**.

---

##  Project Structure

VSDBabySoC/

├── src/

│ ├── include/ # Header files (*.vh)

│ └── module/ # Verilog + TL-Verilog source files

│ ├── vsdbabysoc.v # Top-level SoC module

│ ├── rvmyth.v # CPU (converted from TL-Verilog)

│ ├── avsdpll.v # Phase Locked Loop

│ ├── avsddac.v # Digital-to-Analog Converter

│ └── testbench.v # Full SoC testbench

└── output/ # Simulation outputs


---

##  Environment Setup

> Run all commands inside Ubuntu terminal ( `divya@divya-VirtualBox`).

### 1. Install dependencies
```bash
sudo apt update
sudo apt install python3-venv python3-pip iverilog gtkwave -y
```
### 2. Create a Python virtual environment
```
cd ~/VLSI/VSDBabySoC
python3 -m venv sp_env
source sp_env/bin/activate
```
### 3. Install SandPiper-SaaS (for TL-Verilog → Verilog conversion)
 
```bash
Copy code
pip install pyyaml click sandpiper-saas
```
### 4. Convert TL-Verilog CPU (rvmyth.tlv) → Verilog
   
```bash
Copy code
sandpiper-saas -i ./src/module/rvmyth.tlv \
  -o rvmyth.v --bestsv --noline -p verilog \
  --outdir ./src/module/
```
Now we should see:

ls src/module/rvmyth.v

## Simulation Flow

### 1. Pre-synthesis simulation
```   
rm -rf output/pre_synth_sim
mkdir -p output/pre_synth_sim
iverilog -o output/pre_synth_sim/pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v

cd output/pre_synth_sim
./pre_synth_sim.out
```

### 2. View waveform
```
gtkwave pre_synth_sim.vcd
```

##  Signals to Observe in GTKWave

| **Signal Name** | **Description** |
|------------------|------------------|
| **uut.pll.CLK** | PLL generated clock signal |
| **uut.cpu.RV_TO_DAC[9:0]** | 10-bit output from CPU to DAC |
| **uut.dac.OUT** | DAC analog output waveform |
| **uut.reset** | Reset signal |
| **uut.pll.ENb_CP / ENb_VCO** | PLL internal enables |

🖱️ In GTKWave:

Right-click uut.dac.OUT → Data Format → Analog Step
→ This displays the analog-like triangular waveform.

### How the System Works

🔹 Architecture Flow

*1 RISC-V CPU (RVMYTH) executes a small assembly program that:*

- Increments a counter

- Accumulates the result in register r17

- Sends this 10-bit value to the DAC

*2 DAC converts the digital r17 value into an analog-like voltage (OUT).*

*3 PLL provides a stable clock for the CPU and SoC.*

## 🔹 Instruction Program Summary

| # | **Instruction** | **Action** |
|---|-----------------|------------|
| 0 | ADDI r9, r0, 1 | r9 = 1 (step value) |
| 1 | ADDI r10, r0, 43 | r10 = 43 (loop limit) |
| 2 | ADDI r11, r0, 0 | r11 = 0 (counter) |
| 3 | ADDI r17, r0, 0 | r17 = 0 (DAC input) |
| 4 | ADD r17, r17, r11 | Accumulate |
| 5 | ADDI r11, r11, 1 | Increment counter |
| 6 | BNE r11, r10, -4 | Loop until r11 = 43 |
| 7 | ADD r17, r17, r11 | r17 += r11 |
| 8 | SUB r17, r17, r11 | r17 -= r11 |
| 9 | SUB r11, r11, r9 | r11-- |
| 10 | BNE r11, r9, -4 | Loop until r11 = 1 |
| 11 | SUB r17, r17, r11 | Final adjust |
| 12 | BEQ r0, r0, ... | Infinite hold loop |

##  Waveform Analysis

| **Phase** | **Register Behavior** | **Description** |
|-----------|---------------------|-----------------|
| **Ramp-Up** | r11 = 0 → 42, r17 = Σ0..42 = 903 | Rising triangular ramp |
| **Peak** | r11 = 43, r17 = 946 | Maximum point |
| **Oscillation** | r11 = 43 → 1, r17 = 903 ± r11 | Falling triangular slope |
| **Final Hold** | r11 = 1, r17 steady | End of loop |


##  DAC Output Voltage

For VREF = 1.0 V:

| **r17 Value** | **DAC Output (V)** |
|---------------|------------------|
| 903           | 0.882 V          |
| 946 (peak)    | 0.925 V          |

**Formula:**

`V_OUT = (r17 / 1023) * V_REF`

### Example Waveforms

🔹 PLL and Clock Output
