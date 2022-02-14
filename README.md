# RFBlindsRevEng

Reverse Engineering the DC3100 RF Blinds Controller
==================

Device Function:
------------
The DC3100 is a 15 channel double control emitter designed for controlling electric blinds. The device supports pairing with multiple motor types such as the DC1680 and DC136. The images below show the structure of the PCB. Interestingly, the device does not have a dedicated antenna, and I presume this is integrated into the PCB as a whole.

Front of PCB
-----------
![image](https://user-images.githubusercontent.com/83759501/153846638-e266a4c1-e03b-44d0-939b-0448fe73ca73.png)

Reverse of PCB
-----------
![image](https://user-images.githubusercontent.com/83759501/153846796-b7e0524b-0b61-4ec2-a649-af44f661eba2.png)

The JTAG-like pins suggest it is possible to read / program the firmware of the microcontroller remotely. This was most likely used by the manufacturer to program the device initially. The chip is an 8bit 32Mhz RISC based PIC16(L)F7902 microcontroller manufactured by Microchip. The full data sheet can be found at the following address.

http://ww1.microchip.com/downloads/en/DeviceDoc/41364E.pdf

The battery is a 3V button battery manufactured by Duracell and the PCB has no other obvious markings other than DC93AA V.1.0 Designed by D-Team.

RF Characteristics
==================

The device operates at ~433.931 Mhz with a transmission power of approximately 11 milliwatts. Messages use pulse width modulation to achieve a binary message frame. Was used to make a recording of the signal as can be seen below.

![image](https://user-images.githubusercontent.com/83759501/153846953-0a664756-1707-4f72-98c4-64c03659e274.png)

Message Structure
=================

The device uses no encryption mechanism to prevent interception, as would be expected so it was trivial to extract the messages directly from the RF recordings. In terms of structure, the messages consist of approximately 6 bytes. From analysing the different messages, it is possible to determine that each frame has the following components.

| Pre-Amble | Device Type Message | Channel | Command |
| --- | ----------- | --------- | ------- | 
| 11111111 | 0011001100110001110010010010 | 1111 | 01010101 | 

By pressing the buttons on the device and recording the different message types we were able to identify how the underlying protocol works.

Side-by-Side Comparison of Message Frames
------------
![image](https://user-images.githubusercontent.com/83759501/153847361-01b67dac-1e2b-48d5-a365-f118d2d76209.png)


Message Preamble
-----------

Each message starts with 8 bits of 1s, presumably as a preamble for the receiver to listen for. In our testing the preamble was consistent with all messages sent.

Message Commands
------------

From recording each button press, it is possible to determine that the remote sends the following command messages.







