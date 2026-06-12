# Samsung Galaxy Book 5 Pro (940XHA) Speaker Fix

## Problem

Ubuntu detects the audio device but no sound comes from the internal speakers.

Headphones may work.

Chrome may route audio to HDMI instead of speakers.

## System

- Samsung Galaxy Book 5 Pro
- Model: 940XHA
- Ubuntu 26.04 LTS
- Kernel 7.0.0-22

## Solution

Install the MAX98390 speaker driver.

### Install dependencies

```bash
sudo apt update
sudo apt install curl git dkms build-essential linux-headers-$(uname -r)
```

### Download fix

```bash
curl -sL https://github.com/Andycodeman/samsung-galaxy-book-linux-fixes/archive/refs/heads/main.tar.gz | tar xz
```

### Install

```bash
cd samsung-galaxy-book-linux-fixes-main/speaker-fix
sudo ./install.sh
```

### Secure Boot

If Secure Boot is enabled:

1. Installer asks for password.
2. Reboot.
3. Blue MOK screen appears.
4. Select:

Enroll MOK

Continue

Yes

Enter password

Reboot

### Verify

```bash
lsmod | grep max98390
```

Expected:

```text
snd_hda_scodec_max98390_i2c
snd_hda_scodec_max98390
```

### Check Service

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

Expected:

```text
Found 3 additional amplifier(s)
```

## Chrome Audio Issue

If Chrome still has no sound:

Open:

Settings → Sound

or

```bash
pavucontrol
```

Move Chrome audio from HDMI output to Speaker output.

## Result

Internal speakers working successfully.

## Credits

Andycodeman's MAX98390 Linux driver project.
