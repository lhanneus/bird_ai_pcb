# bird_ai_pcb
Xiao ESP32-C6 with ICS-43434 PCB , RTSP streaming for BirdNet recognition

![Box](doc_images/1-2/closed-box.jpg)
![Box](doc_images/box_01.jpg)

Material:
- Xiao ESP32-C6 : https://wiki.seeedstudio.com/xiao_esp32c6_getting_started/
- ICS-43434 break out board : https://www.adafruit.com/product/6049 , other models and brands exists.
- 6 pin SMT connector
- the pcb board, here with FR1 material for fabrication
- a **metal** case
- external antenna and pigtail connector for wifi

## How does it works, where is the AI ?

ICS-43434 microphone → ESP32-C6 I2S capture → audio stream over Wi-Fi/RTSP → BirdNET recognition server (on a rapsberry pi).

This project focus on the 3 first part :
ICS-43434 microphone → ESP32-C6 I2S capture → audio stream over Wi-Fi/RTSP →

### Bill of materials BOM

DIY Hardware cost about 12€ or less .

| Part | Reference | Quantity | Notes |
| :--- | :--- | :---: | :--- |
| MCU Board | Seeed Xiao ESP32-C6 | 1 | Main controller |
| Microphone | ICS-43434 breakout board | 1 | I2S MEMS microphone |
| PCB | Custom PCB | 1 | KiCad/Gerber files provided |
| Connector | 6-pin connector | 1 | |
| Enclosure | Metal Case | 1 | **IMPORTANT**: Required to shield WiFi interference |
| Antenna | External WiFi antenna (U.FL) | 1 | For extended range |

Battery option & Solar pannel option
TBD.


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

Video of the laser engraving : https://github.com/lhanneus/bird_ai_pcb/raw/refs/heads/main/doc_images/laser_engraving.mp4

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

Supprisingly the 18650 battery and the circuit protection & MPPT fit inside the metal enclosure. But then you save to protect every battery for short circuit with the metal case or any other component, here I used shrink tube and 18650 battery holder.

![Battery inside the metal box](doc_images/1-2/battery_inside.jpg)

I used an old defect wood bird house. The top angle is close the ideal angle for my place ~40°, I glue the small 6V solar pannel.

![Solar on a wood bird house](doc_images/1-2/solar_001.jpg)

Solar pannel allow to install the setup in a more ideal position, the solar pannel facing south and you'll still need a good wifi coverage.

![Installed](doc_images/1-2/solar_002.jpg)

In the meantime I order & manufacture the pcb with my design. Here a photo.

![Installed](doc_images/1-2/pcb1-2.jpg)

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

Share on other social media. I'll share with other support.

Live spectrogram & recognition : https://www.linkedin.com/posts/luc-hanneuse-9984569_birdnet-ugcPost-7464689365605093376-QiMQ/

Recording of a Falcon after the disappearing of many of my Duckling : https://www.linkedin.com/posts/luc-hanneuse-9984569_duckling-vs-falcon-activity-7459691452105646082-Vw8M


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

### Easy firmware flash

The easiest way to flash is to use this link https://esp32mic.msmeteo.cz/ with firefox or chrome that allowed usb with a browser.

Click on connect and have the board connected to your computer

![Box](doc_images/easy_flash_01.jpg)

You will get whit popup and to flash select flash (it takes under 2 minutes to flash). Keep that window open.

![Box](doc_images/easy_flash_02.jpg)

Once the flash finish, select *Logs & Console*

You will have this screen, and click on *Reset Device* , then console message will appears.

![Box](doc_images/logs_reset.jpg)

After clicking reset you should see those messages

![Box](doc_images/console-1st-start.png)

important part is that your board now create a wifi you have to connect to configure the device and to connect it to your own wifi running birdnet system.

SSID is normally ESP32-RTSP-Mic-AP

On your computer you have to connect to that wifi connection

![Box](doc_images/wifi-ap.png)

Then connect to this adress in your browser : 192.168.4.1

![Box](doc_images/config1.png)

Click on the "Configure Wifi" button

![Box](doc_images/config2.png)

Select your wifi network and enter your password. In my case, my wifi is called Theleme, but you have a different wifi connection name at home. Then click *Save*

![Box](doc_images/config3.png)

On your other window with the log console you should see that the connection suceed. You have also to connect your computer to your home wifi.

![Box](doc_images/config4.png)

In the log are important informations to get and understand:
the ip adress of the microphone and that it suceed to connect to your wifi
also the RSTP adress that we will have to enter in the birdnet sound channel.

The console in my case show 192.168.1.36 as IP adress , yours is probably different.

![Box](doc_images/config5.png)

And my RTSP adress is rtsp://192.168.1.36:8554/audio1 , that is needed for Birdnet audio !

![Box](doc_images/config6.png)

Connect your browser to the IP adress to configure the microphone , for me on 192.168.1.36 browser page

![Box](doc_images/browser_config_1.png)

![Box](doc_images/browser_config_2.png)

You can improved the parameters on that page. Usually reducing the Wi-Fi TX Power is good. Also filtering the low frequency is usefull, birds don't emit in those frequency, it's mainly noise.

On your BirndetGo server, go to the *Audio* parameters then click on *Streams*

![Box](doc_images/select_stream-01.png)

Click on add a stream

![Box](doc_images/select_stream-02.png)

Fill in the RTSP adress, mine is RTSP adress is rtsp://192.168.1.36:8554/audio1 , your adress is probably different. Add a name .

![Box](doc_images/select_stream-03.png)

Click on test stream, you should get a 48KHz response

![Box](doc_images/select_stream-04.png)

If everything is correct click on save, and your microphone will start streaming with a nice green "connected" message !!!

![Box](doc_images/select_stream-05.png)

![Box](doc_images/select_stream-06.png)

![Box](doc_images/select_stream-07.png)

You can directly listen to the stream with the new feature "live aduio"

![Box](doc_images/live-01.png)

and by clicking you should see the live sonogram

![Box](doc_images/live-02.png)

There is plenty more to setup but you have now a functionning microphone !!!



## Testing without birdnet

You can test the RSTP audio with VLC or ffmpeg:
VLC: Media → Open Network Stream → paste the RTSP URL.
RTSP url can look like rtsp://192.168.1.36:8554/audio1 , but use your own adress !

ffplay: 
```ffplay -rtsp_transport tcp rtsp://192.168.1.36:8554/audio1```
replace the rtsp://192.168.1.36:8554/audio1 with your board adress

ffplay is used by birdnet.

Since a few release the .local adress name change. And It is allowed now to switch between TCP or UDP ( https://github.com/Sukecz/esp32-birdnet-mic/releases/tag/v1.8.0 ) , even to broadcast 2 RTSP signal at the same time.


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

## BirdNetGO

My quickest and simplest way to have a functional BirdNetGO system is to flash Homeassistant to an SD card with HA OS for a raspberry pi: https://www.home-assistant.io/installation/raspberrypi/#install-home-assistant-operating-system

Then to add an app/repository https://github.com/alexbelgium/hassio-addons

After adding this repository you'll find in the list **birdnet-go** wich use https://github.com/alexbelgium/hassio-addons/tree/master/birdnet-go

After installation you can add the RTSP audio source easyli.


## Future Improvement

~1. Reduce PCB size~

~2. Connect battery Xiao connector and use solar power & 18650 standard battery with associated controlers // Partially documented~

~3. Improved raining protection, done with the reuse bird house~

4. expand to ultrasonic with another cheap mems microphone, to allow bats recognition, introduced with a recent Birndetgo release.

## Licence

Licences CC BY-NC-SA 4.0 : https://creativecommons.org/licenses/by-nc-sa/4.0/

Author : Luc Hanneuse
