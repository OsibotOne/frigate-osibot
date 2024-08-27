# Installing Frigate on Raspberry Pi 5 with Ubuntu Server 24

This guide provides step-by-step instructions to install Frigate on a Raspberry Pi 5 running Ubuntu Server 24, in a headless configuration. The setup process includes using the Windows Raspberry Pi Imager to prepare the Raspberry Pi and Docker Compose to run Frigate.

## Table of Contents

1. [Setting Up the Raspberry Pi 5 in Headless Mode](#setting-up-the-raspberry-pi-5-in-headless-mode)
2. [Installing Docker and Docker Compose](#installing-docker-and-docker-compose)
3. [Installing Frigate with Docker Compose](#installing-frigate-with-docker-compose)
4. [Configuring Frigate](#configuring-frigate)
5. [Starting Frigate](#starting-frigate)
6. [Accessing Frigate's Web Interface](#accessing-frigates-web-interface)

## Setting Up the Raspberry Pi 5 in Headless Mode

### 1.1. Download and Install Raspberry Pi Imager

1. Download the Raspberry Pi Imager for Windows from the official Raspberry Pi website: [Raspberry Pi Imager](https://www.raspberrypi.org/software/).
2. Install the software on your Windows computer by following the on-screen instructions.

### 1.2. Prepare the MicroSD Card

1. Insert a microSD card (at least 16GB recommended) into your computer using a card reader.
2. Open the Raspberry Pi Imager and select the OS:
   - Choose `Other general-purpose OS` > `Ubuntu` > `Ubuntu Server 24.04 LTS (RPi 5/4/3/2)`.
3. Select the microSD card as the storage device.

### 1.3. Configure for Headless Setup

1. Click on the settings icon (gear icon) in the bottom right corner of the Raspberry Pi Imager.
2. Enable SSH:
   - Check the box for "Enable SSH" and select "Use password authentication".
3. Set the hostname:
   - Check the box for "Set hostname" and enter a name for your Raspberry Pi (e.g., `raspberrypi`).
4. Configure Wi-Fi:
   - Check the box for "Configure wireless LAN".
   - Enter your Wi-Fi SSID and password.
   - Set the "Wireless LAN country" to your country code.
5. Set user credentials:
   - Enter a username and password for your Ubuntu Server.
6. Save and write the image:
   - Click "SAVE" and then "WRITE" to begin writing the image to the microSD card.

### 1.4. Boot Raspberry Pi in Headless Mode

1. Once the writing process is complete, safely eject the microSD card from your computer.
2. Insert the microSD card into your Raspberry Pi 5.
3. Power on the Raspberry Pi by connecting it to a power source.
4. Allow a few minutes for the initial setup. The Raspberry Pi will connect to your Wi-Fi network automatically.

### 1.5. Connect to Raspberry Pi via SSH

1. Open a terminal or command prompt on your computer.
2. Connect to your Raspberry Pi using SSH:
   ```bash
   ssh username@hostname.local

