# WM1302 LoRaWAN Gateway Module

WM1302 module is a new generation of LoRaWAN gateway module with mini-PCIe form-factor. Based on the Semtech® SX1302 baseband LoRaWAN® chip, WM1302 unlocks the greater potential capacity of long-range wireless transmission for gateway products. It features higher sensitivity, less power consumption, and lower operating temperature than the previous SX1301 and SX1308 LoRa® chip. 

WM1302 LoRaWAN gateway module has SPI and USB versions on both US915 and EU868 frequency bands, enable you to have a wide-range of LoRaWAN frequency plans options to choose including EU868, US915, AS923, AS920, AU915, KR920 and IN865.

WM1302 module is CE, FCC and Telec certified, which helps simplify the development and certification process of the LoRaWAN gateway devices.

WM1302 is designed for M2M and IoT applications and can be widely applied in LPWAN gateway supported scenarios. It would be a perfect choice for you to significantly reduce the technical difficulties and time-consumption when developing the LoRa gateway devices, including LoRaWAN gateway, miner hotspots, etc.

![](./pinout.jpg)

## Getting Started

### Difference between SPI version and USB version

For WM1302 LoRaWAN Gateway Module SPI version, the Semtech SX1302 and SX126x chip are conntected to Raspberry Pi via the same SPI bus with different chip select(CS) pin.

For WM1302 LoRaWAN Gateway Module USB version, the Semtech SX1302 and SX126x chip are conntected to a STM32L4 MCU, and this factory programmed MCU will work as a USB device, becoming a bridge between Raspberry Pi and SX1302/SX126x.

### Quick Start with WM1302

#### Hardware Required

- WM1302 LoRaWAN Gateway Module

- Raspberry Pi boards with 40-pin GPIO header(e.g. Raspberry Pi 4B or Raspberry 3B+)

- WM1302 Pi Hat for Raspberry Pi

- Power Adapter for Raspberry Pi

- A LoRa antenna

- A 8G or larger SD card and a card reader

- A Type C USB cable if using WM1302 LoRaWAN Gateway Module USB version 

#### Software Required

- [Raspberry Pi imager](https://www.raspberrypi.com/software/) to create a bootable SD card. We recommend Raspberry Pi OS Lite 64.

- [Putty](https://www.putty.org/) to connect to Raspberry Pi via SSH on Windows

#### Step 1. Mounting WM1302 Raspberry Pi Hat and install WM1302 module

It is easy to mount the Pi Hat on Raspberry Pi 40 Pin Header. Power off Raspberry Pi first, insert WM1302 module to the Pi Hat as the following picture and screw it down.

![](./spiversion.jpg)

If USB version WM1302 module is using, please also connect its Type C port to the Raspberry Pi USB port with a Type C USB cable.

![](./usbversion.jpg)

#### Step 2. Install the OS

Using the Raspberry Pi imager, select Raspberry Pi OS (other) and then Raspberry Pi OS Lite (64-bit) and select the SD card you want to use and click 'next'.
When asked for 'OS Customization', click 'edit settings' and make sure to enable SSH in the 'services' tab. In the 'general tab', you can configure your user and WiFi if needed. Save the settings and continue to create the SD card. After the Raspberry Pi has booted from the SD card you can use SSH.
 

#### Step 3. Enable the Raspbian I2C and SPI interface

WM1302 module communicates with Raspberry Pi with SPI and I2C. But these two interfaces are not enabled by default in Raspbian, so developer need to enable them before using WM1302. Here, we introduce a command line way to enable SPI and I2C interface.

First, login in Raspberry Pi via SSH or using a monitor(don't use serial console as the GPS module on the Pi Hat takes over the Pi's hardware UART pins), then type `sudo raspi-config` in command line to open Rasberry Pi Software Configuration Tool:

```
sudo raspi-config
```

![](./raspiconfig.png)

1. Select `Interface Options`

2. Select `SPI`, then select `Yes` to enable it

3. Select `I2C`, then select `Yes` to enable it

4. Select `Serial Port`, then select `No` for "Would you like a login shell..." and select `Yes` for "Would you like the serial port hardware..."

5. After this, please reboot Raspberry Pi to make sure these settings work.

### Step 4. Run LoRa Basics™ Station for Docker

LoRa Basics™ Station software connects your gateway to The Things Stack or other modern LoRa Network Servers. Please follow [these instructions](https://github.com/xoseperez/basicstation-docker) to set it up for your device.
Here is a ```docker-compose.yaml``` example to get you started:
```
services:

  basicstation:
    image: xoseperez/basicstation:latest
    container_name: basicstation
    restart: unless-stopped
    privileged: true
    network_mode: host
    environment:
      MODEL: "WM1302"
      DEVICE: "AUTO"
      USE_LIBGPIOD: 1
      RESET_GPIO: 17
      POWER_EN_GPIO: 18
      TC_KEY: "***"
      CUPS_KEY: "***"
      SERVER: "***"
```
