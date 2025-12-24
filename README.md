# ELE1001 BCD Keypad Display 

## Overview

This project implements an embedded system that reads a 4×4 matrix keypad and drives a two digit BCD based display using an Arduino. The system converts user input into stable BCD signals for tens and units digits and generates control signals used by external logic modules.

The Arduino replaces the hardware encoder by handling keypad scanning, digit selection, display control, and synchronization signals entirely in software.

---

## Core Functionality

### Keypad Reading

The keypad is read using a matrix scanning method:
- Columns are driven LOW one at a time
- Rows are read using internal pull up resistors
- A key is detected on release, not on press

Detecting key release eliminates contact bounce and prevents repeated inputs when a key is held.

---

### Digit Entry Logic

Numeric keys (0 to 9) are converted to BCD values using a dedicated conversion function.  
The system alternates automatically between:
- Units digit
- Tens digit

This behavior is managed by an internal state variable that switches focus after each valid digit entry.

---

### BCD Output Generation

Each digit is output using four dedicated pins:
- Four pins for the units digit
- Four pins for the tens digit

The Arduino writes each BCD bit directly to the corresponding output pin, producing stable logic levels compatible with external decoders and counters.

---

### Display Blanking

Pressing the `*` key blanks both displays by forcing all BCD outputs to a high impedance like state.  
While blanked:
- No digit updates occur
- Outputs remain disabled

Any valid key input automatically re enables the outputs and restores normal operation.

---

### Validation and Control Signals

#### Z Signal

Each valid numeric key entry generates a short pulse on the Z signal pin.  
This pulse acts as a validation signal and is used by external logic to confirm that a new digit has been entered.

#### `#` Signal

Pressing the `#` key generates a dedicated control pulse indicating that the entered value is final.  
This signal is used to synchronize the start of a counting or processing cycle in downstream modules.

Additionally, pressing `#` copies the tens digit to both displays, ensuring a consistent and intentional final value.

---

## Software Design Choices

- Key release detection instead of key press to improve reliability
- Explicit BCD bit control for hardware compatibility
- Output enable and disable functions for safe display blanking
- Clear separation between input handling, conversion logic, and signal generation

---

## Result

The system provides:
- Reliable numeric input
- Stable BCD outputs
- Clean synchronization signals
- Full replacement of discrete encoding logic using software

This approach improves flexibility, reduces hardware complexity, and allows easy future modifications through firmware changes alone.

---

## Tools Used

- Arduino IDE
- Arduino compatible microcontroller
- External BCD and logic circuitry
