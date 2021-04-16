# Voron V0 Cura Configuration
Setup instructions for Cura and the Voron V0.

<img alt="Cura Screenshot with Voron Test Cube" src="https://user-images.githubusercontent.com/11207308/114968109-ae590880-9e43-11eb-8c6e-75436ae5b6a9.png" width="600px" />

These instructions were tested on Linux and Cura 4.8, they should also work on MacOS/Windows.

## Copy Configuration Folder
Find the Cura `Configuration Folder`:
 1. Open Cura.
 2. Using the file menu, `Help -> Configuration Folder`
 3. The `Configuration Folder` will open in your file manager

On Linux, the configuration folder is: `~/.config/cura/{cura_version}/`.

Simply copy the contents `cura-config-folder` from this repository into the path above.

## Adding Your Printer
Creates an instance of your printer in Cura.

Note, the Configuration Folder step above must be done before proceeding with the steps below.

Add your printer to Cura:
 1. In the file menu, `Settings -> Printer -> Add Printer...`.
 2. Select, `A non-networked printer -> VoronDesign -> VORON V0`.
 3. Type a unique `Printer name` in the text box on the right.
 4. Click the `Add` button.
 5. Set the `G-code flavor` dropdown to `Marlin`
 6. Paste your Start G-code, see [Startup GCode](#Startup-GCode).
 7. Modify any settings, the defaults should be fine.
 8. Click the `Next` button.
 9. You should see your pinter in the dropdown.

Screenshots:

<img alt="Cura Add Printer Dialog" src="https://user-images.githubusercontent.com/11207308/114968045-8a95c280-9e43-11eb-8af2-f235f4fb8e2e.png" width="500px" /> <br />
<img alt="Cura Machine Settings Dialog" src="https://user-images.githubusercontent.com/11207308/114967996-73ef6b80-9e43-11eb-8d43-f9128d373388.png" width="500px" /> <br />
<img alt="Cura Printer Dropdown" src="https://user-images.githubusercontent.com/11207308/114968080-a0a38300-9e43-11eb-88df-3aa113475efb.png" width="300px" /> <br />

## Printer Definition
Basic information about the printer is defined in:

`{cura_config_folder}/definitions/voron0_120.def.json`

```
{
    "name": "VORON V0",
    "version": 0,
    "inherits": "voron2_base",
    "metadata":
    {
        "visible": true,
        "platform": "V0_120mm_Bed.stl",
        "quality_definition": "voron2_base"
    },
    "overrides":
    {
        "machine_name":     { "default_value": "VORON V0" },
        "machine_width":    { "default_value": 120 },
        "machine_depth":    { "default_value": 120 },
        "machine_height":   { "default_value": 120 }
    }
}
```

## Print Platform 3D Model
The 3D model is definied in:

`{cura_config_folder}/meshes/V0_120mm_Bed.stl`

The STL model is courtesy of [hartk1213](https://github.com/VoronDesign/VoronUsers/blob/master/slicer_configurations/PrusaSlicer/hartk1213/V0/Bed_Shape/Model/V0_120mm_Bed.stl).

## Toolhead Definitions
A set toolheads with various nozzle sizes:
```
{cura_config_folder}/variants/voron0_120_v6_0.25.inst.cfg
{cura_config_folder}/variants/voron0_120_v6_0.30.inst.cfg
{cura_config_folder}/variants/voron0_120_v6_0.40.inst.cfg
{cura_config_folder}/variants/voron0_120_v6_0.50.inst.cfg
{cura_config_folder}/variants/voron0_120_v6_0.60.inst.cfg
{cura_config_folder}/variants/voron0_120_v6_0.80.inst.cfg
```

It is recommended to copy all toolheads, even if your printer only has one nozzle size available.

These files can be easily modified if your extruder has unique sizing/requirements.

Here is an example:
```
[general]
name = V6 0.40mm
version = 4
definition = voron0_120

[metadata]
setting_version = 16
type = variant
hardware_type = nozzle

[values]
machine_nozzle_size = 0.4
```

## Startup GCode
Start/End GCode runs before and after every printer, it is unique to your specific printer.

Here is an example for **Startup**:
```
G28                  ; home printer xyz
G0 Y0 X40            ; go to tongue of print bed
G1 Z0.2 F500.0       ; move bed to nozzle
G92 E0.0             ; reset extruder
G1 E4 F500.0         ; pre-purge prime LENGTH SHOULD MATCH YOUR PRINT_END RETRACT
G1 X80 E10.0 F500.0  ; intro line 1
G1 Y0.3              ; move in a little
G1 X40 E10.0 F500.0  ; second line
G92 E0.0             ; reset extruder
G1 Z2.0              ; move nozzle to prevent scratch
print_start
```
**NOTE:** The pre-purge prime (line 4) should match your printer's `~/printer.cfg`.

Usually the default `print_end` Stop GCode is enough.

These examples are courtesy of:

https://discord.com/channels/460117602945990667/696930677161197640/757703097949749399

## References
These setting are based on the following sources:
 - https://github.com/Ultimaker/Cura/blob/master/resources/definitions/voron2_base.def.json
 - https://github.com/Ultimaker/Cura/tree/master/resources/variants
