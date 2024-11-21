# elegrp_dtr10_dtr30_esphome_template
ESPHome device package for ELEGRP DTR10 and DTR30 Dimmer devices.

These units use a [Tuya Wifi Module CB2S](https://docs.libretiny.eu/boards/cb2s/) in tandem with a TuyaMCU.

Includes most features mapped except Coutdown timer to turn light off.

Adds a couple extra features:
- If disconnected from API for 10 minutes straight, reset LED indicator bar brightness to a preset value 
- Added a number input slider to set Dimmer's brightness without actually turning the light ON



# Initial device programming
My units bought in September 2024 came with Wifi module firmware version 2.0.2 and TuyaMCU firmware version 2.0.4. 

I was able to use [tuya cloudcutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter) without a specific device profile. I just needed to select a generic profile for version 2.0.2 and follow the instructions.

You activate AP mode by holding the paddle down button on the dimmer for 3 to 5 seconds.

Be warned that I was notified of a firmware update to version 3.1.17 when I provisionned a unit with the Tuya Android App. That version is not exploitable through cloudcutter.
It is likely the manufacturer will switch to this updated firmware version in future production.

Firmware version 3.1.17 does NOT include a TuyaMCU firmware update. As for version 2.0.2, the firmware version of TuyaMCU is 2.0.4. There is no point to update the official firmware for the moment.

## Serial flashing
If your device is programmed with a version incompatible with tuya cloudcutter, you can still relatively easily partially dissassemble the unit to flash the unit through serial.

It will require work on the PCB to expose tiny vias and most likely solder onto them. Moderate soldering skills are required. Alternatively, you could desolder the CB2S module to program it with a hot air station.

Information on how to program is located on [Elektroda's forums](https://www.elektroda.com/rtvforum/topic3974847.html). Instructions on how to program it via serial are written for DPR10/30 but will work for DTR10/30 as well.



# How to use
Include this package and populate the necessary variables.

Example of a device YAML file:
```
packages:
  remote_package_files:
    url: https://github.com/bennydiamond/elegrp_dtr10_dtr30_esphome_template
    files: [.base.elegrp.dtr10_dtr30_template.yaml]  # optional; if not specified, all files will be included
    ref: main  # optional
    refresh: 1d  # optional
    vars:
      device_name: "my-dtr30-dimmer"
      device_friendly_name: "Dimmer"
      api_key: "supersecretapikey"
      ota_password: "otapsswd"
      hotspot_name: "DTR30-dimmer-AP"
      hotspot_password: !secret fallback_hotspot_password
      log_level: "INFO"
      indicator_brightness_when_offline_percent: '100'
```

## Customization

Refer to ESPHome's documentation on [packages](https://esphome.io/components/packages) to customize your device.

### Example of customization. 
To remove the Web server component from your device, you would just need to add the following at the bottom of your device's yaml file.

```
web_server: !remove
```

To set static IP.
```
wifi:
  manual_ip:
    static_ip: 192.168.1.10
    gateway: 192.168.1.1
    subnet: 255.255.255.0
```
