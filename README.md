# VS1053b Standalone Real-Time General MIDI Synth for Arduino
This minimal sketch for Arduino UNO (and some other supported boards) turns just your Arduino with Music Shield into a fully working standalone USB/Serial General MIDI synth without the need of additional libraries or parts.

USB/Serial/UART MIDI data are filtered and adapted to SDI for speed and reliability.

### MIDI commands natively supported by VS1053b (XDCS)
Command|Name|Parameters｜c = Channel #
-|-|-
`8c nn xx`|Note Off|nn = Note #<br>xx = Don't care
`9c nn vv`|Note On|nn = Note #<br>vv = Velocity (0 = note off)
`Bc 00 vv`|Channel Mode (Bank Select)|0 = Default, 121 = Melodic, 120/127 = Drum
`Bc 06 vv`|RPN Data Entry|vv = Value
`Bc 07 vv`|Channel Volume|vv = Value (100 = Default)
`Bc 0A vv`|Channel Panpot|vv = Value (64 = Default)
`Bc 0B vv`|Expression|vv = Value
`Bx 0C vv`|Global Reverb Decay|x = Don't care<br>vv = Value
`Bc 40 vx`|Sustain|v<4 = Off, v>=4 = On<br>x = Don't care
`Bc 42 vx`|Sostenuto|v<4 = Off, v>=4 = On<br>x = Don't care
`Bc 5B vv`|Channel Reverb Level|vv = value (40 = Default)
`Bc 62 xx`<br>`Bc 63 xx`|Deselect RPN|xx = Don't care
`Bc 63 vv`|RPN LSB|0 = Pitch Bend Range, 2 = Transpose/Coarse Tune, 1/3-127 = None
`Bc 64 vv`|RPN MSB|0 = On, 1-127 = Off
`Bc 78 xx`|All Sound Off|xx = Don't care
`Bc 79 xx`|Reset All Controllers|xx = Don't care
`Bc 7B xx`<br>`Bc 7C xx`<br>`Bc 7D xx`|All Notes Off|xx = Don't care
`Cc vv`|Program Change|vv = General MIDI Instrument # for melodic, else don't care for drum
`Ec ll mm`|Pitch Bend|ll = Value LSB<br>mm = Value MSB (8192 = center)

### Additional MIDI commands and SysEx implemented and wrapped (to XCS)
Command/SysEx|Name|Parameter(s)
-|-|-
`Bc 7E xx`|Mono Channel Mode|xx = Don't care
`Bc 7F xx`|Poly Channel Mode|xx = Don't care
`FF`| System Reset
`F0 7E 7F 09 01 F7`|GM On
`F0 7E 7F 09 02 F7`|GM Off|(Enters minimal 4-poly synth mode)
`F0 7F 7F 04 01 xx vv F7`|Master Volume|vv = Value<br>xx = Don't care
`F0 7F 7F 04 02 +x vv F7`|Master Panning|vv = Value<br>+>=4 = +1 to Value<br>x = N/A
`F0 00 01 11 01 0v F7`|EarSpeaker Setting|0 = Off, 1 = Low, 2 = Mid, 3 = High
`F0 00 01 11 02 bl th F7`|Bass & Treble Setting|b = Bass Enhancement in 1 dB steps<br>l = Bass lower limit frequency in 1000Hz steps<br>t = Treble Control in 1.5 dB steps<br>h = Treble lower limit frequency in 10Hz steps
`F0 00 01 11 03 0v F7`|Reverb Setting|0 = Default, 1 = Off, 2-15 = Overridden Decay Level

### Button Panel Controls for Waveshare Music Shield
Button Name|Symbol|Arduino & Shield Pin|Description｜Activity LED Pin: 8
-|-|-|-
Reverb|⏯️|5|Press to toggle. Dim and bright LED blinks for off and on respectively. On by default.
Reset|⏮️|6|Press once to reset the synth to it's initial GM parameters. Faster than Arduino's built-in Reset button.
Volume+|🔊|3|Hold to increase. The brighter the LED, the higher the volume.
Volume-|🔉|7|Hold to decrease. The dimmer the LED, the lower the volume.

## Hardware Requirements
* Arduino UNO (R3 or compatible)
* VS1053b Module (e.g. Waveshare Music Shield)
### Typical Wiring (Arduino UNO)
VS1053b/Shield Pin|Arduino Pin|Description
-|-|-
MISO|12|SPI Master In, Slave Out
MOSI|11|SPI Master Out, Slave In
SCK|13|SPI Serial Clock
XRST|A0|Reset (active low)
XCS|A3|Command Chip Select (active low)
XDCS|A2|Data Chip Select (active low)

## Software Requirements
* [CH340 USB Driver](https://wch-ic.com/downloads/ch341ser_exe.html)
* [Arduino IDE](https://docs.arduino.cc/software/ide)
* OmniSerial (eventually), [EA Serial MIDI Bridge](https://github.com/ezequielabregu/EA-serialmidi-bridge/releases), [Hairless MIDI<->Serial Bridge](https://github.com/tyan0/hairless-midiserial/releases) (for Windows/Mac users), or [KAWAI GMegaRSNT](https://geocities.ws/devan/Drivers/KAWAI%20Serial%20MIDI%20NT%20Driver%2032bit.zip) (Windows 32-bit only)
* [loopMIDI (for Windows users)](https://tobias-erichsen.de/software/loopmidi.html)
* A MIDI application (player, sequencer, utility, or game)

## How to install the program onto your Arduino
1. Download the .ino sketch, saving it into a new folder
2. Open the downloaded sketch file in Arduino IDE
3. Make sure your Arduino board is connected to your computer, select the correct board type (UNO in that case), and **Upload**!

NOTE: Make sure your Serial MIDI driver or bridge is not active, otherwise the program will fail to upload due to serial confliction. Then you can turn on your Serial MIDI driver/bridge afterwards.<br>In case you have a different Music Shield that doesn't have control buttons nor an LED, you can remove one or all of the 5 #define lines for the button definitions and LED to tell the compiler not to use those features.<br>Baud rates 31250, 38400, 9600, and 57600 can be used instead of the default 115200 for use with the standard MIDI protocol or different serial MIDI software/drivers.

## Links
* [VS1053b](https://vlsi.fi/en/products/vs1053.html) ([datasheet](https://vlsi.fi/fileadmin/datasheets/vs1053.pdf))
* [Waveshare Music Shield](https://www.waveshare.com/wiki/Music_Shield)
* [Adafruit VS1053 Codec](https://www.adafruit.com/product/1381)
* [Adafruit Music Maker MP3 Shield](https://www.adafruit.com/product/1788)
* [Sparkfun MP3 Player Shield](https://www.sparkfun.com/sparkfun-mp3-player-shield.html)
* [HandsOn Tech VS1053b MP3 Arduino Shield](https://handsontec.com/index.php/product/vs1053b-mp3-arduino-shield)
* [Arduino UNO](https://docs.arduino.cc/hardware/uno-rev3)