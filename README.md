# Stealth Port Scanner

Stealth Port Scanner is an advanced and stealthy port scanning tool that supports TCP, SYN, and UDP scans while implementing multiple techniques for IPS bypassing and evasion. The scanner supports VPN rotation, randomized scanning, and stealth delay techniques to avoid detection by security systems.

## Features

- **Supports multiple scanning modes:**
  - TCP Connect (`--mode tcp-connect`)
  - SYN Stealth (`--mode syn-stealth`, requires root)
  - UDP Scan via Nmap (`--mode udp`, requires root)
- **IPS Bypass Techniques:**
  - Randomized delays between scans (`--min-delay`, `--max-delay`)
  - Randomized port order (`--randomize`)
  - VPN rotation (`--rotation`, `--location`)
  - Low-thread execution to simulate human-like behavior (`--threads`)
  - Controlled retries (`--retries`) to prevent detection
- **Supports scanning predefined port sets:**
  - `top` (Nmap default ports + additional common ports)
  - `top1000` (Top 1000 ports from Nmap)
  - `top-udp` (Top UDP ports from Nmap)
  - `scada` (Common SCADA system ports)
  - `-` (All ports from 1-65535)
- **Multi-threading for fast scanning (`--threads`)**
- **Export results to a file (`--output`)**

## Installation

Stealth Port Scanner requires Python 3 and the following dependencies:

### Install dependencies using `pip`
```sh
pip install -r requirements.txt
```

### Install required system tools
Stealth Port Scanner relies on `nmap` for certain scanning modes. Install it using:

#### Debian/Ubuntu:
```sh
sudo apt update && sudo apt install nmap
```

#### macOS (Homebrew):
```sh
brew install nmap
```

## Usage

### Basic Scan
```sh
python3 stealth_scanner.py --target 192.168.1.1 -p 80,443,22
```

### Scan all ports (1-65535)
```sh
python3 stealth_scanner.py --target --threads 50 192.168.1.1 -p -
```

### Randomized Stealth Scan with VPN rotation (IPS Bypass Mode)
```sh
sudo python3 stealth_scanner.py --target 192.168.1.1 -p top --mode syn-stealth --ips-bypass-syn --rotation 50 --location us
```

### Scan with UDP and TCP simultaneously
```sh
python3 stealth_scanner.py --target 192.168.1.1 -p 53,443,80 --udp
```

### Example Help Output
```
usage: stealth_scanner.py [-h] --target TARGET [-p PORTS] [--threads THREADS] [--timeout TIMEOUT]
                           [--retries RETRIES] [--output OUTPUT]
                           [--mode {tcp-connect,syn-stealth,udp}] [--udp] [--randomize]
                           [--min-delay MIN_DELAY] [--max-delay MAX_DELAY] [--ips-bypass-tcp]
                           [--ips-bypass-syn] [--ips-bypass-udp] [--top-ports | --top-1000 |
                           --top-scada | --top-udp] [--rotation ROTATION] [--location LOCATION]

Stealth Port Scanner: port scanner with options for TCP, SYN, UDP (via nmap -sU), VPN rotation. NOTE: The
'syn-stealth' and 'udp' (via nmap -sU or --top-udp) modes require root privileges.

options:
  -h, --help            show this help message and exit
  --target TARGET       Target IP or hostname.
  -p, --ports PORTS     Port spec (e.g., '1-1000', '80,443', '-' for all, 'top' for default, 'top1000'
                        for top 1000, 'top-udp' for default UDP ports, or 'scada' for default SCADA
                        ports)
  --threads THREADS     Number of threads (default: 50).
  --timeout TIMEOUT     Timeout in seconds (default: 3).
  --retries RETRIES     Retries per port (default: 1).
  --output OUTPUT       Output directory (default: ./port-scan-results).
  --mode {tcp-connect,syn-stealth,udp}
                        Scan mode: 'tcp-connect', 'syn-stealth', or 'udp' (UDP-only mode ignores --udp
                        flag).
  --udp                 Also scan UDP alongside TCP (ignored if --mode udp is used).
  --randomize           Randomize port order.
  --min-delay MIN_DELAY
                        Minimum delay (default: 0).
  --max-delay MAX_DELAY
                        Maximum delay (default: 0).
  --ips-bypass-tcp      Enable TCP IPS bypass parameters.
  --ips-bypass-syn      Enable SYN IPS bypass (requires root).
  --ips-bypass-udp      Enable IPS bypass for UDP scans (requires root for nmap -sU).
  --top-ports           Scan default top TCP ports.
  --top-1000            Scan top 1000 TCP ports.
  --top-scada           Scan known SCADA ports.
  --top-udp             Scan default top UDP ports (use UDP-only mode).
  --rotation ROTATION   Approx. number of ports per VPN rotation.
  --location LOCATION   NordVPN location (e.g., 'us') for VPN rotation.
```

## Disclaimer
This tool is intended for educational and authorized security research purposes only. Unauthorized scanning of networks without permission is illegal and unethical. Use responsibly.

