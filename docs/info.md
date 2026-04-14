---
title: "8-Bit Current Steering DAC (255-cell)"
author: "jaycam"
discord: ""
description: "A pure analog 8-bit, 255-cell current steering digital-to-analog converter (CSDAC) using thick-oxide IOs."
---

## How it works

This project implements a full 8-bit Current Steering Digital-to-Analog Converter (CSDAC) utilizing a 255-cell array architecture. 

The design relies on standard 1.8V digital logic for the cell selection and switching, while the core current steering elements utilize 5V thick-oxide devices (`sky130_fd_pr__nfet_g5v0d10v5`). 

The 8-bit digital input is fed through the standard `ui_in` pins, which controls the switching network. Based on the digital input code, a proportional number of the 255 unit cells are switched to route their individual currents to the shared analog output pad (`Iout`), creating a stepped analog current output. 

## How to test

1. Provide standard 1.8V power to the `VPWR` and `VGND` rails to power the control logic.
2. Apply an external bias voltage to the `Vbias` analog pin to set the reference current for the internal cells.
3. Apply an 8-bit digital input word (ranging from `00000000` to `11111111`) across the `ui_in[7:0]` pins.
4. Measure the resulting analog current at the `Iout` analog pin. Sweeping the 8-bit digital input from 0 to 255 should produce a linear staircase current output, which can be used to calculate the DAC's DNL and INL.

## External hardware

Testing this design requires:
* A precise voltage source for the `Vbias` pin.
* A microcontroller, FPGA, or dip-switch board to provide the 8-bit digital stimulus to `ui_in`.
* A Source Measure Unit (SMU) or a Transimpedance Amplifier (TIA) circuit to sink and accurately measure the analog current coming from the `Iout` pin.

## Attribution

This project builds upon open-source frameworks and templates:
* **Analog Layout:** The top-level analog integration uses the [Tiny Tapeout Analog Template](https://github.com/tinytapeout/tt-analog-template) authored by Uri Shaked.
* **Digital Architecture & Verification:** The digital controller wrapper, VGA pattern generation logic, and associated Verilator simulation testbenches are based on the work of Anton Maurovic ([algofoogle](https://github.com/algofoogle)), licensed under the Apache License 2.0.
