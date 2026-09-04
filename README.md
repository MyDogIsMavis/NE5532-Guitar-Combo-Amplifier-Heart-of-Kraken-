# Heart of Kraken

Heart of Kraken is a fully custom analog guitar combo amplifier designed and simulated from scratch.

The project was created to deepen my understanding of operational amplifiers, analog audio circuit design and PCB layout while developing a practical amplifier that can be used both with an electric guitar and as a compact audio power amplifier for a 6 Ω loudspeaker.

Unlike many hobby projects based on existing schematics, the entire architecture, calculations, simulations and PCB layout were developed independently.

## Table of Contents

- [Project Goals](#project-goals)
- [Features](#features)
- [Hardware Architecture](#hardware-architecture)
- [Circuit Description](#circuit-description)
  - [Power Supply](#power-supply)
  - [First Gain Stage](#first-gain-stage)
  - [Active 3-Band Equalizer & Summing Amplifier](#active-3-band-equalizer--summing-amplifier)
  - [Second Gain Stage](#second-gain-stage)
  - [Class AB Output Stage](#class-ab-output-stage)
- [PCB Design](#pcb-design)
- [Design Iterations](#design-iterations)
  - [Revision 1](#revision-1)
  - [Problem & Investigation](#problem--investigation)
  - [Revision 2](#revision-2)
    - [Output Stage Redesign](#output-stage-redesign)
    - [Control Potentiometers](#control-potentiometers)
    - [High-Frequency Decoupling](#high-frequency-decoupling)
    - [Component Footprints](#component-footprints)
    - [Power and Ground Routing](#power-and-ground-routing)
    - [Test Points](#test-points)
    - [High-Current Routing](#high-current-routing)

## Project Goals

* Learn practical analog circuit design
* Gain hands-on experience with operational amplifiers
* Design an active three-band equalizer
* Develop a Class AB transistor power amplifier
* Design a manufacturable PCB in Altium Designer
* Verify the circuit using NI Multisim
* Produce approximately 5-6 W into a 6 Ohm speaker

## Features

|Parameter|Value|
|-|--------|
| Supply Voltage  | 24 V DC                         |
| Input           | 6.35 mm Jack                    |
| Output          | Speaker terminal / 6.35 mm Jack |
| Load            | 6 Ohm                             |
| Output Power    | ~5-6 W                          |
| Amplifier Type  | Analog                          |
| Output Stage    | Class AB                        |
| Op-Amp          | NE5532                          |
| Equalizer       | Active 3-band                   |
| PCB             | 2-layer                         |
| Design Software | Altium Designer                 |
| Simulation      | NI Multisim                     |
| Calculations    | MathCAD                         |

## Hardware Architecture

<p align="center">
  <img src="images/stages_amp.drawio.drawio.svg" width="900" alt="High-level functional block diagram">
</p>

<p align="center">
Figure 1. High-level functional block diagram of the amplifier
</p>

The amplifier consists of a first gain stage built around an NE5532 operational amplifier, followed by an active three-band equalizer, a summing amplifier, a second gain stage and a complementary Class AB output stage based on BD911 and BD912 transistors. The entire circuit operates from a single 24 V DC supply with a virtual ground reference for the analog stages.

## Circuit Description

The amplifier consists of several functional blocks that process the audio signal from the input connector to the loudspeaker while operating from a single 24 V DC power supply.

### Power Supply

The amplifier operates from a single 24 V DC power supply.

<p align="center">
  <img src="images/power-supply.png" width="900" alt="Power supply schematic">
</p>
<p align="center">
Figure 2. Power Supply </p>

Since the analog circuitry is built around operational amplifiers while only a single supply voltage is available, a dedicated virtual ground generator creates a stable mid-supply reference used by all low-level analog stages. This approach allows the amplifier to process bipolar AC signals without requiring a dual power supply.

The power distribution network separately supplies the analog circuitry and the output stage while providing local decoupling capacitors near each active device.

### First Gain Stage

The first gain stage is built around one section of the NE5532 operational amplifier.

<p align="center">
  <img src="images/first-stage.png" width="900" alt="First gain stage schematic">
</p>
<p align="center">
Figure 3. First Gain Stage </p>

Its primary function is to amplify the low-level signal from the guitar before further processing. The voltage gain is adjustable using a potentiometer placed in the negative feedback loop, allowing the amplifier to provide anything from a clean signal to moderate overdrive.

Placing the gain control before the equalizer allows the tone control stage to shape both clean and distorted signals.

### Active 3-Band Equalizer & Summing Amplifier

The tone control section is implemented as an active three-band equalizer based on first-order active filters providing independent adjustment of bass, midrange and treble frequencies.

<p align="center">
  <img src="images/equalizer-and-summing-amplifier.png" width="900" alt="Active 3-band equalizer and summing amplifier">
</p>
<p align="center">
Figure 4. Active 3-Band Equalizer & Summing Amplifier </p>

During the design process it became apparent that directly connecting the first-order filter outputs to the summing amplifier caused undesirable interaction due to their relatively high output impedance.

This issue was resolved by introducing voltage followers after each filter stage. The buffers provide high input impedance and low output impedance, effectively isolating the filter stages and preventing unwanted interaction before the summing amplifier.

The resistor values at the amplifier inputs were optimized through simulation to achieve balanced equalizer operation.

### Second Gain Stage

After passing through the equalizer, the signal level is reduced due to filter attenuation.

<p align="center">
  <img src="images/second-stage.png" width="900" alt="Second gain stage schematic">
</p> 
<p align="center">
Figure 5. Second Gain Stage </p>

The second gain stage restores the required voltage swing before driving the output stage.

### Class AB Output Stage

The output stage is implemented as a complementary Class AB emitter follower using BD911 and BD912 bipolar transistors.

Its purpose is to provide the current required to drive a 6 Ohm loudspeaker while preserving the voltage waveform generated by the preceding stages. Class AB operation offers a good compromise between efficiency, output power and crossover distortion, making it suitable for compact analog audio amplifiers.

The amplifier is designed to deliver approximately 5-6 W of output power from a single 24 V supply.

<p align="center">
  <img src="images/class-ab-output-stage.png" width="900" alt="Class AB output stage schematic">
</p>
<p align="center">
Figure 6. Class AB Output Stage </p>

## PCB Design

The PCB was designed from scratch in Altium Designer based on the final circuit schematic.

The board layout was organized according to the functional blocks of the amplifier, with the signal path arranged from the input stage to the output stage. Particular attention was given to component placement, grounding and routing of the higher-current output stage.

<p align="center">
  <img src="images/pcb.png" width="900" alt="PCB layout">
</p>
<p align="center">
Figure 7. PCB all layers</p>

The PCB layout follows the signal flow from the input stage to the output stage. Low-level signal traces are kept as short as practical to reduce the possibility of unwanted noise pickup.

The PCB uses separate signal and power ground paths to reduce the influence of output-stage currents on the low-level analog circuitry. The two ground paths are connected at a single point near the negative terminal of the 2200 µF C1 filter capacitor in the power supply circuit.

The power supply and Class AB output stage are located in the upper part of the PCB. The signal processing and virtual ground sections are located in the lower part of the PCB.

The output provides two connectors for the loudspeaker: a screw terminal XS3 for connecting speaker wires with tinned ends, and a 6.35 mm jack XS2 for connecting an external guitar cabinet.

The project logo, "HEART OF KRAKEN", is placed in the center of the PCB.

<p align="center">
  <img src="images/3d-model-top.png" width="900" alt="3D model of the PCB - top view">
</p>
<p align="center">
Figure 8. 3D model of the PCB - top view </p>

<p align="center">
  <img src="images/3d-model-bottom.png" width="900" alt="3D model of the PCB - bottom view">
</p>
<p align="center">
Figure 9. 3D model of the PCB - bottom view </p>

Manufacturing files were generated from Altium Designer and the PCB was manufactured by JLCPCB.

<p align="center">
  <img src="images/manufactured-pcb.jpg" width="900" alt="Manufactured PCB">
</p>
<p align="center">
Figure 10. Manufactured PCB </p>

## Design Iterations

### Revision 1

The first PCB revision was manufactured and assembled to verify the amplifier design in hardware.

The amplifier successfully powered up and reproduced an audio signal through the loudspeaker. However, the BD911 and BD912 output transistors became extremely hot during operation. Their temperature increased to a level where touching the transistor cases was unsafe.

After the input signal was removed, the amplifier remained powered in an idle state. After a few minutes, the output transistors overheated further, smoke appeared, and the transistors failed.

After replacing the damaged transistors with a new pair, they failed almost immediately, even without an input signal.

### Problem & Investigation

The output stage was operating with excessive current, resulting in severe heating of the BD911 and BD912 transistors.

The fact that the transistors also overheated without an input signal indicated a problem with the quiescent operating point of the Class AB output stage.

The output stage was inspected and the voltages and current paths were analyzed.

The problem was traced to the placement of the 0.22 Ohm emitter resistors in the bias circuit. In the original design, the resistors did not provide the intended current feedback in the output transistor emitter paths.

### Revision 2

Based on the results of the first hardware revision, several changes were made to the amplifier circuit and PCB layout.

| Area | Revision 1 | Revision 2 |
|---|---|---|
| Output stage | Initial Class AB design | Redesigned Class AB output stage |
| Emitter resistors | 0.22 Ohm, incorrect placement | 0.47 Ohm, placed in the emitter paths |
| Gain / Master controls | Reversed control direction | Corrected clockwise operation |
| SMD packages | 0805 | 1206 |
| Power connection | Twisted-pair wires | PCB traces |
| HF decoupling | Initial configuration | Additional 100 nF capacitor |
| Test points | Not included | +24 V, GND, Virtual GND |
| +24 V routing | Initial layout | Shorter high-current path |

#### Output Stage Redesign

The Class AB output stage was redesigned to address the excessive heating observed during testing of Revision 1.

The 0.22 Ohm resistors were moved from the bias circuit into the emitter paths of the BD911 and BD912 output transistors. Their value was also increased from 0.22 Ohm to 0.47 Ohm.

The emitter resistors provide local negative feedback and help stabilize the output-stage current.

#### Control Potentiometers

The pin connections of the Gain and Master potentiometers were corrected.

In Revision 1, both controls operated in the opposite direction: turning the potentiometers clockwise reduced the corresponding level.

The connections were changed so that both controls now increase the corresponding level when turned clockwise.

#### High-Frequency Decoupling

A 100 nF capacitor (C15) was added at the NPN transistor collector/supply node to provide additional high-frequency decoupling and reduce the possibility of high-frequency noise on the power rail.

#### Component Footprints

The SMD footprints were changed from 0805 to 1206.

The larger package size makes the components easier to place, inspect and hand-solder during prototyping and rework.

#### Power and Ground Routing

In Revision 1, the connection between the power supply section around the C1 filter capacitor and the power switch/indicator section was made using twisted-pair wires. This was initially intended to reduce possible noise coupling.

For Revision 2, this connection was moved onto the PCB and routed directly as copper traces.

Local 100 nF ceramic bypass capacitors are placed near the operational amplifiers, at the power supply output and near the output transistors to provide local high-frequency decoupling.

#### Test Points

Three dedicated test points were added to simplify hardware testing and debugging:

- +24 V supply
- GND
- Virtual GND

These test points allow the main supply and reference voltages to be measured directly on the assembled PCB.

#### High-Current Routing

The connection between the +24 V supply rail and the local amplifier supply rail was routed on the top layer.

This routing was chosen to keep the high-current path from the VT2 output transistor to the negative terminal of the main C1 filter capacitor as short as practical.

<p align="center">
  <img src="images/schematic_ver2.svg" width="900" alt="Schematic of amplifier rev.2">
</p>

<p align="center">
Figure 12. Schematic of the amplifier - Revision 2
</p>

<p align="center">
  <img src="images/pcb_ver2.png" width="900" alt="PCB layout of Revision 2">
</p>
<p align="center">
Figure 11. PCB layout - Revision 2
</p>

The second revision was manufactured and is currently awaiting hardware testing.
