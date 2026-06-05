# CMOS-Circuit-Design-and-SPICE-Simulation-using-Sky130nm-Technology

## Table of Contents

- Project Overview
- Tools and Technology Used
# Day 1: Basics of NMOS and SPICE Simulation
  - Introduction to SPICE Simulation
  - NMOS Structure and Working
  - Threshold Voltage and Strong Inversion
  - Id vs Vds Characteristics
  - Lab: NMOS Id-Vds Simulation using Sky130 Models
# Day 2: Velocity Saturation and CMOS Inverter VTC
  - Short Channel Effects
  - Velocity Saturation
  - Id vs Vgs Simulation
  - CMOS Inverter Voltage Transfer Characteristics
# Day 3: CMOS Switching Threshold and Dynamic Simulation
  - CMOS Inverter SPICE Deck
  - Static Simulation of CMOS Inverter
  - Switching Threshold Vm
  - Transient Analysis and Delay Calculation
# Day 4: Noise Margin Analysis
  - Introduction to Noise Margin
  - Noise Margin Parameters
  - Noise Margin Calculation using VTC
# Day 5: Power Supply and Device Variation Analysis
  - Power Supply Variation
  - Device Variation
  - Conclusion

---

# Project Overview

This repository contains my work from a CMOS circuit design and SPICE simulation workshop using Sky130nm technology. The workshop focused on understanding MOSFET device behavior, CMOS inverter characteristics, SPICE simulation flow, switching threshold, delay, noise margin, and robustness against supply/device variations.

The main objective of this project was to understand how CMOS circuits are designed, simulated, and characterized using SPICE tools and open-source Sky130 technology files.

---

# Tools and Technology Used

- Ngspice
- Sky130 PDK / Sky130 model files
- Linux terminal / Virtual machine
- SPICE netlists
- GitHub for documentation
- Screenshots and waveform plots from simulations

---

# Day 1: Basics of NMOS and SPICE Simulation

## Introduction to SPICE Simulation

SPICE stands for Simulation Program with Integrated Circuit Emphasis. It is used to simulate electronic circuits before fabrication.

In CMOS design, SPICE simulations are important because they help us analyze:

- Drain current characteristics
- Voltage transfer characteristics
- Delay
- Power consumption
- Noise margins
- Effect of device and supply variations

In digital VLSI design, timing libraries, delay tables, and cell characterization are based on circuit-level SPICE simulations.

-The following image shows the delays of different cells

<img width="1050" height="682" alt="Screenshot 2026-06-05 at 9 05 19 PM" src="https://github.com/user-attachments/assets/0710a3ea-713a-4f02-941f-e64ba944f5cf" />

-We will further have to decode where these delay come from?


## NMOS Structure and Working

An NMOS transistor consists of:

- P-type substrate
- Heavily doped n+ source and drain regions
- Thin oxide layer
- Gate terminal
- Source, drain, gate, and body terminals

When the gate voltage is zero, no conducting channel exists between source and drain. When a positive gate-to-source voltage is applied, electrons start accumulating near the oxide-substrate interface.


<img width="910" height="496" alt="Screenshot 2026-06-05 at 9 14 30 PM" src="https://github.com/user-attachments/assets/6d858885-847e-4cd4-9640-18a2bbecedde" />

---

## Threshold Voltage and Strong Inversion

As the gate voltage increases, the surface of the p-type substrate starts getting inverted. At a certain gate voltage, a conducting n-type channel is formed between source and drain.

This gate voltage is called the threshold voltage.

When the channel is strongly formed, the MOSFET is said to be in strong inversion.

Important points:

- If Vgs < Vt, NMOS is OFF.
- If Vgs > Vt, NMOS is ON.
- Threshold voltage depends on body bias and technology parameters.

This Image shows NMOS Threshold Voltage calculation with body grounded-
<img width="910" height="496" alt="Screenshot 2026-06-05 at 9 16 36 PM" src="https://github.com/user-attachments/assets/5be8310e-bc00-4980-a61e-bf248395ae76" />

This is when body substrate is biased-
<img width="954" height="488" alt="Screenshot 2026-06-05 at 9 59 18 PM" src="https://github.com/user-attachments/assets/73e3d6af-41c6-4574-afe3-7974cfc2194e" />



## Id vs Vds Characteristics

The drain current of an NMOS depends on gate-to-source voltage and drain-to-source voltage.

The main operating regions are:

1. Cut-off Region  
   The transistor is OFF and drain current is almost zero.

2. Linear / Resistive Region  
   The transistor behaves like a voltage-controlled resistor.

3. Saturation Region  
   The drain current becomes almost independent of Vds.

Condition for linear region:

text Vds < Vgs - Vt 

Condition for saturation region:

text Vds >= Vgs - Vt 

All equation are derived using semiconductor phyics-
<img width="954" height="543" alt="Screenshot 2026-06-05 at 10 02 15 PM" src="https://github.com/user-attachments/assets/051ae0bd-3a2a-42f3-9199-bb64d2c0f196" />

---

## Lab: NMOS Id-Vds Simulation using Sky130 Models

In this lab, NMOS drain current was simulated by sweeping Vds for different values of Vgs.

### Simulation Setup

- Technology: Sky130nm
- Tool: Ngspice
- Device: NMOS
- Analysis: DC sweep
- Output observed: Id vs Vds

### SPICE Simulation Command

spice .dc Vds 0 1.8 0.1 Vgs 0 1.8 0.2 

## Spice Simulation involves two major components-

# 1.Spice Netlist-
<img width="954" height="543" alt="Screenshot 2026-06-05 at 10 06 56 PM" src="https://github.com/user-attachments/assets/682cd179-c567-4592-a155-af26dc69837f" />
# 2.Technology File-
<img width="954" height="543" alt="Screenshot 2026-06-05 at 10 08 43 PM" src="https://github.com/user-attachments/assets/90dbb9c0-f405-49e6-8b30-d88648760d6a" />


### Observation(Lab Work)

The obtained graph shows different Id-Vds curves for different gate voltages. As Vgs increases, the drain current also increases. For lower Vds, the device operates in the linear region, and for higher Vds, it enters the saturation region.
<img width="641" height="567" alt="WhatsApp Image 2026-06-05 at 22 57 04" src="https://github.com/user-attachments/assets/aed6d0c3-6d20-433c-8007-954727f2c6bb" />

---

# Day 2: Velocity Saturation and CMOS Inverter VTC

## Short Channel Effects

In short-channel MOSFETs, the behavior is different from long-channel devices. Even when the W/L ratio is kept same, the current may not scale exactly as expected because of short-channel effects.

Important short-channel effects include:

- Velocity saturation
- Channel length modulation
- Drain-induced barrier lowering
- Mobility degradation


## Velocity Saturation

In long-channel devices, carrier velocity increases almost linearly with electric field. But in short-channel devices, at high electric fields, carrier velocity reaches a maximum value and saturates.

This effect is called velocity saturation.

Because of velocity saturation:

- Drain current becomes more linear with Vgs
- Saturation current does not increase as expected
- Short-channel devices behave differently from ideal long-channel MOSFETs

## To compensate Velocity saturation Effect In Short Channel we come up with this model-
<img width="954" height="543" alt="Screenshot 2026-06-05 at 10 12 36 PM" src="https://github.com/user-attachments/assets/39890d28-7e47-4409-95e9-003e96e5bb13" />



## Id vs Vgs Simulation

In this simulation, Vgs was swept while keeping Vds constant. The purpose was to observe how drain current changes with gate voltage.

### Simulation Setup

- Technology: Sky130nm
- Tool: Ngspice
- Device: NMOS
- Analysis: DC sweep
- Output observed: Id vs Vgs

### Observation(LAB work)

## The Id-Vgs curve shows that drain current increases as gate voltage increases. For short-channel devices, the curve shows more linear behavior at higher gate voltages due to velocity saturation.
<img width="1250" height="825" alt="Screenshot 2026-06-05 at 11 35 26 PM" src="https://github.com/user-attachments/assets/e1f9db4d-b85f-4739-babf-60f6cc52e6b5" />



## CMOS Inverter Voltage Transfer Characteristics

A CMOS inverter consists of one PMOS and one NMOS transistor.

- PMOS is connected between VDD and output.
- NMOS is connected between output and ground.
- Both gates are connected together as input.
- Both drains are connected together as output.

Operation:

- When Vin = 0, PMOS is ON and NMOS is OFF, so Vout = VDD.
- When Vin = VDD, NMOS is ON and PMOS is OFF, so Vout = 0.
  
<img width="954" height="600" alt="Screenshot 2026-06-05 at 10 14 28 PM" src="https://github.com/user-attachments/assets/64f0b097-9f3c-4a0e-aa05-9c728c915740" />

## The voltage transfer characteristic shows how output voltage changes with input voltage.

<img width="954" height="611" alt="Screenshot 2026-06-05 at 10 15 07 PM" src="https://github.com/user-attachments/assets/e650a441-2954-4325-8178-66dffb3d91c7" />

## After Superimpostion-

<img width="954" height="618" alt="Screenshot 2026-06-05 at 10 16 25 PM" src="https://github.com/user-attachments/assets/6ac6481f-132a-4cea-af76-ac814227da00" />


# Day 3: CMOS Switching Threshold and Dynamic Simulation

## CMOS Inverter SPICE Deck

The CMOS inverter SPICE deck contains the circuit connectivity information. It includes PMOS, NMOS, supply voltage, input voltage, model files, and simulation commands.

Basic MOSFET terminal order in SPICE:

text Drain Gate Source Substrate 

Example structure:

spice M1 out in vdd vdd pmos_model W=... L=... M2 out in 0   0   nmos_model W=... L=... Vdd vdd 0 1.8 Vin in 0 0 .dc Vin 0 1.8 0.01 
<img width="962" height="477" alt="Screenshot 2026-06-05 at 10 17 37 PM" src="https://github.com/user-attachments/assets/706fe487-7228-4572-a852-cee5c0e4f1b4" />


## Static Simulation of CMOS Inverter

In static simulation, the input voltage is swept from 0V to VDD and the output voltage is observed.

### Simulation Setup

- Circuit: CMOS inverter
- Analysis: DC sweep
- Input: Vin from 0V to 1.8V
- Output: Vout

### Observation

## The output voltage decreases as input voltage increases. The curve obtained is the voltage transfer characteristic of the CMOS inverter.
<img width="706" height="540" alt="WhatsApp Image 2026-06-05 at 22 57 04 (4)" src="https://github.com/user-attachments/assets/d153a61e-cf0f-4f32-9a83-1bb15c4bac58" />


## Switching Threshold Vm

The switching threshold, Vm, is the point where:

text Vin = Vout 

At this point, both PMOS and NMOS conduct current. The switching threshold depends on the strength ratio of PMOS and NMOS.

If PMOS width is increased, PMOS becomes stronger and the switching threshold shifts.

<img width="919" height="569" alt="Screenshot 2026-06-05 at 10 20 28 PM" src="https://github.com/user-attachments/assets/69826e25-aa61-4525-ba78-d0af586d8ae8" />

---

## Transient Analysis and Delay Calculation

In transient analysis, a pulse input is applied to the CMOS inverter and output response is observed with respect to time.

Important delay parameters:

- Rise delay
- Fall delay

Delay is usually calculated at 50% of input and output voltage levels.

### Rise Delay

text Rise Delay = Time when output rises to 50% - Time when input falls to 50% 

### Fall Delay

text Fall Delay = Time when output falls to 50% - Time when input rises to 50% 

## We try to prove robustness of CMOS inverter through Delay and Rise time of two MOS with Different Vm-
<img width="919" height="497" alt="Screenshot 2026-06-05 at 10 23 04 PM" src="https://github.com/user-attachments/assets/bc482da1-3e52-4446-b4a1-c334d5b12440" />


### Observation

## From transient simulation:
<img width="944" height="805" alt="WhatsApp Image 2026-06-05 at 22 57 04 (8)" src="https://github.com/user-attachments/assets/7f0dfe16-f5f0-4354-95ad-1755267877ff" />


---

# Day 4: Noise Margin Analysis

## Introduction to Noise Margin

Noise margin is the ability of a digital circuit to tolerate unwanted noise without changing the correct logic output.

A higher noise margin means the circuit is more robust.

There are two types of noise margins:

- Noise Margin High, NMH
- Noise Margin Low, NML
---

## Noise Margin Parameters

The important voltage parameters are:

- VOH: Minimum output voltage recognized as logic HIGH
- VOL: Maximum output voltage recognized as logic LOW
- VIH: Minimum input voltage recognized as logic HIGH
- VIL: Maximum input voltage recognized as logic LOW

Noise margin equations:

text NMH = VOH - VIH NML = VIL - VOL 


<img width="919" height="497" alt="Screenshot 2026-06-05 at 10 23 48 PM" src="https://github.com/user-attachments/assets/3a340d14-27fb-4285-82d6-c7e614d3ef95" />

---

## Noise Margin Calculation using VTC

Noise margin is calculated from the CMOS inverter VTC curve. The points where slope is equal to -1 are used to find VIL and VIH.

## We try to prove robustness through Noise Mragin Too-
<img width="919" height="484" alt="Screenshot 2026-06-05 at 10 26 54 PM" src="https://github.com/user-attachments/assets/1f1a4275-3fa1-4020-84f8-b4d3f109e092" />

### Observation

## From my simulation:
<img width="1600" height="768" alt="WhatsApp Image 2026-06-05 at 23 20 42" src="https://github.com/user-attachments/assets/01fe84fd-2bb4-426f-bfb3-122197d415aa" />
<img width="323" height="106" alt="WhatsApp Image 2026-06-05 at 23 20 43" src="https://github.com/user-attachments/assets/1016667c-6e7a-4f65-beaf-82486c612f01" />

---

# Day 5: Power Supply and Device Variation Analysis

## Power Supply Variation

In this simulation, the supply voltage was varied and the VTC of CMOS inverter was observed.

The purpose was to check whether the CMOS inverter remains functional for different values of supply voltage.

<img width="919" height="571" alt="Screenshot 2026-06-05 at 10 31 54 PM" src="https://github.com/user-attachments/assets/5f1e3545-8770-4f08-9f57-895487a8d255" />

## Advantages and Disadvantages of Power Supply Scaling-
<img width="919" height="571" alt="Screenshot 2026-06-05 at 10 33 11 PM" src="https://github.com/user-attachments/assets/d643a944-7286-4fe5-90f8-ca6ce0257ce8" />


### Observation

As supply voltage decreases:

- Dynamic power reduces
- Delay increases
- Switching behavior changes
- Circuit becomes slower
  
<img width="1600" height="759" alt="WhatsApp Image 2026-06-05 at 23 20 43 (1)" src="https://github.com/user-attachments/assets/1c5eac9f-fb48-4b45-a8aa-4701082f839d" />


---

## Device Variation

Device variation occurs during fabrication because of changes in parameters like:

- Channel length
- Channel width
- Oxide thickness
- Threshold voltage
- Mobility

Different process corners can make PMOS or NMOS stronger/weaker.

## Due to Itching Process-
<img width="919" height="482" alt="Screenshot 2026-06-05 at 10 34 47 PM" src="https://github.com/user-attachments/assets/b427b43e-333f-4608-bde5-76afd5e969ff" />

## Due to Oxide Thickness-
<img width="919" height="482" alt="Screenshot 2026-06-05 at 10 35 36 PM" src="https://github.com/user-attachments/assets/1d984d6d-96e7-4545-bd93-80df0fd5a2d9" />

Examples:

- Strong PMOS and weak NMOS
- Weak PMOS and strong NMOS
- Typical corner

<img width="919" height="621" alt="Screenshot 2026-06-05 at 10 36 04 PM" src="https://github.com/user-attachments/assets/e3431f16-0840-40a7-b0ec-de05936f5628" />


### Observation

When PMOS becomes stronger, the switching threshold shifts towards the right.  
When NMOS becomes stronger, the switching threshold shifts towards the left.

## Despite device variations, the CMOS inverter still shows robust logic behavior.
<img width="919" height="609" alt="Screenshot 2026-06-05 at 10 39 32 PM" src="https://github.com/user-attachments/assets/39f9a9ae-b6aa-443d-9eca-854e0bdab205" />

---

# Conclusion

In this workshop, I learned the fundamentals of CMOS circuit design and SPICE simulation using Sky130nm technology.

Key learnings:

- NMOS and PMOS device operation
- Threshold voltage and strong inversion
- Linear and saturation regions of MOSFET operation
- SPICE netlist creation
- Id-Vds and Id-Vgs simulations
- CMOS inverter voltage transfer characteristics
- Switching threshold calculation
- Rise and fall delay measurement
- Noise margin calculation
- Effect of supply and device variations

This project helped me understand how circuit-level simulations are used in VLSI design and how CMOS cells are characterized before being used in digital design flows.

---

# References

- Sky130 open-source PDK
- Ngspice documentation
- CMOS circuit design workshop material
- VLSI system design learning resources
