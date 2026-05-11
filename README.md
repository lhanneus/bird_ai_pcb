# bird_ai_pcb
Xiao ESP32-C6 with ICS-43434 PCB , RTSP streaming for BirdNet recognition

![Box](doc_images/box_01.jpg)

Material:
- Xiao ESP32-C6 : https://wiki.seeedstudio.com/xiao_esp32c6_getting_started/
- ICS-43434 break out board : https://www.adafruit.com/product/6049 , other models and brands exists.
- 6 pin SMT connector
- the pcb board, here with FR1 material for fabrication
- a **metal** case
- external antenna and pigtail connector for wifi

### PCB Schematic

![Schematic](doc_images/1-2/kicad_1-2_schematic.png)


| ICS-43434   | Xiao ESP32-C6 |
| ----------- | ------------- |
| LRCLK / WS  | D1            |
| SD (DOUT)   | D2            |
| BCLK / SCK  | D3            |


### PCB Board

![Schematic](doc_images/kicad_traces.png)

I didn't find the correct Xiao ESP32-C6 footprint I use a similar footprint. This need to be improved to allow battery connection for a future solar-powered version.

For version 1.2, correct Xiao ESP32-C6 footprint, smaller pcb board to be compatible with "cheap" MHP30 heating system.

![Schematic](doc_images/1-2/kicad_1-2_trace.png)


## Kicad files , gerber & PNG

[/kicad_files](kicad_files)

[/geber_files](geber_files)

[/png_engrave_file](png_engrave_file)

I used https://gerber2png.fablabkerala.in/ to create the png file from gerber kicad files.

Cut out board file image
![Cut](png_engrave_file/1-2_outline.png)

Engrave trace file image
![traces_toplayer](png_engrave_file/1-2_traces.png)

## Fabrication process

Milling process of the FR1 board with a Roland SRM CNC machine.

![Milling](doc_images/milling_01.jpg)

![Milling](doc_images/milling_02.jpg)

Video of the FR1 board CNC milled : https://github.com/lhanneus/bird_ai_pcb/raw/refs/heads/main/doc_images/milling.mp4

And then laser engraving of the circuit with an Xtool F1 ultra.

Video of the laser engraving : "https://github.com/lhanneus/bird_ai_pcb/raw/refs/heads/main/doc_images/laser_engraving.mp4

Result

Xtool F1 ultra parameters were 100% power IR laser. Engraved between 20 to 30 passes with Unidirectionnel Option cross & hatch option in the advance settings.

Result of the board engraved.

![Milling](doc_images/pcb_01.jpg)

Soldered and stuff with the Xiao ESP32-C6 & ICS-43434 PCB components

![Box](doc_images/pcb_02.jpg)

I use a 8 pin connector, because I didn't have anymore 6 pin connector.

Necessary method for version 1.2 is solder paste as the battery connector is under the Xiao ESP32

![Schematic](doc_images/1-2/pcb_1-2_0.jpg)

![Schematic](doc_images/1-2/pcb_1-2_1.jpg)

![Schematic](doc_images/1-2/pcb_1-2_2.jpg)

![Schematic](doc_images/1-2/pcb_1-2_3.jpg)

![Schematic](doc_images/1-2/pcb_1-2_4.jpg)

![Schematic](doc_images/1-2/pcb_1-2_5.jpg)

![Schematic](doc_images/1-2/pcb_1-2_6.jpg)

Here with an 18650 lithium battery and the protection circuit

![Schematic](doc_images/1-2/pcb_1-2_7.jpg)

The solar MPPT and solar pannel are ready to be tested.

## Box

Metal box is really important. The wifi emitted by the esp32 is massively disturbing the mems microphone. 

![Metal Box](doc_images/metal_box_001.jpg)

Drill 3 holes in the metal box:
- one for the external antenna connector
- one for the USB c cable for power supply
- one on the backside for microphone sound capture. Microphone opening is on the opposite side of the pcb

![Box](doc_images/metal_box_002.jpg)

![Box](doc_images/box_01.jpg)

![Box laser engraved](doc_images/box_02.jpg)

## Audio Samples result


## Upload the code

Flash with https://github.com/Sukecz/birdnetgo-esp32-rtsp-mic code.
There is a web flasher version (use Chrome) on https://esp32mic.msmeteo.cz/


The following lines ensure it's the external wifi antenna that is used.

```
    pinMode(3, OUTPUT);
    digitalWrite(3, LOW);
    Serial.println("RF switch control enabled (GPIO3 LOW)");
    pinMode(14, OUTPUT);
    digitalWrite(14, HIGH);
    Serial.println("External antenna selected (GPIO14 HIGH)");
```


After uploading the code, you can put parameters by connecting to the wifi captive portal created, page 192.168.4.1
Then fill your ssid and password codes.

It will then be accesible on your wifi network on esp32mic.local

You can improved the parameters on that page. Usually reducing the Wi-Fi TX Power is good.

And broadcast RTSP audio on
rtsp://esp32mic.local:8554/audio

You can test the RSTP audio with VLC or ffmpeg:
VLC: Media → Open Network Stream → paste the RTSP URL.
ffplay: ffplay -rtsp_transport tcp rtsp://esp32mic.local:8554/audio

ffplay is used by birdnet.


## Expected performance
Here a few data extracted from the ICS-43434 datasheet

```
Omni directionnal
50Hz - 15KHz
MEMS microphone is bottom ported
1.6-3.6V max device only
Sensitivity: −26 dB FS ±1dB
SNR High Performance Mode: 65 dBA
SNR Low Power Mode:  64 dBA
Current Draw High Performance Mode: 490 µA
Current Draw Low Power Mode: 230 µA
Microphone Acoustic Overload Point (AOP) 120 dB    SPL
Sample Rate High Performance Mode: 23 ~ 51.6 kHz
Sample Rate Low Power Mode: 6.25 ~ 18.75 kHz
24bit data
```

This is incredible for the price.

## Power consumption:

To be measured more precisely.

Some real condition test, worked for 5 days on a powerbank.

## Temperature in external condition

I test the system in different location in my garden.
The system shutdown in some place due to the temperature protection, protecting above 80°C.
The black color of the metal case is probably nice looking but not efficient for this aspect.

## Future Improvement

~1. Reduce PCB size~

2. Connect battery Xiao connector and use solar power & 18650 standard battery with associated controlers // Partially documented

3. Improved raining protection

## Licence

Licences CC BY-NC-SA 4.0 : https://creativecommons.org/licenses/by-nc-sa/4.0/

Author : Luc Hanneuse
