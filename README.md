# Install
mkdir -p ~/bin
nano ~/bin/rw-local-tunnel    # paste the script
chmod +x ~/bin/rw-local-tunnel
sudo ln -sf ~/bin/rw-local-tunnel /usr/local/bin/rw-local-tunnel

# Use
rw-local-tunnel start   # prompts for port, adds scoped allow
rw-local-tunnel stop    # prompts for port, removes it


# Notes

Scope is VPS-only by default (least privilege). Choose “whole tailnet” if you truly want any tailnet peer to reach that port.

Same port can be open on multiple machines; each machine just runs rw-local-tunnel start for its own port.

If your VPS has a different Tailscale hostname than vpn, run:

VPS_NAME=my-vps rw-local-tunnel start
