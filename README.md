# Control Xiaomi Mi Home devices with the miIO protocol
Use [Homey](https://www.athom.com/) to control Xiaomi Mi Home devices that use the miIO protocol. Currently Xiaomi does not offer an open API for controlling these devices. This app uses an unofficial library called the [miIO Device Library](https://github.com/aholstenson/miio) for communication with these devices, credits go out to the author of this library.

## Warning
For Homey to be able to communicate with these devices a unique device token needs to be obtained. Most of the miIO devices hide their token and some technical skills are needed for retrieving these tokens. If your are not to tech-savvy this app might not be of any use to you. That being said, below is explained on how to obtain your token.

## Supported devices
Below is a list of currently supported devices and devices that might be supported in the future.
* Yeelight Color Wi-Fi (partially supported but untested yet)
* Mi Robot Vacuum Cleaner (supported and tested)
* Air Purifiers 1, 2 and Pro (supported but untested)
* Mi Humidifier (supported but untested)
* NOT SUPPORTED YET: Mi Smart Socket Plug and Power Strips
* NOT SUPPORTED YET: Mi Smart Home Gateway (Aqara) and accessories

## Obtaining tokens
To make communication between Homey and Mi Home devices possible you will need to retrieve the unique device token. There are currently two ways of doing this.

### Method 1 - Nodejs Command Line Tool from the miIO Device library
The author of the miIO Device Library has also created a nodejs command line tool for retrieving device tokens. Please follow the steps in [these instructions](https://github.com/aholstenson/miio/blob/master/docs/management.md). Be aware that some devices like the Mi Robot Vacuum Cleaner and Yeelights hide their token. Retrieving tokens from these devices require a reset of the device first as described [here](https://github.com/aholstenson/miio/blob/master/docs/management.md#getting-the-token-of-a-device).

### Method 2 - Packet Sender Tool
During setup of Mi Home devices the device tokens an be retrieved by sending a ping command to the device. This method uses a tool called Packet Sender which you will need to download. Choose the portable version which does not require installation.
* Download the portable version of [Packet Sender](https://packetsender.com/download).
* Reset the device following the instructions from the device manual, this usually means holding one or two buttons for 10 seconds. This will reset all device settings including the Wi-Fi settings.
* After reset the device will create a it's own Wi-Fi network. This network will have a name related to the device and is used for configuring the device but will also allow us to retrieve the token. Connect to this Wi-Fi network with your computer which has Packet Sender running.
* Open Packet Sender and enter the following details.
    * HEX: 21310020ffffffffffffffffffffffffffffffffffffffffffffffffffffffff
    * IP: 192.168.8.1
    * Port: 54321
    * Protocol dropdown: UDP
* Click send and the device will respond with an answer which contains the unique device token. In the last 16 bytes (32 characters) of the devices response is the device token. Copy and save it somewhere.
* Disconnect your computer from the devices network, you can now use the Mi Home app to setup the device and connect it to your Wi-Fi network.

## Supported Cards
### Yeelight Color Wi-Fi
* Default flow cards for light capabilities class

### Mi Robot Vacuum Cleaner
* [CONDITIONS] Cleaning, Charging, Paused, Returning to dock
* [ACTIONS] Start, Pause, Stop, Return to dock, spotClean, Find, Set Fan Power

### Mi Air Purifiers
* [CONDITIONS] Powered
* [ACTIONS] Power on/off, Set speed

### Mi Humidifiers
* [CONDITIONS] Powered
* [ACTIONS] Power on/off, Set speed

## Changelog
### 2017-05-21 -- v2.0.0
* rebuild from version 1.0.0 of the [Xiaomi Vacuum Cleaner app](https://github.com/jghaanstra/com.robot.xiaomi-mi)
* use the miIO device library

## To Do List
* Finish driver for Yeelight Color (add hue and saturation)
* Add other devices
* Resolve devices under their own handle during initialization so the app can listen to device events
