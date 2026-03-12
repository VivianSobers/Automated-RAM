# Automated-RAM
## Overview

This project implements an **automated RAM writer** in Logisim Evolution. On simulation start, a free-running counter sequences through RAM addresses from `0000h` upward, while a random generator continuously produces 8-bit values that get written to each address on every clock cycle.

---

## Circuit File

**File:** `AutomatedRAM.circ`  
**Tool:** Logisim Evolution  
**RAM spec:** 16-bit address width · 8-bit data width · Separate load and store ports

---

## How It Works

The circuit has three main components working together:

**Counter (Clock B driven)** — sequences through RAM addresses starting at `0000h`, incrementing by 1 on each Clock B rising edge.

**Random Generator (Clock A driven)** — produces a new pseudo-random 8-bit value on each Clock A tick. Clock A runs at **twice the frequency** of Clock B, so the random value stabilizes before each RAM write.

**RAM** — receives the current address from the counter and the current random value from the generator. With `st` (store enable) tied high, it writes a new byte on every Clock B pulse.

A **D flip-flop** latches the last written value, which is also displayed on a **7-segment display** (showing the hex value of the most recently stored byte).

---

## RAM Configuration

| Parameter | Value |
|-----------|-------|
| Address Bit Width | 16 |
| Data Bit Width | 8 |
| Data Interface | Separate load and store ports |
| Store Enable (`st`) | Tied HIGH (always writing) |

---

## Clock Design

Two separate clocks are used:

- **Clock A** — drives the random generator (faster clock)
- **Clock B** — drives the counter and RAM write pulse (slower clock)
- **Ratio:** Clock A ticks **twice per RAM write cycle**

This 2:1 ratio ensures the random generator has produced a stable new value before Clock B triggers a write — guaranteeing each RAM address receives a unique random byte.

> If both clocks run at the same speed, the random value updates at the same instant as the write pulse, causing repeated values across consecutive addresses.

---

## Write Sequence

On simulation start (after reset):

1. Counter initializes to `0000h`
2. Random generator outputs a stable 8-bit value
3. Clock B fires → value written to address `0000h`
4. Counter increments → address becomes `0001h`
5. Process repeats sequentially through all 65,536 addresses

---

## How to Run

1. Open **Logisim Evolution** (Java required)
2. File → Open → select `AutomatedRAM.circ`
3. Go to **Simulate → Ticks Enabled** (or press `Ctrl+K`)
4. Watch the RAM viewer fill with random hex values starting from address `0000h`
5. The 7-segment display shows the last written byte in hex

---

## Author

**Vivian Sobers E** — [github.com/VivianSobers](https://github.com/VivianSobers)


