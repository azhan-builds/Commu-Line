# Commu Line

This project aims to solve a problem related to disaster situations. During disasters, the network and communication infrastructure often breaks down, leading to loss of proper communication between people especially rescuers. For V1, I am trying to focus only on the communication between rescue teams.

I will be developing a LoRa mesh communication node for disaster zones. I will be developing small pods that will be placed in a defined distance to create a mesh network.

## What it does

A pod is a small standalone node. They will dropped around an affected area in order to create or form a mesh network.

Each pod:
- Relays text messages hop to hop, so range is not limited to one node's radio.
- It can also run a local Wifi hotspot so any phone can connect to it and send an SOS message without needing an app.
- It will also feature a physical SOS button in case phone isn't an option.
- It will log GPS positions and status.
- Stores and forwards messages if the network is broken mid-hop.

For V1, we hope to develop 7 pods.

## Hardware

- ESP32-S3 dev board for computing
- Waveshare Core1262-HF module (SX1262 LoRa radio, IN865 band)
- Neo-6 GPS
- 0.96' Oled display
- TP4056 charger + single-cell Li-ion battery
- SOS button, buzzer and status LEDs
- PVC enclosure (For prototyping, PVC will be used but once the system is up and running, we will shift to 3d printing.)
- Circuit on a vero board (Custom PCB will be developed only after the system is ready)

I will not be using any pre-integrated boards like Heltec or seeed. Radio and compute are seperate and will be wired and programmed by me.

## Status

V1 scope: text mesh only (discovery, multi-hop, store-and-forward, ACK, local wifi UI).

Image transfer will be experimental. Survivor to rescue teams is planned for V2.

So far the hardware is finalised. The next step is schematics.