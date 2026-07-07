# LARRYGOTCHI

![Larry the Cat, aka Evil Larry, Destroyer of Worlds](https://cdn3.emoji.gg/emojis/21098-larry-evil.png)

*depicted: Larry the Cat, aka "Scary Larry," aka Evil Larry, Destroyer of Worlds*

## Intro (by me)

**this is my custom/modded setup source code, for quick access**

mostly just preferences i have for configs im guessing, but i might end up adding some more advanced stuff who knows.if you wanna make a release, use pwn-gen from jay or my fork larry-gen (which will eventually have some custom stuff set up hopefully).

plz bear in mind if youre considering in any way using this code that this is very very very much a wip and i have almost no idea what im doing so dont go off what i say.

**credit where its due to jay and company for making such awesome code and to me for ruining it**

## Pwnagotchi

- RPiZeroW (32bit)
- RPiZero2W, RPi3, RPi4, RPi5 (64bit)

**For installation docs check out the [wiki](https://github.com/jayofelony/pwnagotchi/wiki)!**

If you want to sponsor this project you can use GH Sponsor or cryptocurrency:

[GH Sponsor](https://github.com/sponsors/jayofelony)

Or send some ethereum: 0x33ceC4Abe80fDE460a924d596d4dE31Bc0767bb6

**Proudly partnering with [PiSugar](https://www.pisugar.com)!!**

---

[Pwnagotchi](https://pwnagotchi.org/) is a Raspberry Pi leveraging [bettercap](https://www.bettercap.org/) that survives from its surrounding Wi-Fi environment to maximize the crackable WPA key material it captures (either passively, or by performing authentication and association attacks). This material is collected as PCAP files containing any form of handshake supported by [hashcat](https://hashcat.net/hashcat/), including [PMKIDs](https://www.evilsocket.net/2019/02/13/Pwning-WiFi-networks-with-bettercap-and-the-PMKID-client-less-attack/), 
full and half WPA handshakes.

![ui](https://i.imgur.com/X68GXrn.png)

The "old" Pwnagotchi used to have AI to help it learn from its environment, but since then AI seemed to destabilize the Wi-Fi firmware. So I have chosen to remove the AI completely to give the Pwnagotchi more up-time and longer battery life when taking it on a walk.

Multiple units within close physical proximity can "talk" to each other, advertising their presence to each other by broadcasting custom information elements using a parasite protocol I've built on top of the existing dot11 standard.

## Documentation

https://github.com/jayofelony/pwnagotchi/wiki 
https://pwnagotchi.org

## Links

| &nbsp;    | Official Links                                           |
|-----------|----------------------------------------------------------|
| Website   | [pwnagotchi.org](https://pwnagotchi.org/)                  |
| Chat      | [discord](https://discord.gg/PGgnzFbz4M) |
| Subreddit | [r/pwnagotchi](https://www.reddit.com/r/pwnagotchi/)     |

## License

`pwnagotchi` created by [@evilsocket](https://twitter.com/evilsocket) and updated by [us](https://github.com/jayofelony/pwnagotchi/graphs/contributors). It is released under the GPL3 license.

# pwn-gen/larry-gen
**i put this here cuz i couldnt find it very easily elsewhere so it just makes sense imo**

Image builder for [Pwnagotchi](https://pwnagotchi.org/) based on [pi-gen](https://github.com/RPi-Distro/pi-gen).

Produces ready-to-flash `.img.xz` images for both 32-bit (Pi Zero W) and 64-bit (Pi Zero 2W, Pi 3, Pi 4, Pi 5) hardware.

---

## Prerequisites

~20GB free disk space and the following packages:

```bash
sudo apt-get install -y \
  git quilt qemu-user-static debootstrap zerofree \
  libarchive-tools curl pigz arch-test qemu-utils \
  qemu-system-arm qemu-user \
  gcc-aarch64-linux-gnu gcc-arm-linux-gnueabihf
```

---

## Building — Native

```bash
# Clone the repo
git clone https://github.com/jayofelony/pwn-gen
cd pwn-gen

# 64-bit (Pi Zero 2W, Pi 3, Pi 4, Pi 5)
make 64bit

# 32-bit (original Pi Zero W)
make 32bit
```

Finished images are placed in `~/images/`.

---

## Building — Docker

```bash
# Install Docker
sudo apt-get install -y docker.io
sudo usermod -aG docker $USER
# Log out and back in after this

# 64-bit
./pi-gen-64bit/build-docker.sh -c config-64bit

# 32-bit
./pi-gen-32bit/build-docker.sh -c config-32bit
```

---

## Which image do I need?

| Device | Image |
|--------|-------|
| Pi Zero W (original) | 32-bit |
| Pi Zero 2W | 64-bit |
| Pi 3 | 64-bit |
| Pi 4 | 64-bit |
| Pi 5 | 64-bit |

---

## Configuration

Before building, check the config files:

- `config-64bit` — settings for the 64-bit build
- `config-32bit` — settings for the 32-bit build

You can override build paths at the command line if needed:

```bash
make 64bit BUILD_USER=myuser
make 64bit IMAGE_DIR=/mnt/storage/images
```

---

## Flashing

Flash the resulting `.img.xz` from `~/images/` using [Raspberry Pi Imager](https://www.raspberrypi.com/software/) or `dd`:

```bash
# Find your SD card device first - be careful to get the right one!
lsblk

# Flash with dd (replace sdX with your actual device)
xzcat ~/images/your-image.img.xz | sudo dd of=/dev/sdX bs=4M status=progress
sync
```

After flashing, use [PwnConfig](https://pwnstore.org/pwnconfig.html) to generate your `config.toml`, then copy it to `/etc/pwnagotchi/` on the boot partition before first boot.
