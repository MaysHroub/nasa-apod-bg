# NASA APOD Wallpaper

![APOD Example - 2004-04-14](https://apod.nasa.gov/apod/image/0404/dr21b_spitzer_big.jpg)
> *NASA's Astronomy Picture of the Day - April 14, 2004 - Massive Star Forming Region DR21 in Infrared*

I wrote a bash script that automatically downloads and sets NASA's Astronomy Picture of the Day (APOD) as your desktop wallpaper. A nice tool for space lovers.

The script runs automatically via `systemd` timer after 1 hour of uploading the image (or video) by NASA; the actual hour depends on your local time. If the script misses the execution time (because the laptop (or PC) is off), `systemd` runs the script on next boot with a random delay up to 300 seconds.

The image is scalled properly with no cropping or stretching. It sets the image as your wallpaper using `dconf` command.

Also, if today's APOD is only a video with no images, it fallsback to a placeholder image. (the script installs the placeholder by itself so you don't need to worry about it)

You can find out more about the API here: [https://github.com/nasa/apod-api](https://github.com/nasa/apod-api)


## Requirements

- **OS**: Linux with GNOME desktop environment
  - I tested it on Ubuntu 24.04, but it should work fine with Fedora and Pop!_OS
  - It's still not compatible with KDE, XFCE, and Cinnamon, but PRs are welcome.
- **Dependencies**: `curl`, `jq`, `dconf`

### Install dependencies:

This is for Ubuntu/Debian. It should be as simple for other distros.

```bash
sudo apt update && sudo apt install curl jq dconf-cli
```


## Installation

1. **Clone the repo**
```bash
git clone https://github.com/MaysHroub/nasa-apod-wallpaper.git
cd nasa-apod-wallpaper
```
or just create a file called `apod-bg` and copy-paste the script code in it. You can put the file anywhere you want. 


2. **Make the script executable:**
```bash
chmod +x apod-bg
```

3. **Get your free NASA API key:**
   - Visit: https://api.nasa.gov/
   - Sign up (it won't take more than 30 sec)
   - Copy your API key

4. **Edit the script and add your API key:**
```bash
vim apod-bg
# Replace the api_key value on line 72
```

5. **Install the systemd timer:**
```bash
./apod-bg --install-timer
```

And yeah that's basically it. Enjoy your new wallpapers.


## Managing the Timer
```bash
# Check timer status
systemctl --user status apod-bg.timer

# View next scheduled run
systemctl --user list-timers

# Run manually right now
systemctl --user start apod-bg.service

# View logs
journalctl --user -u apod-bg.service

# Disable automatic updates
systemctl --user disable apod-bg.timer
systemctl --user stop apod-bg.timer
```


## Some troubleshooting

**Wallpaper didn't update?**
```bash
# Check service logs
journalctl --user -u apod-bg.service -n 20

# Run manually to see errors
./apod-bg
```

**Timer not running?**
```bash
# Check timer status
systemctl --user list-timers | grep apod

# Restart timer
systemctl --user restart apod-bg.timer
```

**Wrong timezone?**
The script auto-detects your timezone. If the update time seems wrong, check:
```bash
timedatectl
```

<br>

If you are still facing a problem, don't hesitate to *google it*.








