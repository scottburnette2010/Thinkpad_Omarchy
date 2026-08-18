# Thinkpad Additions

## QSV (Intel Quick Sync) recording in OBS Studio

- For QSV Intel recording in OBS Studio you have to install Intel-Media-SDK from the AUR

## QEMU / KVM

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