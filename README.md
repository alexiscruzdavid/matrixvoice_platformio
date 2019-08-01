# MatrixVoice PlatformIO

The current project show how build and upload any firmware for [MatrixVoice ESP32 board](https://www.matrix.one/products/voice) using [PlatformIO](https://platformio.org/)

**note**: The next documentation is based on [Program Over the Air on ESP32 MATRIX Voice](https://www.hackster.io/matrix-labs/program-over-the-air-on-esp32-matrix-voice-5e76bb) documentation but it using PlatformIO instead Arduino IDE. ***You dont need IDF toolchain or any library***, PlatformIO do it for you.


## Prerequisites

### PlatformIO software

Please install first [PlatformIO](http://platformio.org/) open source ecosystem for IoT development and its command line tools (Windows, MacOs and Linux). Also, you may need to install [git](http://git-scm.com/) in your system (PC).

---

### MatrixVoice software

For get updates without RaspberryPi, first you should have a one RaspberryPi with `MatrixVoice` software. Please run into your RaspberryPi shell or ssh:

##### add debian repository key:

```bash
curl https://apt.matrix.one/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.matrix.one/raspbian $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/matrixlabs.list
```

##### update your repository and packages:
```bash
sudo apt-get update
sudo apt-get upgrade
```
#####  install the MATRIX init package:
```bash
sudo apt install matrixio-creator-init
```
#####  reboot your Raspberry Pi:
```bash
sudo reboot
```
##### SSH back into the pi, execute this command:
```bash
voice_esp32_enable
```
---

### Building initial firmware for OTA

Return to your PC and clone this repository:

```bash
git clone https://github.com/alexiscruzdavid/matrixvoice_platformio.git
cd matrixvoice_platformio
```
Copy `platformio.ini` sample and change your network parameters:
```bash
cp platformio.ini.sample platformio.ini
```
**NOTE:** plase change `platformio.ini` and set your SSID and PASSW like this:

```python
'-DWIFI_SSID="MyWifiSsid"'       
'-DWIFI_PASS="MyWifiPassw"'    
```
### Note OTA is not supported by our boards as of yet; to upload new programs you will have to reset the esp32 and flash the firmware.bin through the pi

##### Building initial firmware
```bash
pio run
```
#### upload initial firmware

Enter to OTA directory and upload the firmware. Please replace the IP with your `RaspberryPi ip`:

```bash
cd ota
./install.sh 192.168.178.65
```

The console output should be like this:
```bash
(master) avp:ota$ ./install.sh 192.168.178.65

Loading firmware: ../.pio/build/esp32dev/firmware.bin

-----------------------------------
esptool.py wrapper for MATRIX Voice
-----------------------------------
esptool.py v2.7
Serial port /dev/ttyS0
Connecting....
Chip is ESP32D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, Coding Scheme None
Crystal is 40MHz
MAC: 30:ae:a4:07:6f:7c
Uploading stub...
Running stub...
Stub running...
Configuring flash size...
Auto-detected Flash size: 4MB
Wrote 32768 bytes at 0x00001000 in 3.0 seconds (86.2 kbit/s)...
Hash of data verified.
Wrote 966656 bytes at 0x00010000 in 90.4 seconds (85.6 kbit/s)...
Hash of data verified.
Wrote 16384 bytes at 0x00008000 in 1.5 seconds (86.5 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
done

[SUCCESS] Please disconnect your MatrixVoice from the RaspberryPi and reconnect it alone for future OTA updates.
```

---

## Upload via PlatformIO OTA

After that you can using your MatrixVoice with RaspberryPi and you only need for a new OTA firmware update:

```bash
pio run --target upload
```

## Troubleshooting

If `pio run --target upload` not works, please check `MVID` parameter, it should be a short name, or you can passing ESP32 ip in `upload_port` parameter in `platformio.ini` file.







