# <div align="center">Microphone Adapter</div>

[Link to project page on AJ6ME Radios Website](https://sites.google.com/aj6me.com/aj6me-radios/microphone-adapter)

This project is not affiliated with Kenwood, nor endorsed by them. All references to the TM-D710GA here only indicate the personal radio the author originally designed the project for.

## Introduction
Standard solutions to enable external headsets for the TM-D710GA and similar amateur radios involve using adapter harnesses and/or large mechanical switch boxes. The handheld microphone's buttons (necessary for VFO direct entry) are not usable at the same time as the headset.

This project allows simultaneous connection of both a standard handheld microphone and a headset mic (3.5mm)/PTT (1/4") combination. A solid state audio switch automatically switches between the two microphone inputs depending on which PTT is pressed. 

The board above will be housed in an extruded aluminum enclosure with endcaps with custom cutouts for the connectors. The aluminum enclosure will be plated in a conductive material for good EMI shielding.

## Implementation

### Radio Connector Pinout (1:1 to Handheld Mic)
| Standard RJ45 Pin # | Kenwood Pin # | Function                    |
| ------------------- | ------------- | --------                    |
| 1                   | 8             | Keypad                      |
| 2                   | 7             | Hook (GND when on hanger)   |
| 3                   | 6             | MIC                         |
| 4                   | 5             | MIC-E (Shield)              |
| 5                   | 4             | PTT                         |
| 6                   | 3             | GND                         |
| 7                   | 2             | SB (8V Power Rail)          |
| 8                   | 1             | No Connect                  |

### Schematic
Allmost all signals are run through 1:1 between the Radio and Handheld Mic connector. The exceptions are:

Shield and board GND are tied together: this is also done on the radio side so there's the potential for a ground loop, but the area is only between two adjacent pairs on an RJ45 cable. As a benefit, this merging of grounds means that the enclosure (conductive aluminum) can be more effectively bonded to the shield/outer contact of all connectors, which should reduce radiated susceptibility.

Hook is not connected (though there's a resistor stuffing option to connect it if you wish). If connected, this would disable all transmit (even headset transmit) when the handheld mic is on its hanger.

The microphone signal is muxed between the handheld mic and the headset mic using a [TS5A22364DGSR](https://datasheet.ciiva.com/1671/ts5a22362-1671776.pdf?src-supplier=Digi-Key) analog switch. This switch is a single-supply (only a single positive 5V supply) analog switch that still allows for negative switched voltages (down to -500mV given the 5V supply) at low THD.

The select input of this analog switch is set using the Headset PTT signal. If the Headset PTT is pressed, the switch will connect the Headset microphone input to the radio. If the Headset PTT isn't pressed (regardless of whether the handheld mic PTT is being pressed) the handheld mic will be connected to the radio. When the headset PTT is pressed, D1 pulls down the main PTT line to key up the radio. When the handheld mic is pressed, the main PTT signal goes low (keying the radio), but because of D1, the Headset PTT line is not pulled low (ensuring the handheld mic is connected).

### Layout
The board is 4 layers. All components are placed on L1, so this is a two-step assembly process of first mounting and reflowing the SMT components, then placing and soldering (by hand, wave, selective wave, etc) the through-hole components. Surface-mount versions of these connectors do exist, but the designer generally avoids them due to mechanical robustness concerns (large connectors only mounted on surface-mount pads tend to be easier to rip out of the board than connectors that are staked through the board).

Audio signals are run (as much as feasible) on Layer 2, where they can be surrounded by GND planes on L1, L2, and L3. Ground Plane stitching vias are used to ensure these three planes stay at the same voltage.

On the two non-connector faces, grounded copper strips are exposed (likely, the sides of the boards there will be plated as well) to enable good connection to the conductive enclosure.

### Enclosure
Vendor selection and prototyping for the enclosure is still ongoing, but will take the form of an aluminum extrusion plus cutout-endplates for the connector faces. Designer is targeting a Trivalent Chromium plating on the enclosure to make it conductive for better connection to the shields of the connectors. When available, manufacturer's drawings will be uploaded.
