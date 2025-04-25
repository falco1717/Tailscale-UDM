# Tailscale-UDM
Install Tailscale on UDM Pro


# Install the latest version of Tailscale UDM  
```
curl -sSLq https://github.com/falco1717/Tailscale-UDM/raw/refs/heads/main/install.sh | sh
```
Add Tailscale's GPG key
```
sudo mkdir -p --mode=0755 /usr/share/keyrings
curl -fsSL https://pkgs.tailscale.com/unstable/debian/bullseye.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
```
Add the tailscale repository
```
curl -fsSL https://pkgs.tailscale.com/unstable/debian/bullseye.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list
```

Install/Update Tailscale
```
sudo apt-get update && sudo apt-get install tailscale=1.79.101
```
edit /etc/default/tailscaled
```
cat <<'EOF' | sudo tee /etc/default/tailscaled > /dev/null
# Set the port to listen on for incoming VPN packets.
# Remote nodes will automatically be informed about the new port number,
# but you might want to configure this in order to set external firewall
# settings.
PORT="41641"

# Extra flags you might want to pass to tailscaled.
FLAGS="--state=/data/tailscale/tailscaled.state --socket=/var/run/tailscale/tailscaled.sock"

EOF
```
# restart Tailscale
```
systemctl restart tailscaled
```
# Start Tailscale
```
sudo tailscale up --advertise-routes="<your-local-subnet-cidr>" --snat-subnet-routes=false --accept-routes --reset
```

# i.e.
```
sudo tailscale up --advertise-routes="10.0.0.0/24" --snat-subnet-routes=false --accept-routes --reset
```
