# Camilla - CAMbio Intrinsicamente LAborioso

Camilla is the software for Policumbent's prototypes' translating gearbox.

## Introduction

## Functioning modes

This software has three modes and a non-default mode:
- [Gears Mode](#gears-mode)
- [While-Pressed Mode](#while-pressed-mode)
- [Nevada Mode](#nevada-mode)
- [Disabled Default Mode](#disabled-default-mode)

### Gears Mode

```C++
// settings.h

#define DEFAULT_MODE_GEARS  1
#define DEFAULT_MODE        DEFAULT_MODE_GEARS

#define NEVADA_MODE         0
```

Puts the gearbox in a semi-automatic mode: when the driver pushes the shift
buttons, the gearbox moves in order to change one gear.

### While-Pressed Mode

```C++
// settings.h

#define DEFAULT_MODE_WHILE_PRESSED  2
#define DEFAULT_MODE                DEFAULT_MODE_WHILE_PRESSED
```

Puts the gearbox in a manual mode: the driver has to keep the desired shift
button pressed until the gearbox arrives on the successive (or previous) gear.

### Nevada Mode

```C++
// settings.h

#define DEFAULT_MODE_GEARS  1
#define DEFAULT_MODE        DEFAULT_MODE_GEARS

#define NEVADA_MODE         1
```

It's an hybrid mode between the semi-automatic and the manual: it has a
semi-automatic behaviour when the desired shift button is pressed and released
as for the Gears Mode, but if the driver keeps it pressed, the gearbox enters in
a kind of While-Pressed Mode, where the driver can manually adjust the position.
The manual-added offset, then, will be added for each shift as overshoot (or
undershoot).

### Disabled Default Mode

```C++
// settings.h

#define DEFAULT_MODE_DISABLED   0
#define DEFAULT_MODE            DEFAULT_MODE_DISABLED
```

In this mode, you can select one of the three previous mode at startup. You can
also reach this mode, from every other mode, by holding down the calibration
button.