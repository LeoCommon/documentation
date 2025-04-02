# Quick-Start-Guide


## Target Audience (Purpose Of This Guide)

This quick start guide is useful in two cases:

- Case 1: You want to start your own network, but want a as simple as possible setup. Our quick-start-guide allows a simple setup of some ground stations that connect to your own server and can execute some basic tasks. For this, start with the 'Server Setup' and follow all steps to the end.<br />
However this simple setup comes with the drawback of no individual updates. For updates you will rely on new releases in this repository. This is caused by the signed updates that are build in the system (for further details see our [security guide](./security_guide.md)). <br />
If you want to install your own updates, you have to build the images on your own and include your own update-certificate, for this see our [full-setup guide](./full_setup/full_setup_guide.md).

- Case 2: You want to contribute with a ground station to a existing network. In this case start with the step 'Ground Station Hardware List' (skip the 'Server Setup'). For getting the login credentials for your ground station to login the existing server, please contact the networks admin. Than use these credentials in the step 'Ground Station Software Setup'.

## Server Setup

You need two servers: one update and one C&C server. We recommend using two distinct URLs (e.g. 'myLeoCommonCnCServer.com' and 'myLeoCommonUpdateServer.com') and two separate servers or virtual boxes for this.

### Update Server Setup

- Clone the [Hawkbit Project](https://github.com/LeoCommon/hawkbit) to get the code and follow the install instructions there. (Hawkbit is an external software, that we just use for updates. We do not claim beeing experts for this software.)

### C&C Server Setup

- Clone the LeoCommon Server Project (TODO: upload) and follow the install instructions in the README of the project. (The following steps are just a summary of the projects README.)
- Install the virtual environment and install the python-requirements.
- Install nginx & certbot.
- Install screen. (optional)
- Configure the http.conf:
	- server_name (e.g. myLeoCommonUpdateServer.com)
	- root
- Start the server by running the `startup.sh`

## Ground Station Hardware List

- Iridium Antenna: [Taoglas IMA.01.105111](https://www.taoglas.com/product/ima-01-iridium-wall-mount-marine-antenna/)
- Mounting hardware for the Iridium Antenna:
	- A 3⁄4 inch (metal) pipe to mount the antenna. (Length of the pipe depends on the location, the antenna should have a good 360° view of the sky.)
	- Some zip-ties or pipe clamps to mount the pipe.
- Antenna extension cable (SMA-female to SMA-male connectors, length of the cable depends on the location, should be as short as possible, best without extension cable)
- [HackRF One](https://greatscottgadgets.com/hackrf/one/) (including a USB-microUSB cable)
- Temperature controlled oscillator (TCXO) (Optional, e.g. [0.1 ppm](https://www.elecbee.com/de-17659-External-TCXO-Clock-CLK-B-Module-PPM-0.1-For-HackRF-One-GPS-Experiment-GSM-WCDMA-LTE-For-Metal-Shell) or [0.5 ppm](https://www.nooelec.com/store/tiny-tcxo.html))
- Raspberry Pi 4 or 5 (with >= 4 GB RAM)
- Power Supply for Pi 4 (5.1 V, 3 A) or Pi 5 (5.1 V, 5 A)
- microSD card (with >= 16 GB)
- USB-Stick (with >= 8 GB)
- weather resistant box (size 25 X 15 X 11 cm, e.g. [DRI-box size Medium](https://dri-box.com/product/))
- GPS antenna for USB
- for connectivity: LAN cable or WiFi or USB-LTE-stick (e.g. HUAWEI 4G Dongle E3372)

## Ground Station Software Setup

### Install OS Image

1. Get the image. Either get the image from the network operator or download it from our repository (TODO)
2. If it is compressed, decompress it. (e.g. `unzstd sdcard.img.zst`)
3. Insert the microSD card to a card-reader.
4. Copy the image onto the microSD card:<br /> (e.g. `sudo dd if=sdcard.img of=/dev/mmcblk0 bs=4M` + `sync`)
5. Unmount the microSD card.

### Get The Credentials

1. For the c&c server: sensor name, refresh-JWT and access-JWT
	- Ask the server admin for the credentials. <br /> **or** 
	- Log in the c&c server with an admin account.
	- Create a new ground station using the sensor name.
	- Create & download new JTWs for the ground station (the server will download both in one zip-file).
2. For the update server: sensor name and security-token
	- Ask the server admin for the credentials. <br /> **or** 
	- Log in the update server with an admin account.
	- Create a new sensor using the sensor name.
	- Check in the details window for the security-token.

### Install the Config USB-Stick

1. Clone the configuration stick repository (TODO:upload) to get the config files.
2. Modify the config files:
	- `configs/config.toml`: Insert sensor name, refresh-JWT, access-JWT and server url.
	- `config.hawkbit`: Add HAWKBIT_CONTROLLER_ID, HAWKBIT_SECURITY_TOKEN and HAWKBIT_SERVER_URL.
	- `eth0.nmconnection`: If necessary add a static IPv4 config. Default is DHCP.
	- `wlan0.nmconnection`: If necessary add a wifi-ssid and wifi-password.
3. Insert the USB-stick
4. Format the USB-stick in 'ext4' with the name 'leocommon-data' (e.g. via gparted)
5. Use the `stick_setup.sh` in the repository to automatically create the config stick <br /> **or** do the configuration manually, by following the next steps
6. Mount the USB-stick and create the following sub-directories on the USB-stick:
	- `/config/apogee`
	- `/config/secrets`
	- `/config/system-connections`
7. Copy the modified config files into their target directory on the usb-stick:
	- copy `config.toml` into the directory `/config/apogee`
	- a: copy the `config.hawkbit` into the directory `/config/secrets`<br />
	  b: inside the directory `/config/secrets` rename the `config.hawkbit` to `.hawkbit`
	- copy `eth0.nmconnection` into the directory `/config/system-connections`
	- copy `wlan0 .nmconnection` into the directory `/config/system-connections`

## Ground Station Hardware Setup

1. Place the Iridium antenna.
	- Find a place to mount the Iridium antenna: more free sky means a wider reception angle of the antenna.
	- Mount the Iridium antenna there, using the mounting equipment. The cable of the antenna should exit the antenna on the downside (no rain should go into the antenna).
	- If required attach an SMA extension cable to the antennas SMA cable. If so, wrap the connectors in tape to keep them dry or use a professional IP68 rated outdoor cable connector.
2. (optional) Open the case of the HackRF One and connect the TCXO
3. Connect the HackRF One to the SMA cable.
4. Connect the HackRF One to the Pi (with the USB-microUSB cable).
5. Connect the USB-GPS antenna to the Pi.
6. Connect the LAN cable or USB-LTE stick to the Pi.
7. Connect the USB stick to the Pi.
8. Insert the microSD card in the Pi.
9. Connect the power supply to the Pi.
10. Put the Pi and the HackRF One in the weather resistant box.
	- This cables should come out of the box: SMA antenna cable, GPS antenna, power cable, (LAN cable)
11. Carefully close the weather resistant box (do not break any antenna cables) 
12. Place the box safely, so it does not fall down or is blown away by strong winds (maybe use some zip-ties).
13. Place the GPS antenna. Usually the GPS-antenna can be placed inside, close to a window, still providing a good GPS reception.
14. Plug in the Pis power supply (and the LAN cable). The Pi should now boot and connect automatically to the server.


