# Thinkpad Additions

## QSV (Intel Quick Sync) recording in OBS Studio

- For QSV Intel recording in OBS Studio you have to install Intel-Media-SDK from the AUR

## QEMU / KVM git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

Run the following commands:

```bash
sudo pacman -S qemu-full virt-manager swtpm
echo 'firewall_backend = "iptables"' | sudo tee -a /etc/libvirt/network.conf
sudo usermod -aG libvirt $USER
systemctl enable --now libvirtd.service
systemctl enable --now libvirtd.socket
sudo virsh net-autostart default
sudo ufw route allow from 192.168.122.0/24
```

## VLC H264 Playback

Install the VLC H264 playback plugins:

```bash
sudo pacman -S vlc-plugins-full
```
## Foot Terminal Customizations

Omarchy already has starship installed all it needs is a preset or one built. I am using the Tokyo-Night preset with mine.
starship preset tokyo-night -o ~/.config/starship.toml

## UXPlay

Installed yay -S uxplay
Opened ports: 
# Allow mDNS network discovery
sudo ufw allow 5353/udp

# Allow UxPlay audio and video streams
sudo ufw allow 6000:6001/udp
sudo ufw allow 7011/udp
sudo ufw allow 7000:7001/tcp

#Still didn't connect until I turned off UFW
systemctl dtop ufw


