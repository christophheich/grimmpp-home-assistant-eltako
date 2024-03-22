## Why use an external temperature controller
In the following example, an external temperature controller is used instead of a temperature controller such as an Eltako FTR55SB. The external temperature controller must be integrated into Home Assistant; in this example we do not not use a temperature controller from Eltako.

The reason for using an external temperature controller instead of an Eltako temperature controller such as the FTR55SB is that it can only be operated manually via a "rotary knob" and therefore cannot be controlled via an app. In my case, it is not possible to install wired Eltako temperature controllers in this location as I have no power there.

In this example a wireless temperature controller from tado° is used, but the temperature controller can be replaced as required, provided it can be integrated into Home Assistant.

---

## Setup, automation and use
The overall structure is as follows:

The tado° wireless temperature controller sends its current and target temperature to Home Assistant. An automation is created in Home Assistant, which sends the corresponding information to the Home Assistant Eltako integration when the temperature of the tado° thermostat changes. The automation sends a corresponding EEP message with temperature information directly to the FAM14. In addition to this, a sensor is stored in Home Assistant, which listens to the data from the FAM14, this is used to check whether the values have been transferred correctly; this step is optional.

This is possible because each message is repeated by the FAM14, as described in the technical information from Eltako.

> Furthermore, every telegram received from a taught-in temperature sensor in FAM14 (e.g. FTR55H) is repeated as a confirmation telegram.

![external-temperature-example](https://github.com/christophheich/grimmpp-home-assistant-eltako/assets/16765979/2b5fe2e5-8641-4ef4-84a6-ea0da953f0d5)

### Automation
...

### Optional Sensor
Inside our Home Assistant ***configuration.yml*** we might define a sensor for each temperature controller. The sensor is more of a virtual sensor, each temperature message sent to the FAM14 will be repeated as a confirmation message, therefore the data might be used to check if the temperature sent to the FAM14 is correct.

To get the sensor id you will need to check either the response after setting the temperature or checking the id using a software such as DolphinView or Enocean Device Manager.

In my case this is the response id that has been sent by the FAM14 after i sent a temperature change.

![image](https://github.com/christophheich/grimmpp-home-assistant-eltako/assets/16765979/380fdef8-062b-4d4d-8e7f-1ceebbda0219)

Example configuration for setting the sensor:

```yml
eltako:
  gateway:
  - id: 1
    device_type: enocean-usb300
    base_id: 00-00-00-00
    devices:
      sensor:
        - id: FF-B9-FC-98
          eep: A5-10-06
          name: "FAM14 Confirmation sensor"
```
