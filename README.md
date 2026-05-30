# linux-le-hearing-aids
Document how to connect LE hearing aids on Linux

# Guide

Tested with:
- Ubuntu 26.04 LTS
- ReSound Vivia 9

### Process

Edit `/etc/bluetooth/main.conf` and set:
```ini
# Restricts all controllers to the specified transport. Default value
# is "dual", i.e. both BR/EDR and LE enabled (when supported by the HW).
# Possible values: "dual", "bredr", "le"
ControllerMode = le

# Enables D-Bus experimental interfaces
Experimental = true

# Enables kernel experimental features, set to true or enable by UUID
# 6fbaf188-05e0-496a-9885-d6ddfdb4e03e (BlueZ Experimental ISO socket)
KernelExperimental = 6fbaf188-05e0-496a-9885-d6ddfdb4e03e
```

Restart Bluetooth:
```
sudo systemctl restart bluetooth
```

Set bluez sink corresponding to hearing aids as default.

Run `pactl list sinks short`

```
599	alsa_output.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__HDMI3__sink	PipeWire	s32le 2ch 48000Hz	IDLE
600	alsa_output.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__HDMI2__sink	PipeWire	s32le 2ch 48000Hz	IDLE
601	alsa_output.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__HDMI1__sink	PipeWire	s32le 2ch 48000Hz	IDLE
602	alsa_output.pci-0000_00_1f.3-platform-skl_hda_dsp_generic.HiFi__Speaker__sink	PipeWire	s32le 2ch 48000Hz	IDLE
1134	bluez_output.A8_A9_13_34_B5_D3.257	PipeWire	float32le 1ch 48000Hz	IDLE
```

The bluez sink is `bluez_output.A8_A9_13_34_B5_D3.257` so run,
`pactl set-default-sink bluez_output.A8_A9_13_34_B5_D3.257`.

Audio should now play through the hearing aids. You need to disconnect or disable bluetooth to switch back to Android phone.
