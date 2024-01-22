# GARGLER
This Eurorack module provides four different outputs with chaotic CV values.
It is based on an idea derived from the 8 Bit Cipher module from NonLinearCircuits.

Features:
- Four different (but not independent) CV outputs with a range from 0V to 10V
- Separate controls for portamento and attenuation for each CV output
- A clock input to drive the module
- Two optional inputs from which a 16-stage shift register is influenced and the CV output values are being derived
- Buttons to add or delete single bits, influencing the CV values
- A control to stop the sequence of CV outputs
- A switch to lock the 16-stage shift register sequence

## How the module works
The module design is purely analog. The core of the module are two CD4094 8-stage shift register in series.
Those are fed by the outcome of an op amp comparator.

The CV values for each output channel are different, but not independent. Each CV value is determined by eight different stage values, with each stage influencing two different channels.
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

## Module Build and PCBs
I added two different versions for the control board here, an "original", and a "Thonk" version.
Reason is that for my own module, I am using specific potentiometers - 16K4 series from Supertech Electronics - and 3.5mm jack sockets - MJ-355 from Marushin - available at my local electronics shop.

However, since most DIY projects for Eurorack modules out there are using potentiometers from ALPHA and so-called THONKICONN jacks, as they are provided by Thonk in the UK, I also created another control board PCB for the "Thonk" version with footprints for those components.

I created the Gerber files with the online tool EasyEDA and ordered it at JLCPCB.

## Panel Layout
I added the information about hole coordinates for the front panel in the folder PanelLayout, referring to the component layout in the PCB Gerber files.

In addition, there is another Gerber file for the panel, following the HP standard. My own modules do not follow that width standard, as I am only using sliding nuts in my racks.

You can use the panel Gerber file to have the panel built out of PCB material.

## Additional Information about specific Components
The module build is mainly THT, including all ICs.
However, there are a few SMD components if you are using the gerber files provided.

There are a number SMD capacitors with the package size 1608 (imperial 0603), as well as several NPN transistors MMBT3904 with package size SOT-23-3.

Concerning the resistor size, I am usually using small-size resistors, about half the length of the usual size, so they need less space on the PCB. If you want to use my Gerber files, you have to consider that fact. You might still use normal size resistors and put them in a standing position on the boards. Should also work fine.

## Soldering ICs on Control Board
There are three ICs to be soldered on the control board in a special way, although the ones to be used are THT.
Due to space issues, there are no holes for the IC sockets.
The socket legs need to be soldered to the PCB surface, like SMD components.

In order to do that, you need to use those types of sockets, where the legs can be bent, NOT the ones with stiff round legs!

<img width="500" src="https://github.com/TOILmodular/GARGLER/assets/97026614/440ea4bd-f64a-49aa-b3e9-e93ae8265c44">

All socket legs need to be bent to the outside, so it can be put flat onto the PCB. Soldering the sockets to the board is very easy, since the legs and the space inbetween are big compared to real SMD components.
