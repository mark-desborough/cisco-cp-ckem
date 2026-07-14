# Pinout for the CP-CKEM-C= Spine

The pin numbering seems reversed on the spine itself (internally visible). These spines were not intentionally damaged. They had a hard life in a bin before I got to them.
The phone side matches the kem for numbering. The phone also includes a label for pin 9 and 10 (both extra legs on the shield).

The raised pads on the spine are fairly easy to lift off. I attempted soldering some wires onto them and they pulled off with (mostly) minimal force.

There is a physical gap between the power pads of approximately 1 extra pad's width. This is how I keep the layout/orientation in my head. Especially with the phone/kem/spine upside-down and backwards.

When booting the phone supplies 36v to the pins 1 (negative) and 2 (positive). Under normal conditions the phone will unlock power supply to the accessories when it has sufficient power budget (either poe or dc power cube).

## Kem
The pins on the KEM numbering are as follows:

```
8) USB 5v+ (VBUS)
7) USB Green D+
6) USB White D-
5) USB Shield
4) USB Black (GND)
3) 1 Wire Device Identification (probably)

2) 36-60V DC+ (Power Positive)

1) 36-60V DC- (Power Negative)
```

## Spine
The spine has the reverse numbering on the pins. They bridge across with a pair of capacitors. I believe the larger capacitors share an isolated ground.
The USB component of the spine is purely for the SHIELD bridge. It does not have any pins inside the connector, and the KEM side is does not have a female jack. 

```
1 <--- small brown capacitor ----  small brown capacitor ---> 1
2 <--- small white capacitor ----  small white capacitor ---> 2
3 <--- small white capacitor ----  small white capacitor ---> 3
4 <--- no capacitors         ----  no capacitors         ---> 4
5 <--- small white capacitor ----  small white capacitor ---> 5
6 <--- small white capacitor ----  small white capacitor ---> 6

7 <--- larger capacitor      ----  larger capacitor      ---> 7

8 <--- larger capacitor      ----  larger capacitor      ---> 8
```

## DC Power pins

The power on my `CP-CKEM-C=` versions (of the 2 I have opened) have an 10-pin flyback transformer: 
- `Pulse PG0738NL` (input voltage is 36-60V)
- `EE 835-01065F 1542 CN2` (seems to be Hang Tung Ltd) in the same configuration as the PG0738NL
