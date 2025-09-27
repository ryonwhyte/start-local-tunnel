# 🌐 rw-local-tunnel

**Secure Tailscale Local Port Tunneling with Real-time Monitoring**

A simple, powerful script that opens local ports for remote access through your Tailscale network with automatic firewall management and live traffic monitoring. Perfect for exposing development servers, databases, or any local service to your VPS or other Tailscale devices.

## ✨ Features

- 🔒 **Secure by default** - Only allows access through Tailscale network
- 📊 **Real-time monitoring** - Live traffic stats and connection tracking  
- 🛡️ **Smart firewall management** - Automatic UFW and nftables configuration
- 🚀 **Auto-cleanup** - Tunnel closes when terminal exits (Ctrl+C or close window)
- 🔧 **Tailscale auto-management** - Detects, starts, and authenticates Tailscale
- 💡 **Interactive prompts** - User-friendly setup with sensible defaults
- 🎯 **Flexible scoping** - Allow access from single VPS or entire Tailnet

## 🚀 Quick Start

```bash
# Download and install
curl -O https://raw.githubusercontent.com/your-username/rw-local-tunnel/main/rw-local-tunnel
chmod +x rw-local-tunnel
sudo mv rw-local-tunnel /usr/local/bin/

# Open port 3000 with monitoring (auto-closes when you exit)
rw-local-tunnel start 3000

# Interactive mode
rw-local-tunnel start
```

## 📋 Requirements

- **Linux** with systemd (Ubuntu, Debian, CentOS, etc.)
- **Tailscale** - Script can help install/configure
- **UFW** (Uncomplicated Firewall)
- **nftables** 
- **jq** - JSON processor

### Auto-Install Dependencies
```bash
# Ubuntu/Debian
sudo apt update && sudo apt install -y ufw nftables jq

# CentOS/RHEL
sudo yum install -y ufw nftables jq

# Arch Linux  
sudo pacman -S ufw nftables jq
```

## 🎯 Usage

### Basic Commands

```bash
rw-local-tunnel start [PORT]    # Open port with live monitoring
rw-local-tunnel stop [PORT]     # Close/block a port
rw-local-tunnel shields on|off  # Tailscale shields up control
```

### Examples

```bash
# Expose local web server on port 8080
rw-local-tunnel start 8080

# Expose database (interactive port selection)
rw-local-tunnel start

# Close a specific port from another terminal
rw-local-tunnel stop 3000

# Enable Tailscale shields (block all incoming)
rw-local-tunnel shields on
```

## 🔧 Configuration

### Environment Variables

```bash
export VPS_NAME="my-server"           # Your VPS hostname in Tailscale
export TAILNET_CIDR="100.64.0.0/10"  # Your Tailnet IP range
export DEFAULT_SCOPE="vps"            # Default access: "vps" or "tailnet"
```

### Access Scopes

When starting a tunnel, choose who can access your local port:

1. **VPS only** (Recommended) - Only your specified VPS can connect
2. **Entire Tailnet** - All devices in your Tailscale network can connect

## 📊 Monitoring Features

The script provides real-time monitoring when you start a tunnel:

```
📊 Monitoring traffic on port 8080 (Press Ctrl+C to stop tunnel)
🎯 VPS IP: 100.64.0.2
📈 Traffic will be shown below:
----------------------------------------
🔗 Port 8080 | Listening: ✅ | Connections: 2 | Traffic: +15 packets (+2.3 KB)
```

**Monitoring shows:**
- ✅/❌ Service listening status
- 🔗 Active connection count
- 📈 Real-time packet and byte counters
- 🎯 VPS IP being monitored

## 🛡️ Security Features

### Firewall Management
- **UFW rules** - Allows specific source IPs/ranges
- **nftables** - Blocks direct Tailscale interface access
- **Automatic cleanup** - Removes rules when tunnel stops

### Tailscale Integration  
- **Shields up** control for extra protection
- **VPS-only access** by default (not whole Tailnet)
- **Automatic daemon management**

## 🔍 Tailscale Auto-Management

The script intelligently handles Tailscale:

### Installation Detection
```
❌ Tailscale is not installed on this system

📦 Install options:
   1. Official Tailscale: https://tailscale.com/download
   2. Trayscale (GUI): https://github.com/DeedleFake/trayscale  
   3. Package manager: sudo apt install tailscale
```

### Service Auto-Start
```
⚠️  Tailscale daemon (tailscaled) is not running
🔄 Attempting to start Tailscale daemon...
✅ Tailscale daemon started successfully
```

### Authentication Helper
```
⚠️  Tailscale daemon is running but not logged in
🔐 Please authenticate with Tailscale:
    tailscale up

Would you like to run 'tailscale up' now? [y/N]: 
```

## 🎮 How It Works

1. **Setup Phase**
   - Detects and starts Tailscale if needed
   - Prompts for port and access scope
   - Configures UFW allow rules
   - Removes any blocking nftables rules

2. **Monitoring Phase**  
   - Shows real-time connection stats
   - Updates every 2 seconds
   - Displays traffic counters

3. **Cleanup Phase** (on exit/Ctrl+C)
   - Adds nftables drop rules
   - Removes UFW allow rules  
   - Blocks further access

## 🚨 Troubleshooting

### Common Issues

**"Bad source address" error**
```bash
# Make sure Tailscale is running and VPS is connected
tailscale status
```

**Service not accessible**
```bash
# Check if your service binds to localhost only
ss -lntp | grep :8080

# Bind to 0.0.0.0 instead of 127.0.0.1
# Example: python -m http.server 8080 --bind 0.0.0.0
```

**UFW not working**
```bash
# Enable UFW if inactive
sudo ufw enable
```

### Debug Mode
```bash
# Check Tailscale connectivity
tailscale ping your-vps-name

# Check firewall rules
sudo ufw status numbered
sudo nft list tables
```

## 📄 License

MIT License - feel free to modify and distribute.

## 🤝 Contributing

Contributions welcome! Please feel free to submit issues and pull requests.

### Development Setup
```bash
git clone https://github.com/your-username/rw-local-tunnel.git
cd rw-local-tunnel
chmod +x rw-local-tunnel

# Test locally
./rw-local-tunnel start 3000
```
