# GARGLER
This Eurorack module provides four different outputs with chaotic, unpredictable CV values.
It is based on an idea derived from the 8 Bit Cipher module from NonLinearCircuits.

Features:
- Four different (but not independent) CV outputs with a range from 0V to 10V
- Separate controls for portamento and attenuation for each CV output
- Clock input to drive the module
- Two optional inputs from which a 16-stage shift register is influenced and the CV output values are being derived
- Controls to add or delete single bits, influencing the CV values
- Control to stop the sequence of CV outputs
- Switch to lock the 16-stage shift register sequence

## How the module works
The module design is purely analog. The core of the module are two CD4094 8-stage shift register in series.
Those are fed by the outcome of an op amp comparator.

The CV values for each channel are different, but not independent. Each CV value is determined by eight different stage values, with each stage influencing two different channels.
This makes the outcome chaotic, but not random.

### IN:
The input signals can come from any type of signal, audio or other CV sources.
Each input, where no cable is plugged in, is fed by an internal feedback connection from the two shift registers via several CD4030 XOR gates.
So the module does not depend on any input.
The feedback logic is slightly different for each input in a way that the bits compared are derived from different shift register stages.

### CLOCK:
The module requires a clock signal (>1V) to output new CV values. 

### STOP:
If there is a cable plugged into the STOP input, the CV outputs will only change to new values, if the input is high (>1V), otherwise the change of CV outputs will stop. If no cable is plugged in, the CV values will change with each clock trigger.

### ADD and DEL:
When pressing the ADD button, and as long as it is kept pressed, the first stage of the shift register will be set to high with each trigger signal into the clock input (provided the sequence is not stopped by a low signal in the STOP input).
Same for the DEL button, which will set the first stage to low.

### Lock Switch:
When pushing the switch at the LED field center to the left, the current 16 stages are being locked and the 16-atep sequence of CV values will be looped.
Influencing the sequence via the ADD and DEL buttons is still possible in this mode.
As soon as the switch is pushed to the right, a new bit value will be determined for the first stage with each clock trigger, moving through the 16 stages with the clock speed.

### PORTAMENTO and ATTENUATOR:
The portamento and attenuator knobs do what they are expected to do.
Those controls are independent for each of the four channels.
