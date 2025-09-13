This is firmware to make a TTGO t-Beam HAB LoRa receiver linked over BLE.  It is my generic firmware but modified to make it easier for an ESP32 client (namely the TTGO T-Watch 2020) to connect over BLE serial.


Libraries
=========

To build LoraBluetooth you may need various libraries into your Arduino IDE.  For boards with an OLED:

	- Adafruit GFX Library
	- Adafruit SSD1306 Library

These Libraries can be added via the Ardunio IDE menu: Sketch -> Include Library -> Manage Libraries…

For boards that use the AXP202X power chip, ie the TTGO V1+ T-Beam, the AXP202X Library is required:

https://github.com/lewisxhe/AXP202X_Library

For boards that use the AXP2101 power chip, ie the T-Beam v1.2, the XPowersLib Library is required:

https://github.com/lewisxhe/XPowersLib

These libraries can be added via the Ardunio IDE menu: Sketch -> Include Library -> Add .zip Library…


Serial Protocol
===============

The same protocol is used over USB, Bluetooth and BLE.  It Sends commands of the form

something=value<CR><LF>

The somethings are, currently:

	- CurrentRSSI=<RSSI>
	- Message=<telemetry>
	- Hex=<hex packet e.g. SSDV>
	- FreqErr=<error in kHz>
	- PacketRSSI=<RSSI>
	- PacketSNR=<SNR>

The serial interface accepts a few commands, each of the form:

~commandvalue<CR>

EG ~F433.3

(a trailing <LF> can be sent but is ignored).  Accepted commands are responded to with an OK (* <CR> <LF>) and rejected commands (unknown command, or invalid command value) with "Unknown Command <CR> <LF>"

The current commands are:

	- F<frequency in MHz>
	- M<mode>
	- B<bandwidth>
	- E<error coding from 5 to 8>
	- S<spreading factor from 6 to 11>
	- I<1=implicit mode, 0=explicit mode>
	- L(low data rate optimisation: 1=enable, 0=disable)
	- C (Displays a PMU Power Managment Unit Report on the Serial Output)

The supported modes are:

0 = (normal for telemetry)  Explicit mode, Error coding 4:8, Bandwidth 20.8kHz, SF 11, Low data rate optimize on

1 = (normal for SSDV)       Implicit mode, Error coding 4:5, Bandwidth 20.8kHz,  SF 6, Low data rate optimize off

2 = (normal for repeater)   Explicit mode, Error coding 4:8, Bandwidth 62.5kHz,  SF 8, Low data rate optimize off	

Bandwidth value strings can be 7K8, 10K4, 15K6, 20K8, 31K25, 41K7, 62K5, 125K, 250K, 500K

History
=======

13/09/2025	V1.1
- Updated to Support T-Beam v1.2
	AXP PMC Hardware updated from AXP192 to AXP2101 in v1.2 of the T-BEAM.
	AXP202X replaced with XPowersLib to support the AXP2101
					
13/09/2025	V1.0
- First version Forked from https://github.com/daveake/LoRaBLE at V1.1
- Added device TBEAM_OLED
	Enables OLED screen fitted to T-BEAM
- Updated to compile after arduino-esp32 BLE upgraded
	https://github.com/espressif/arduino-esp32/pull/8724
