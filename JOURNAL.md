---
title: "Commu-Line"
author: "azhan.builds"
description: "A short description of your project"
created_at: "2026-09-21"
---


# October 15: Research and schematics

the first step for the project was to begin researching what the best components for our projects were, and before that we needed to understand the vision of the project and what features it must have. After understanding that and coming up with a plan, we finalised the hardware and moved on towards the schematics.

The choice of software for the schematics was KiCad 10.0.

Initially, the choice of hardware is:
-38 pin ESP32 dev board
-Ra02 SX1278 Lora module
-Neo 6M GPS
-0.96 inch OLED screen
-18650 Li-ion battery
-TP4056 protected battery charger
-MT3608 buck converter
-LEDs
-Buzzer
-Capacitors
-Switches
-NPN transistor 

For most of the components, we had to make custom symbols. Beginning with ESP32, we made a custom 38-pin Symbol taking reference from the esp32 we had. Below is the attached image:

![screenshot](https://cdn.hackclub.com/01a1983b-2ab7-760a-ab11-353f4a50f6af/Screenshot%202026-11-01%20231909.png)

Next in line was the Ra02 lora module, we made a custom symbol for it as well
![screenshot](https://cdn.hackclub.com/01a1983d-fe5d-7b26-8181-6aebd28ed647/Screenshot%202026-11-01%20232151.png)

After adding the two custom made symbols to the schamtics editor, we made their respective connections. Here are the connections:
![screenshot](https://cdn.hackclub.com/01a19856-9b18-7d0b-9d9d-011c4f3362ba/Screenshot%202026-11-01%20234925.png)

One thing to be noted is that we also added two capacitors between 3.3V and GND. The reason is that the Ra-02 pulls a sudden burst of current when it transmits, and without a buffer the voltage can dip and in turn reset the esp32. 

![](https://cdn.hackclub.com/01a1984a-90f9-7b6c-b268-5c5253009ac6/Screenshot%202026-11-01%20233611.png)

Now that we have a brain to process and Lora module to send signals over a long distance, the next things is to make the signals from the lora to be useful. One way to do that is to make sure that we know the location from where the signal is coming from, for which we've used NE0 6M GPS. We added a four pin connector and began making the connections to the esp32
![](https://cdn.hackclub.com/01a19857-b5bb-7a6d-8fbb-9c8d9f8c6369/Screenshot%202026-11-01%20235029.png)

Now the node itself needs to display information(what information it will display will be mentioned later in the firmware devlog), for which we will be using a 0.96 inch OLED display. The connections itself were very basic. The communication protocol used by these oleds are I2C. A few things it will display are Node ID, battery level, and other menu options.
![](https://cdn.hackclub.com/01a19858-719b-73c5-8b9d-699479f9e4ff/Screenshot%202026-11-01%20235119.png)

Let's assume that the node reaches someone who is in desperate need of help, now inorder to alert all the neighbouring rescue teams and also the base team, we will have a SOS button, which when pressed for 3 seconds will send a priority message to the base and near by teams along with the GPS coordinates and other necessary info. We connected the buzzer to IO32 of the esp32.

Next we also added a buzzer to alert nearby survivors of the presence of these nodes and can be used for various other purposes(the exact features will be discussed later in the firmware devlog). We are controlling the buzzer through a NPN transistor connecctedto GPIO33 of the esp32. The reason we use the transistor is because the esp32 cannot reliably supply enough current to run the buzzer. The transistor acts as a switch, which is controlled by esp32 GIO33, which then supplies the necessary current to the buzzer.
![](https://cdn.hackclub.com/01a1985e-b67a-7715-bb8f-0ee95295914e/Screenshot%202026-11-01%20235815.png)

Now to identify if the Lora module and GPS are working properly and are transmitting properly, we added two leds. One led blinks when Ra02 sends a signal, the other does the similar for the GPS. They are connected to IO27 and IO13 respectively.

Now we need to measure the battery level using just ESP32. We will be needing two 100k ohm resistors. It is measured with a voltage divider feeding one of the ESP32's ADC pins. The ESP32 can only read voltages upto 3.3v, and a lion cell can go upto 4.2v, so two resistors scale it down. 
We connect two resistors in series between battery positive and GND. The output is taken from the junction between them to  the ADC pins. A few more considerations and info will be discussed later.

The battery we will be using is a single Liion cell, preferably pouch cell. To charge it, we will be using a protected TP4056 charging module, its output pins can supply power to  the load. The current from the TP4056 goes throught the power switch. Turning the switch off cuts power to everything.

Next is the MT3608 which is a boost converter. This is used because some of the components need a reliable and stable 5v source, which the Li-ion cell fails to provide. The output  from the module is adjustable.

and some of the components still need a 3.3v source, so we use AMS1117-3.3 regulator to give 3.3v. We've added a jumper to choose between the regulator  and the esp32's own 3v3 pin, so we can test both.

![](https://cdn.hackclub.com/01a19870-cda0-7b4b-9ef4-11523987cabd/Screenshot%202026-11-02%20001753.png)

![](https://cdn.hackclub.com/01a19871-41db-7ddd-9cf9-af02fdcba9ce/Screenshot%202026-11-02%20001832.png)

After this, i made all the remaining connections and also updated the footprints to match what i desired. Most of the components have default footprints, but a few needed custom footprints. A few custom footprints are available in the project root folder, in the file KINETIQ.pretty

Note: The schematics is not completely finished. In my next session, i might be moving towards PCB designing and i might add, remove or make some changes to the schematics, but i will make sure to mention it.

**Total time spent: 3h 41min**


# October 17: Designing the PCB v1

Having finished the schematics, the next natural step was PCB design. The choice of software again was Kicad. 

I began by assigning footprints to all the components, most of which i got from the default library. However, a few footprints were custom made, like Esp32 dev kit 38 pin module, MT3608 module and more(you will find them in KINETIQ.pretty).

After assigning footprints, i moved towards the pcb and imported all the components to begin adjusting and deciding their position. I kept thinking for a while, trying to come up with the best position possible for every component. I began experimenting by drawing on piece of papers. I finally came up with something. however, i forgot to resume recording for most of the part. Anyways, in this iteration, the Esp32 is places on bottom left and the lora module is place on top right. I will not realize this until late, that this was a mistake. By placing the ESP32, it became difficult for me to route the pcb. 

Meanwhile, i also added an AMS1117-3.3v regulator so i can get stable 3.3v instead of relying on ESP32. And also, the footprints i used for TP4056 and MT3608 were wrong, which i will correct later.

![](https://cdn.hackclub.com/01a1a2c0-0b37-76a1-9cb6-52a2eee32a24/Screenshot%202026-11-04%20002035.png)

As i  was surfing through the internet, i saw a few things that made me realize that i might need to add a DPad into these pods/nodes, so i proceeded to do so. I added four buttons, 3 of which needed an external pullup resistor in order to connect to esp32 GPIO pins. After arranging the buttons and making a few changes, i started routing the pcb beginning from the power section. I added netclasses and netclass assignments.

Component by component, i started routing and slowly i started to realize the  mistake. The problem was that a few GPIO pins were on the left side of the esp32, and making a connection there was a bit problematic as the esp32 was on the edge. I started using vias, so much so that for each connections i had to add 5-6 vias, which i knew would cause me problems later.

**Total time spent: 4h**

# October 18: Re arranging the PCB
by the time i had only 11 connections left, i decided to not take risk and re arrange the components. Brought the esp32 in the middle, moved the dpad and oled to the left and pushed the gps to the right. Kept the lora module as it is. 
![](https://cdn.hackclub.com/01a1a2c9-c889-7805-98bc-68f1d1eda756/Screenshot%202026-11-04%20003017.png)

I began routing again, component by component. It was really fun tbh, i had learned a lot by now. I used chatgpt mostly to make sure that the decisions i made are correct, and to clear some doubts.

I then finally completed the PCB, made a few small tweaks off screen, nothing big. 
The next step is to clearly define the features i would like on these nodes, and then write the firmware. 

![](https://cdn.hackclub.com/01a1a2ce-cecb-7d30-8716-3edd92546f77/Screenshot%202026-11-04%20003650.png)

**Total time spent: 4h 55min**