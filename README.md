# Installing Frigate on Raspberry Pi 5 with Ubuntu Server 24

This guide provides step-by-step instructions to install Frigate on a Raspberry Pi 5 running Ubuntu Server 24, in a headless configuration. The setup process includes using the Windows Raspberry Pi Imager to prepare the Raspberry Pi and Docker Compose to run Frigate.

## Table of Contents

1. [Setting Up the Raspberry Pi 5 in Headless Mode](#setting-up-the-raspberry-pi-5-in-headless-mode)
2. [Installing Docker and Docker Compose](#installing-docker-and-docker-compose)
3. [Installing Frigate with Docker Compose](#installing-frigate-with-docker-compose)
4. [Installing coral TPU](#installing-coral-tpu)

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


## Step 2: Installing Docker Compose

To run Frigate on your Raspberry Pi 5, you need to install Docker Compose, which helps manage multi-container applications like Frigate.

### Instructions:

```bash
# Update the package list and upgrade all packages
sudo apt-get update && sudo apt-get upgrade -y

# Install prerequisites for Docker Compose
sudo apt-get install -y curl

# Download Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.10.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Apply executable permissions to the Docker Compose binary
sudo chmod +x /usr/local/bin/docker-compose

# Verify the installation of Docker Compose
docker-compose --version
```

## Step 3: Installing Frigate with Docker Compose

To set up Frigate on your Raspberry Pi 5, follow the steps below to create the Docker Compose file and start Frigate.

### Instructions:

\```bash
# Navigate to your home directory
cd ~

# Create a directory for Frigate
mkdir frigate && cd frigate

# Create a Docker Compose file for Frigate
nano docker-compose.yml

# Add the following content to the docker-compose.yml file
version: '3.9'

services:
  frigate:
    container_name: frigate
    restart: unless-stopped
    privileged: true
    shm_size: '64mb'
    image: ghcr.io/blakeblackshear/frigate:stable
    devices:
      - /dev/bus/usb:/dev/bus/usb
      - /dev/video0:/dev/video0
      - /dev/video1:/dev/video1
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./config.yml:/config/config.yml:ro
      - /media/frigate:/media/frigate
      - type: tmpfs
        target: /tmp/cache
        tmpfs:
          size: 1000000000
    ports:
      - "5000:5000"
      - "1935:1935"
    environment:
      FRIGATE_RTSP_PASSWORD: "your_password_here"

# Save and exit nano by pressing CTRL + X, then Y, then ENTER.

# Create the Frigate configuration file
nano config.yml

# Add your Frigate configuration, for example:
detectors:
  coral:
    type: edgetpu
    device: usb

cameras:
  front_yard:
    ffmpeg:
      inputs:
        - path: rtsp://username:password@camera-ip:port/stream
          roles:
            - detect
    detect:
      width: 1280
      height: 720
      fps: 5

# Save and exit nano by pressing CTRL + X, then Y, then ENTER.

# Launch Frigate with Docker Compose
docker-compose up -d

# Verify that Frigate is running
docker-compose ps

# If everything is set up correctly, open a web browser and navigate to http://<your-pi-ip-address>:5000 to access Frigate’s web interface.
\```


## Step 4: Installing Coral TPU

To enhance Frigate’s object detection performance, you can install and use the Coral Edge TPU on your Raspberry Pi 5. Follow the steps below to set up the Coral TPU.

### Instructions:

\```bash
# Ensure your system is up to date
sudo apt-get update && sudo apt-get upgrade -y

# Install the required packages for Coral TPU
sudo apt-get install -y libedgetpu1-std

# Verify that the Coral TPU is connected and recognized by the system
lsusb

# The Coral TPU should appear in the list of connected USB devices. Look for a device labelled "Global Unichip Corp." Once setup it will say "Google Inc"


# Look for logs that indicate the Coral TPU is being used for object detection. Adjust your setup as needed if errors are reported.
\```


