# RISC-V SoC Tapeout Journey – VSD India

[![RISC-V](https://img.shields.io/badge/RISC--V-Reference%20SoC-blue)](https://riscv.org/)
[![RTL → GDSII](https://img.shields.io/badge/Flow-RTL%20%E2%86%92%20GDSII-purple)](https://en.wikipedia.org/wiki/GDSII)
[![Tapeout Ready](https://img.shields.io/badge/Goal-Tapeout%20Ready-red)](https://en.wikipedia.org/wiki/Photomask)
[![Made in India](https://img.shields.io/badge/Made%20in-India-green)](https://www.makeinindia.com/)

## **RISC-V-SoC_tapeout-WEEK2**

*VSDBabySoC is an open-source System-on-Chip (SoC) built using Sky130 technology, integrating digital and analog components on a single chip. It is based on the RVMYTH RISC-V processor core, combined with a Phase-Locked Loop (PLL) for stable clock generation and a 10-bit Digital-to-Analog Converter (DAC) for real-world interfacing. The SoC demonstrates how digital computation, precise timing, and analog conversion work together to create a compact and efficient system.*

![Image](https://github.com/user-attachments/assets/13707b3d-af04-453d-bce6-add4421b2e2a)

## VSDBabySoC – RISC-V Based System on Chip (SoC) Design

## Project Overview

This project focuses on designing a compact, open-source System on Chip (SoC) using the RVMYTH (RISC-V) processor core.

The SoC integrates:

Phase-Locked Loop (PLL) → for precise clock generation and synchronization

10-bit Digital-to-Analog Converter (DAC) → for digital-to-analog interfacing (audio/video output)

🏷️ The SoC (fabricated using Sky130 technology) demonstrates digital + analog integration — a key skill for modern chip designers.

It serves as an educational and experimental platform for SoC design, clock management, and analog interfacing.

## Understanding System on Chip (SoC)

A System on Chip (SoC) is like a complete computer on a single silicon chip.
It combines multiple components that earlier existed separately on boards.

🔹 Main Components

- CPU (RVMYTH – RISC-V Core) → Main processing brain

- Memory (RAM/ROM) → Data storage and program execution

- I/O Interfaces → Connects to peripherals (UART, SPI, GPIO, etc.)

- GPU / DSP → Handles graphics, audio/video signal processing

- Power Management → Controls voltage and power domains

- Analog Blocks → PLL, DAC/ADC for real-world interfacing

## Why SoCs are Important

- High performance – tightly integrated data flow

- Low power – small distances → less power loss

- Cost-effective – one chip instead of multiple boards

- Compact – perfect for mobile, IoT, and wearable systems

- Reliable – fewer connections = fewer failure points

 ## Types of SoCs

| **Type** | **Description** | **Examples** |
|-----------|-----------------|---------------|
| **Microcontroller-based SoC** | Low power, simple control tasks | IoT sensors, appliances |
| **Microprocessor-based SoC** | High performance, can run OS | Smartphones, tablets |
| **Application-specific SoC (ASIC)** | Custom optimized for one use | GPUs, AI accelerators |

## VSDBabySoC Architecture

**VSDBabySoC** = *RVMYTH (RISC-V CPU) + 8x PLL + 10-bit DAC*

🔸**1. Initialization & Clock Generation**

Input signal activates PLL

PLL locks and generates a stable, synchronized clock

Keeps RVMYTH and DAC perfectly timed

🔖 Prevents data corruption due to mismatched clocks

🔸**2. Data Processing (RVMYTH)**

The RVMYTH core executes instructions

Uses r17 register to hold and cycle values

Sends digital data continuously to DAC for analog conversion

🔸**3. Analog Output (DAC)**

The 10-bit DAC converts digital data → analog signal

Output (saved as OUT) can drive TVs, speakers, etc.

🔖 Demonstrates digital-to-analog communication in real applications

<img width="1361" height="1123" alt="image" src="https://github.com/user-attachments/assets/52a53868-d240-4d86-b3cc-2d65efd24942" />

## Key Components Explained

*1 Phase-Locked Loop (PLL)*

Synchronizes output phase/frequency with input reference

Components:

- Phase Detector – compares input vs. feedback phase

- Loop Filter – smooths error signal

- VCO (Voltage-Controlled Oscillator) – adjusts frequency

**Why PLL is needed:**

- Off-chip clocks have delay, jitter, and frequency errors (ppm)

- On-chip PLL provides precise & stable clocks

*2 Digital-to-Analog Converter (DAC)*

Converts binary (digital) data → continuous analog signal

Common DAC types:

- Weighted Resistor DAC

- R-2R Ladder DAC (used in BabySoC)

- In VSDBabySoC → 10-bit DAC → 1/1024 analog precision

