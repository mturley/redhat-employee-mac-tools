# redhat-mac-wifi

Forces macOS to stop trying to remember your OTP token password for the corporate wifi.

## For Red Hat Employees

This repo doesn't contain anything sensitive, but it won't be useful to you unless you have credentials for the Red Hat wifi network at a Red Hat office building.

## What does it do?

Take a look at the [redhat-wifi.mobileconfig.template](https://github.com/mturley/redhat-mac-wifi/blob/master/config/redhat-wifi.mobileconfig.template) file. That pile of XML was exported by [Apple Configurator 2](https://itunes.apple.com/us/app/apple-configurator-2/id1037126344?mt=12). It's a `mobileconfig` file which defines a Wi-Fi network profile for your Mac, including some settings not available via System Preferences.

Noteworthy properties of this special network profile:

* `OneTimeUserPassword` is turned on. **The OS will not offer to remember your Red Hat wifi password, ever again.** It's one less risky checkbox on your screen.
* `AutoJoin` is turned on. This is normally undesirable for Red Hat wifi because if your Mac remembers your (one-time) password, failed auto-joins will end up locking you out of the network. **Now auto-join is helpful again!** You will get a friendly password prompt when the network is detected.
  * If you don't want this feature, that's easy to fix: before you run `setup`, edit the `redhat-wifi.mobileconfig.template` file and change `AutoJoin`'s value from `true` to `false`.
* `UserName` will be set explicitly to your username, which the `setup` script will prompt you to type. I want to be asked for my password every time, but I don't need to enter my username every time.
  * If you don't want this feature, just press Enter when prompted for your username.
* Because it's a Configurator profile, the changes to your system settings are all self-contained and can be removed in one easy step.

These `mobileconfig` files from Apple Configurator are normally used for deploying restrictive settings across many Mac or iOS devices at once, in environments like schools. Here, I am using one to restrict the settings available on just one Mac, and make it refuse to remember the password for just one Wi-Fi network.

It's simple, but effective... I no longer fuss with my wi-fi every morning.

*Note: This profile only affects wifi networks with the SSID "Red Hat". Unless you name your home wifi "Red Hat", your Mac will still remember the password for it.*

## Setup

Eventually I'll put this into a `brew` tap or something, but for now you can install the profile by cloning this repo and running a script.

**To install your new and improved Red Hat wifi profile:**

```bash
git clone https://github.com/mturley/redhat-mac-wifi.git
redhat-mac-wifi/setup
```

![Setup Script Output](https://raw.githubusercontent.com/mturley/redhat-mac-wifi/master/screenshots/setup.png)

You will be prompted for your sudo password by the terminal, to toggle your wifi power. Then, you'll get a GUI password prompt from "Profiles/MDM" (which stands for 'Multi-Device Manager', that's Apple Configurator's service) to apply the new profile.

![MDM Password Prompt](https://raw.githubusercontent.com/mturley/redhat-mac-wifi/master/screenshots/MDM-password-prompt.png)

When the script is done, you'll be prompted for your wifi password, and the checkbox for "Remember this information" will be gone.

## Removal

Easy peasy.

```bash
redhat-mac-wifi/remove
```

![Removal Script Output](https://raw.githubusercontent.com/mturley/redhat-mac-wifi/master/screenshots/remove.png)

Thanks to Configurator and MDM, it's a clean removal; no trace will be left behind and your settings will be fine.

You'll be asked for both admin passwords again, to toggle the wifi and modify your profiles once more. When the script is done, you can try to reconnect to the Red Hat wifi and you should see your "Remember this information" checkbox appear again.

-----------

Mike Turley

Solving problems nobody needed him to solve since 1989

-----------
