# Random RGB 1080p

This is a simple 1920x1080 canvas demo that updates once per second. Most frames are full random RGB pixels, and sometimes it shows a structured image.

## Run

Open `random_rgb_1080p.html` in a browser.

## Controls

- **Image ratio**: percentage chance each frame shows a structured image.
# Flipper Zero WiFi Extension Programs

Collection of WiFi tools for Flipper Zero with WiFi Dev Board extension.

## Programs Included

### 1. WiFi Password Brute Force (`wifi_bruteforce`)
- Attempts common passwords on target networks
- **Speed**: ~1-10 attempts per minute (very slow due to WPA2 rate limiting)
- **Effectiveness**: Low - mainly useful for weak/default passwords
- **Saves**: Found passwords to `wifip1.txt`, `wifip2.txt`, etc.

### 2. WiFi Handshake Capture (`wifi_handshake_capture`)
- Captures 4-way WPA/WPA2 handshake for offline cracking
- **Speed**: Capture once, crack offline at millions of attempts/second
- **Effectiveness**: High - standard method for WiFi cracking
- **Saves**: Handshake files as `handshake1.cap` and `handshake1.hccapx`
- **Usage**: Transfer to PC and use hashcat/John the Ripper

### 3. WiFi PMKID Capture (`wifi_pmkid_capture`)
- Captures PMKID hash for offline cracking (alternative to handshake)
- **Speed**: Faster capture than handshake (10-30 sec vs 30 sec-5 min)
- **Effectiveness**: High - no need to wait for handshake
- **Saves**: PMKID files as `pmkid1.22000` (hashcat format)
- **Usage**: Transfer to PC and use hashcat (mode 22000)

### 4. WiFi WPS PIN Cracker (`wifi_wps_crack`)
- Cracks WPS PIN if enabled on router
- **Speed**: ~1-2 PIN attempts per second = 1-4 hours typical
- **Effectiveness**: Medium-High - works on ~50-60% of routers
- **Requirements**: WPS must be enabled (many modern routers disable it)
- **Saves**: PIN to `wps_pin1.txt` and password to `wifip1.txt` if successful

## Speed Comparison

| Method | Capture Time | Crack Time (PC) | Success Rate |
|--------|--------------|-----------------|--------------|
| **Online Password Brute Force** | N/A | Days/weeks | Very low |
| **WPS PIN Attack** | 1-4 hours | N/A | 50-60% (if WPS enabled) |
| **PMKID Capture + Offline** | 10-30 sec | Minutes-hours | High (60-90%) |
| **Handshake Capture + Offline** | 30 sec-5 min | Minutes-hours | High (60-90%) |

## Which Method Should You Use?

1. **Check if WPS is enabled** → Use WPS cracker (fastest if it works, 1-4 hours)
2. **WPS disabled?** → Try PMKID capture first (fastest capture, 10-30 sec)
3. **PMKID fails?** → Capture handshake (30 sec - 5 min)
4. **Then crack offline** → Use hashcat on PC with wordlists (seconds to hours)
5. **Last resort** → Online brute force (very slow, low success)

## Offline Cracking (Required for Methods 2 & 3)

You **must** crack the captured hashes on a PC with hashcat:

**Fastest Methods (Try in order):**
1. **Dictionary attack**: `hashcat -m 22000 capture.22000 rockyou.txt` (seconds if password in list)
2. **Rule-based**: Add `-r rules/best64.rule` to dictionary attack (minutes)
3. **Mask attack**: If you know format, e.g., `-a 3 ?d?d?d?d?d?d?d?d` for 8 digits (very fast)
4. **Full brute force**: Last resort (days/weeks)

See `CRACKING_METHODS.md` for detailed offline cracking guide.

## Building

These programs require:
- Flipper Zero firmware source
- WiFi Dev Board extension
- ESP32 WiFi library

### Build Instructions

1. Clone Flipper Zero firmware:
```bash
git clone https://github.com/flipperdevices/flipperzero-firmware
cd flipperzero-firmware
```

2. Copy programs to `applications_user/` directory

3. Build:
```bash
./fbt launch_app APPSRC=wifi_bruteforce
```

## Legal Notice

These tools are for **educational purposes and authorized testing only**. Only use on networks you own or have explicit permission to test. Unauthorized access to computer networks is illegal.

## Notes

- **WPS**: Many modern routers disable WPS by default. Check if enabled first.
- **Handshake Capture**: Requires a device to connect/disconnect to trigger handshake (deauth helps)
- **Brute Force**: Very slow due to WPA2 rate limiting. Not practical for strong passwords.
- **Offline Cracking**: Handshake/PMKID capture is the most effective method - crack on PC with GPU acceleration.


