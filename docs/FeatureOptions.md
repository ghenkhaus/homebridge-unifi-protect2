<SPAN ALIGN="CENTER" STYLE="text-align:center">
<DIV ALIGN="CENTER" STYLE="text-align:center">

[![homebridge-unifi-protect2: Native HomeKit support for UniFi Protect](https://raw.githubusercontent.com/hjdhjd/homebridge-unifi-protect2/master/homebridge-protect.svg)](https://github.com/hjdhjd/homebridge-unifi-protect2)

# Homebridge UniFi Protect<SUP STYLE="font-size: smaller; color:#0559C9;">2</SUP>

[![Downloads](https://img.shields.io/npm/dt/homebridge-unifi-protect2?color=%230559C9&style=for-the-badge)](https://www.npmjs.com/package/homebridge-unifi-protect2)
[![Version](https://img.shields.io/npm/v/homebridge-unifi-protect2?color=%230559C9&label=UniFi%20Protect%202&logo=ubiquiti&logoColor=%230559C9&style=for-the-badge)](https://www.npmjs.com/package/homebridge-unifi-protect2)
[![verified-by-homebridge](https://img.shields.io/badge/homebridge-verified-blueviolet?color=%2357277C&style=for-the-badge)](https://github.com/homebridge/homebridge/wiki/Verified-Plugins)

## HomeKit support for the UniFi Protect ecosystem using [Homebridge](https://homebridge.io).
</DIV>
</SPAN>

`homebridge-unifi-protect2` is a [Homebridge](https://homebridge.io) plugin that provides HomeKit support to the [UniFi Protect](https://unifi-network.ui.com/video-security) device ecosystem. [UniFi Protect](https://unifi-network.ui.com/video-security) is [Ubiquiti's](https://www.ui.com) next-generation video security platform, with rich camera, doorbell, and NVR controller hardware options for you to choose from, as well as an app which you can use to view, configure and manage your video camera and doorbells.

### Feature Options

Feature Options allow you to enable or disable certain features in this plugin. These Feature Options provide unique flexibility by also allowing you to set a scope for each option that allows you more granular control in how this plugin makes features and capabilities available in HomeKit.

The priority given to these options works in the following order, from highest to lowest priority where settings that are higher in priority can override lower ones:

* Device options that are enabled or disabled.
* Controller options that are enabled or disabled.
* Global options that are enabled or disabled.

To specify the scope of an option, you append the option with `.MAC`, where `MAC` is the MAC address of either a UniFi Protect controller or a camera. If you don't append a MAC address to an option, it will be applied globally, unless a more specifically scoped option is specified elsewhere. The plugin will log all devices it encounters and knows about, including MAC addresses. You can use that to guide what features you would like to enable ot disable.

The `options` setting is an array of strings used to customize Feature Options in your `config.json`. I would encourage most users, however, to use the [Homebridge webUI](https://github.com/oznu/homebridge-config-ui-x), to configure Feature Options as well as other options in this plugin. It contains additional validation checking of parameters to ensure the configuration is always valid.

These Feature Options are available on ***all*** UniFi Protect devices available through this plugin:

| Option                                        | Description
|-----------------------------------------------|----------------------------------
| <CODE>Enable.<I>MAC</I></CODE>                | Show the camera or controller identified by MAC address `MAC` from HomeKit.
| <CODE>Disable.<I>MAC</I></CODE>               | Hide the camera or controller identified by MAC address `MAC` from HomeKit.
|                                               |
| <CODE>Enable.Stream.<I>Quality</I></CODE>     | Show the stream of quality *Quality* from HomeKit. Valid quality settings are `Low`, `Medium`, `High`.
| <CODE>Disable.Stream.<I>Quality</I></CODE>    | Hide the stream of quality *Quality* from HomeKit. Valid quality settings are `Low`, `Medium`, `High`.
| <CODE>Enable.StreamOnly.<I>Quality</I></CODE> | Only allow the stream of quality *Quality* to be used in HomeKit. Valid quality settings are `Low`, `Medium`, `High`.
|                                               |
| `Enable.MotionSensor`                         | Add a motion sensor accessory to HomeKit to enable motion detection. *(Default)*
| `Disable.MotionSensor`                        | Remove the motion sensor and motion sensor switch accessories to disable motion detection capabilities
|                                               |
| `Enable.MotionSwitch`                         | Add a switch accessory to activate or deactivate motion detection in HomeKit. *(Default)*
| `Disable.MotionSwitch`                        | Remove the switch accessory used to enable or disable motion detection. *Note: this will not disable motion detection, just remove the ability to selectively activate and deactivate it in HomeKit.*

In addition to the Feature Options available to all UniFi Protect devices, these are available for UniFi Protect doorbell devices:

| Option                                        | Description
|-----------------------------------------------|----------------------------------
| `Enable.ContactSensor`                        | Add a contact sensor accessory that is triggered when the doorbell rings. This is useful when trying to create HomeKit automations for doorbell ring events.
| `Disable.ContactSensor`                       | Remove the contact sensor accessory.  *(Default)*
| `Enable.Messages`                             | Enable the doorbell messages feature on UniFi Protect doorbells. *(Default)*
| `Disable.Messages`                            | Disable the doorbell messages feature on UniFi Protect doorbells.
| `Enable.Messages.FromDoorbell`                | Allow messages saved on the UniFi Protect doorbell to appear as switches in HomeKit. *(Default)*
| `Disable.Messages.FromDoorbell`               | Prevent messages saved on the UniFi Protect doorbell from appearing as switches in HomeKit.


Before using these features, you should understand how Feature Options propagate to controllers and the devices attached to them. If you choose to disable a controller from being available to HomeKit, you will also disable all the cameras attached to that controller. If you've disabled a controller, and all it's devices with it, you can selectively enable a single device associated with that controller by explicitly setting an `Enable.` Feature Option. This provides you a lot of richness in how you enable or disable devices for HomeKit use.

### Example

An example `options` setting might look like this:

```js
"platforms": [
  {
    "platform": "UniFi Protect",

    "options": [
      "Disable.Stream.High",
      "Enable.Stream.High.BBBBBBBBBBBB"
    ]

    "controllers": [
      {
        "address": "1.2.3.4",
        "username": "some-unifi-protect-user (or create a new one just for homebridge)",
        "password": "some-password"
      }
    ]
  }
]
```
In this example:

| Device MAC Address    | Description
|-----------------------|------------------
| AAAAAAAAAAAA          | A UniFi Protect controller with 4 cameras attached to it, including a UVC G3 Flex with a MAC address of BBBBBBBBBBBB.
| BBBBBBBBBBBB          | A UVC G3 Flex that is managed by a UniFi Protect controller with a MAC address of AAAAAAAAAAAA.

* The first line `Disable.Stream.High` disables the high quality stream on all UniFi Protect devices that appear in HomeKit.
* The second line, overrides the first and enables the high quality stream on the G3 Flex because specifying device-specific options always overrides global settings.
