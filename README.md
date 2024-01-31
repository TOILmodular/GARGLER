# GARGLER - a chaotic CV generator Eurorack module
This Eurorack module provides four different outputs with chaotic CV values.
It is based on an idea derived from the 8 Bit Cipher module from NonLinearCircuits.

<img height="500" src="https://github.com/TOILmodular/GARGLER/assets/97026614/3fb18a32-90c7-4129-8e60-ad567214e95c">
<img height="500" src="https://github.com/TOILmodular/GARGLER/assets/97026614/2d934e23-873a-4cbc-9ae3-71b2fb08ed65">

<img height="500" src="https://github.com/TOILmodular/GARGLER/assets/97026614/121e99c1-6281-40e6-89a4-87d81681f247">
<img height="500" src="https://github.com/TOILmodular/GARGLER/assets/97026614/da834276-f82b-410b-8704-74eb76ab6ea8">


#### Features
- Four different (but not independent) CV outputs with a range from 0V to 10V (not evenly distributed)
- Separate controls for portamento and attenuation for each CV output
- A clock input to drive the module
- Two optional external inputs from which a 16-stage shift register is influenced and the CV output values are being derived
- Two internal feedback loops normalled to each of the two input channels, so the module does not depend on any external input signal
- Buttons to add or delete single bits, influencing the CV values
- A control to stop the change of CV outputs
- A switch to lock the current 16 shift register stages and to bring the CV outputs into a 16-step sequence loop

## How the Module works
The core of the module are two CD4094 8-stage shift register ICs in series.
The stage values are derived from a comparison of two input signals (external or internal via feedback loops) and mixed together in different ways to provide CV outputs.

The CV values for each output channel are different, but not independent. Each CV value is determined by twelve different stage values (each of the 16 stages is influencing three of the output channels).
This makes the outcome chaotic, but not random.

The fact that the derived values for each output depend on how many of the related twelve stages are high or low has two consequences for those values:
- There are only twelve possible discrete CV values for each output.
- The probabilities for each of those values are not the same. E.g. a value of 0V would require that all twelve stages are low, while a maximum value of 10V requires that all those stages are high. Due to indistiguishable permutations of stage combinations, the probabilities for 0V and 10V are much lower, and the highest probability is given for middle CV value with six stages being high and six stages being low... well, in theory. The actual values and the probabilities of different stage scenarios depend on the signal inputs, running through the comparator. I also played around with different resistor values around the output op amps to get some more variation in the CV values.

#### IN
The input signals can come from any type of signal, audio or other CV sources.
An internal feedback connection from the two shift registers via several CD4030 XOR gates is fed into each input, if no cable is plugged in.
So the module does not depend on any external input.

The feedback logic is slightly different for each input in a way that the bits compared are derived from different shift register stages.
The resulting pattern from having both feedback loops as the only source (no external signals) is repeating after a number of steps and sometimes showing slight deviations.
That is just my observation. One might find the reason for it by checking the feedback loop logic in detail. I didn't do that. I just fiddled around with different feedback options during the module design phase.

#### CLOCK
The module requires a clock signal (>1V) to output new CV values. 

#### STOP
If there is a cable plugged into the STOP input, the CV outputs will only change to new values, if the input is high (>1V), otherwise the change of CV outputs will stop.

Due to some reason which I did not yet find out, the constellation of active and inactive stages changes, when sending a high signal to the STOP input, compared to the stopped pattern. Sometimes, this results in the deactivation of all stages (all LEDs off) and no further movement. In that case, just press the ADD button to add a new high value to the first stage to ignite a new pattern.

Consider this bug as an additional feature of chaos.

#### ADD and DEL
When pressing the ADD button, and as long as it is kept pressed, the first stage of the shift register will be set to high with each trigger signal into the clock input (provided the sequence is not stopped by a low signal in the STOP input).

Same for the DEL button, except that the first stage will be set to low.

#### Lock Switch
When pushing the switch at the LED field center to the left, the current 16 stages are being locked and the 16-step sequence of CV values will be looped.
Influencing the sequence via the ADD and DEL buttons is still possible in this mode.

As soon as the switch is pushed to the right, the 16 stages are being changed again depending on the input signals (external or internal).

#### PORTAMENTO
The PORTAMENTO knobs do what they are expected to do, i.e. smoothing out the shift from one CV value to the next.
Those controls are independent for each of the four channels.

#### ATTENUATOR

Attenuating a CV output with the related ATTENUATOR knob does not simply reduce all output values from that channel. Due to the uneven probability distribution of possible CV values, as mentioned above, attenuating an output shifts the entire probability distribution to lower values, like squeezing the probability curve with 12 discrete values to a narrower voltage range.

## Some Suggestions about how to use the Module
- The module has the tendency to lock the CV value changes to certain patterns. This can be interrupted by playing around with the ADD and DEL buttons.
- The module invites to use it interactively by adding and deleting single bits or a series of bits (high or low). The outcome can be quite chaotic, and sometines the module returns back to a previous repetition.
- Also the interactive use of the attentuators has quite some influence on CV output probabilities due to the facts described above.
- Changes or repetitions can nicely be observed in the LED field. You can get a general impression if the module tends to higher or lower CV output values (more or fewer LEDs lit). Depending on that, you can add or delete some bits.
- If the left IN signal is higher than the right one at the time of a trigger from the CLOCK input, the first bit of the shift register will be set to high. If the right IN signal is higher, it will be set to low. Keeping that in mind, you can experiment with different input signals on each side - noise, audio, LFO, CV, trigger, internal feedback.

## Module Build and PCBs
I added two different versions for the control board in the folder GerberFiles, an "original", and a "Thonk" version.
Reason is that for my own module, I am using specific potentiometers - 16K4 series from Supertech Electronics - and 3.5mm jack sockets - MJ-355 from Marushin - available at my local electronics shop.

<img width="300" alt="CtrlPCB_Orig" src="https://github.com/TOILmodular/GARGLER/assets/97026614/a8fbc0af-805f-4c7b-9b86-065e128c370f">

However, since most DIY projects for Eurorack modules out there are using potentiometers from ALPHA and so-called THONKICONN jacks, as they are provided by Thonk in the UK, I also created another control board PCB for the "Thonk" version with footprints for those components.

<img width="300" alt="CtrlPCB_Thonk" src="https://github.com/TOILmodular/GARGLER/assets/97026614/172d28bf-8240-41f0-a2d5-ef757705ce10">

The main PCB is the same for both versions.

<img width="300" alt="MainPCB" src="https://github.com/TOILmodular/GARGLER/assets/97026614/e4118d6f-81de-4b8a-8c81-1f80d62421d3">

I created the Gerber files with the online tool EasyEDA and ordered the PCBs at JLCPCB.

## Panel Layout
I added the information about hole coordinates for the front panel in the folder PanelLayout, referring to the component layout in the PCB Gerber files.

In addition, there is another Gerber file for the panel, following the HP standard. My own modules do not follow that width standard, as I am only using sliding nuts in my racks.

You can use the panel Gerber file to have the panel built out of PCB material.

## Additional Information about specific Components
The module build is mainly THT, including all ICs.
However, there are a few SMD components if you are using the Gerber files provided.

There are a number SMD capacitors with the package size 1608 (imperial 0603), as well as several NPN transistors MMBT3904 with package size SOT-23-3.

Concerning the resistor size, I am usually using small-size resistors, about half the length of the usual size, so they need less space on the PCB. If you want to use my Gerber files, you have to consider that fact. You might still use normal size resistors and put them in a standing position on the boards. Should also work fine.

## Soldering ICs on Control Board
There are three ICs to be soldered on the control board in a special way, although the ones to be used are THT.
Due to space reasons, there are no holes for the IC sockets.
The socket legs need to be soldered to the PCB surface, like SMD components.

In order to do that, you need to use those types of sockets, where the legs can be bent, NOT the ones with stiff round legs!

<img width="500" src="https://github.com/TOILmodular/GARGLER/assets/97026614/440ea4bd-f64a-49aa-b3e9-e93ae8265c44">

All socket legs need to be bent to the outside, so they can be put flat onto the PCB. Soldering a socket to the board is very easy, since the legs and the space inbetween are big compared to real SMD components.

The sockets on the main board are to be soldered the usual THT way.
