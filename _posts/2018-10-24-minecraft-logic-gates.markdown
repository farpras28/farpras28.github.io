---
layout: post
title: Minecraft Logic Gates
category: Random
date: October 24, 2018
hashtags: minecraft,logic,gates
comments: true
---

# Key for Diagrams

![alt text][keyDiagram]

# Logic Gates

**Input/Output Gate:**
![alt text][ioGate]
The most basic gate you can have. When the input signal is on, the output signal is on, and vice versa.

**NOT Gate:**
![alt text][notGate]
A NOT Gate (¬A) also known as inverter, is a gate used when you want an opposite output from the input you give.

**AND Gate:**
![alt text][andGate]
An AND gate (A^B) is used with two or more switches or other inputs. The output is toggled to "on" ONLY when both switches, or inputs, are toggled to "on". Otherwise, the output will remain "off".

**NAND Gate:**
![alt text][nandGate]
A NAND gate -(A^B) is the opposite to the AND gate. The output is toggled to "off" ONLY when both switches are toggled to "on". Otherwise, the output is set to "on". This gate also requires two or more inputs.

**OR Gate:**

An OR gate (AvB) uses two or more inputs. Whenever any input is "on", the output is to "on". The only time the output is "off" is when all inputs are "off".

**NOR Gate:**
![alt text][norGate]
A NOR gate -(AvB) is the opposite of the OR gate. Whenever at least one switch is toggled to "on", the output is toggled to "off". The only time the output is "on" is when all inputs are toggled to "off". This gate also uses two or more inputs.

**XOR Gate:**
![alt text][xorGate]
An XOR gate (A⊕B) is a gate that uses two inputs. In this gate, the output is toggled to "on" when one switch is "on" and one switch is "off". If both switches are in the same position, the output is toggled to "off".

**XNOR Gate:**
![alt text][xnorGate]
An XNOR gate (A⇔B) is the opposite of an XOR gate. It uses two inputs. When both switches are in the same state (both switches are "on" or both switches are "off"), then the output is toggled to "on". Otherwise, if the switches differ, the output is toggled to "off".

**ONLY / NON-IMPLY Gate:**
![alt text][onlyGate]
In this gate, the output is toggled to "on" only when input A is "on" and input B is "off". If input A is "off" and input B is "on", the output will remain "off". If both inputs are "off" or "on", the output will remain "off". This makes this gate useful when needing a specific order of inputs to trigger the output.

**Diodes:**

Diodes prevent power from flowing backwards in a circuit. This can be very useful if you need to isolate an input wire to avoid feedback, or need to merge two inputs into one (such as in the OR gate above).
There are three flavors of diodes: The one-block one (up to four) tick delay repeater, the three-block two tick delay redstone torch repeater(also called a classic or traditional repeater), and the two-block, zero tick delay Glowstone diode.

Reference: [https://minecraft.gamepedia.com/Tutorials/Basic_logic_gates](https://minecraft.gamepedia.com/Tutorials/Basic_logic_gates)

[keyDiagram]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/8/8c/MCGatesKey.png "Key for Diagrams"
[ioGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/8/83/BasicGate.png "I/O Gate"
[notGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/a/a1/NOTgate.png "NOT Gate"
[andGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/4/4c/ANDgate.png "AND Gate"
[nandGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/3/39/NANDgate.png "NAND Gate"
[norGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/8/80/NORgate.png "NOR Gate"
[xorGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/6/6b/XORgate.png "XOR Gate"
[xnorGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/5/51/XNORgate.png "XNOR Gate"
[onlyGate]: https://d1u5p3l4wpay3k.cloudfront.net/minecraft_gamepedia/1/10/ONLYgate.png "ONLY Gate"
