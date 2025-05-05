# ESP32-Desktronic-HomePro
ESPHome configuration for controlling the Desktronic HomePro via HomeAssistant using an ESP32 and ESPHome

The ESPHome YAML config can be found under `config.yaml`.

**WARNING: I haven't extensively tested this. You may break your desk, proceed at your own risk.**

## Hardware Setup
I'm using an ESP32-C3 SuperMini and a cheap RJ50 extension.

**WARNING: Do not connect the black and white pins to your ESP, they carry 36V!**

This is the pinout I'm using:
| Function                                                      | Desk Controller | ESP32                        |
|---------------------------------------------------------------|-----------------|------------------------------|
| Ground                                                        | Orange          | Ground                       |
| 5V                                                            | Green           | 5V                           |
| UART                                                          | Red             | GPIO6                        |
| `M` button                                                    | Brown           | GPIO7                        |
| `Down` button / `Preset 1` with purple / `Preset 3` with blue | Gray            | GPIO8                        |
| `Preset 2` / `Preset 3` with gray                             | Blue            | GPIO20                       |
| `Up` button / `Preset 1` with gray                            | Purple          | GPIO21                       |
| Unknown                                                       | Yellow          | Not Connected/Passed through |
| 36V (for USB power)                                           | Black           | Not Connected/Passed through |
| Ground (for USB power)                                        | White           | Not Connected/Passed through |


## Additional Findings

### Controls
| Command   | Pins to bridge (inverted) |
|-----------|---------------------------|
| Move Up   | Purple                    |
| Move Down | Gray                      |
| M         | Brown                     |
| Preset 1  | Purple + Gray             |
| Preset 2  | Blue                      |
| Preset 3  | Blue + Gray               |

### UART Messages

#### Height reports
The desk reports its height using the following structure:
`0x01 0x01 0x02 0x7B`
The first two bytes are always `0x01`, I'm assuming they're represeting the mode the desk is currently in.
The last two bytes represent the height in Millimeters, in this example `27B` = 635mm.

To get a report, tap any of the buttons to wake the controller.

#### The `M` button
After tapping the `M` button, the desk sends the following UART signal:
`\x01\x06\x00\x00`
I implemented some initial handling for this, but I don't need this to work through HA, so I didn't analyze it further.