# Samsung Galaxy Book 5 Pro (940XHA) Speaker Fix

⚠️ **This repository does NOT contain the driver.**

This repository documents the installation and troubleshooting process that successfully restored speaker functionality on my Samsung Galaxy Book 5 Pro (940XHA) running Ubuntu Linux.

Credit for the MAX98390 driver and installer goes to the original project:

https://github.com/Andycodeman/samsung-galaxy-book-linux-fixes

---

## Symptoms

You may be affected if:

* Speakers are detected but produce no sound
* `speaker-test` runs without errors
* Headphones work correctly
* HDMI audio works correctly
* Bluetooth audio works correctly
* Internal laptop speakers remain silent

---

## Tested Hardware

| Component    | Value                     |
| ------------ | ------------------------- |
| Model        | Samsung Galaxy Book 5 Pro |
| Model Number | NP940XHA                  |
| Audio Codec  | Realtek ALC298            |
| Amplifiers   | MAX98390                  |
| OS           | Ubuntu 26.04 LTS          |
| Kernel       | 7.0.0-22-generic          |
| Status       | Working                   |

---

## Solution

### Install Dependencies

```bash
sudo apt update
sudo apt install curl git dkms build-essential linux-headers-$(uname -r)
```

### Download the Fix

```bash
curl -sL https://github.com/Andycodeman/samsung-galaxy-book-linux-fixes/archive/refs/heads/main.tar.gz | tar xz
```

### Install

```bash
cd samsung-galaxy-book-linux-fixes-main/speaker-fix
sudo ./install.sh
```

---

## Secure Boot

If Secure Boot is enabled:

1. Run the installer
2. Enter a MOK password when prompted
3. Reboot
4. On the blue MOK Manager screen select:

* Enroll MOK
* Continue
* Yes
* Enter the password you created
* Reboot

---

## Verification

### Check Driver Modules

```bash
lsmod | grep max98390
```

Expected output:

```text
snd_hda_scodec_max98390
snd_hda_scodec_max98390_i2c
```

### Check Service Status

```bash
systemctl status max98390-hda-i2c-setup.service
```

Expected:

```text
active (exited)
```

### Check Amplifier Detection

```bash
sudo journalctl -u max98390-hda-i2c-setup.service
```

Expected output:

```text
Found 3 additional amplifier(s)
```

---

## Chrome Has No Sound After Fixing Speakers

In some cases Chrome may route audio to an HDMI output instead of the laptop speakers.

Check routing:

```bash
wpctl status
```

If Chrome is connected to HDMI:

1. Open Settings
2. Go to Sound
3. Select Speaker as the output device
4. Restart Chrome

---

## Troubleshooting

### Speakers Still Silent

Verify:

```bash
lsmod | grep max98390
```

Verify:

```bash
systemctl status max98390-hda-i2c-setup.service
```

Verify:

```bash
wpctl status
```

Ensure audio is routed to:

```text
Lunar Lake-M HD Audio Controller Speaker
```

and not:

```text
HDMI / DisplayPort Output
```

---

## Credits

This guide is based on the work of:

* Andycodeman
* The SOF Project contributors
* Linux audio community contributors

Original project:

https://github.com/Andycodeman/samsung-galaxy-book-linux-fixes

The MAX98390 DKMS driver and installation scripts are maintained by the original project.

---

## Repository Topics

Recommended GitHub topics:

* samsung
* galaxy-book
* galaxy-book-5-pro
* ubuntu
* linux
* alc298
* max98390
* audio-fix
* speaker-fix
* pipewire
