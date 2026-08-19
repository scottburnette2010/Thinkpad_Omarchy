# Thinkpad Additions

## QSV (Intel Quick Sync) recording in OBS Studio

- For QSV Intel recording in OBS Studio you have to install Intel-Media-SDK from the AUR

## QEMU / KVM Install

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
systemctl stop ufw

# Install tlp and configure for 80% max charge
 TLP is a widely used power management tool that simplifies setting these values:
 Install TLP: sudo pacman -S tlpOpen /etc/tlp.conf with a text editor.
 Uncomment and adjust the parameters for your battery:
 START_CHARGE_THRESH_BAT0=75
 STOP_CHARGE_THRESH_BAT0=80
 Enable and start the service: sudo systemctl enable --now tlp

power-profiles-daemon does not support setting battery charge thresholds because it is strictly designed to manage CPU performance profiles (Power Saver, Balanced, Performance). However, you can easily keep power-profiles-daemon running and use a simple systemd service to safely limit your charge to 80% through the Linux kernel. [1, 2, 3, 4, 5] 
## 1. Verify Kernel Support
Your laptop hardware must support charging thresholds via the Linux kernel. Run this command to check for the control file: [1, 2, 6] 

ls /sys/class/power_supply/BAT*

Look for a file named charge_control_end_threshold or charge_stop_threshold. If neither exists, your hardware vendor hasn't exposed this feature to Linux, and it must be changed in your BIOS instead. [1, 2, 6, 7, 8] 
## 2. Create an Automated Rule
If the file exists, you can build a native startup service that runs perfectly alongside power-profiles-daemon. [1] 

* Create a new configuration file:

sudo nano /etc/systemd/system/battery-limit.service

* Paste the following configuration blocks exactly as shown:

[Unit]
Description=Set Battery Charge Limit to 80%
After=multi-user.target

[Service]
Type=oneshot
ExecStart=/bin/bash -c "echo 80 > /sys/class/power_supply/BAT0/charge_control_end_threshold"
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target

(Note: Replace BAT0 with your actual battery name from step 1, and change charge_control_end_threshold to charge_stop_threshold if your machine uses that naming convention). [1, 6, 9] 

## 3. Activate the Limit

* Reload systemd to detect your new service:

sudo systemctl daemon-reload

* Enable and immediately apply the rule:

sudo systemctl enable --now battery-limit.service

[9, 10, 11] 

If you encounter any issues, what is the exact brand and model of your laptop (e.g., ThinkPad, ASUS, Dell)? Certain brands have dedicated, lightweight terminal tools that handle this seamlessly.

[1] [https://forum.endeavouros.com](https://forum.endeavouros.com/t/how-to-manually-set-thresholds-for-battery-charge/39803)
[2] [https://www.reddit.com](https://www.reddit.com/r/debian/comments/1dji16j/new_to_debian_how_to_set_power_charging_limits/)
[3] [https://www.reddit.com](https://www.reddit.com/r/debian/comments/1dji16j/new_to_debian_how_to_set_power_charging_limits/)
[4] [https://github.com](https://github.com/deepin-community/power-profiles-daemon)
[5] [https://discussion.fedoraproject.org](https://discussion.fedoraproject.org/t/battery-threshold-methods-supports-in-silverblue/38172)
[6] [https://askubuntu.com](https://askubuntu.com/questions/34452/how-can-i-limit-battery-charging-to-80-capacity)
[7] [https://community.frame.work](https://community.frame.work/t/bios-battery-charging-limit-ignored/69610)
[8] [https://askubuntu.com](https://askubuntu.com/questions/1312186/battery-thresholds-in-ubuntu-dell)
[9] [https://askubuntu.com](https://askubuntu.com/questions/34452/how-can-i-limit-battery-charging-to-80-capacity)
[10] [https://discussion.fedoraproject.org](https://discussion.fedoraproject.org/t/power-profiles-and-charging-threshold-in-fedora-39/96683)
[11] [https://discussion.fedoraproject.org](https://discussion.fedoraproject.org/t/battery-charge-limit-reset-after-reboot/162506/11)
