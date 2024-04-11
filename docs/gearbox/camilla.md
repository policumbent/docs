# Camilla - CAMbio Intrinsicamente LAborioso

Camilla is the software for Policumbent's prototypes' translating gearbox.

## Introduction

## Functioning modes

This software has three modes:
- [Gears Mode](#gears-mode)
- [While-Pressed Mode](#while-pressed-mode)
- [Nevada Mode](#nevada-mode)

### Gears Mode

Puts the gearbox in a semi-automatic mode: when the driver pushes the shift
buttons, the gearbox moves in order to change one gear.

### While-Pressed Mode

Puts the gearbox in a manual mode: the driver has to keep the desired shift
button pressed until the gearbox arrives on the successive (or previous) gear.

### Nevada Mode

It's an hybrid mode between the semi-automatic and the manual: it has a
semi-automatic behaviour when the desired shift button is pressed and released
as for the Gears Mode, but if the driver keeps it pressed, the gearbox enters in
a kind of While-Pressed Mode, where the driver can manually adjust the position.
The manual-added offset, then, will be added for each shift as overshoot (or
undershoot).