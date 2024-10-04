
# Raspberry Pi 4 Wi-Fi to Ethernet Bridge

This guide will help you set up your Raspberry Pi 4 to accept a Wi-Fi connection and share it over Ethernet, turning the Raspberry Pi into a Wi-Fi-to-Ethernet bridge or router.

## Steps to Configure Raspberry Pi as a Wi-Fi-to-Ethernet Bridge

### 1. Setup Wi-Fi on the Raspberry Pi
1. Make sure your Raspberry Pi has Wi-Fi enabled and configured to connect to your Wi-Fi network. You can do this using `raspi-config` or by manually editing the `/etc/wpa_supplicant/wpa_supplicant.conf` file.

   Example configuration for `/etc/wpa_supplicant/wpa_supplicant.conf`:

    ```conf
    country=US
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
        ssid="Your_WiFi_SSID"
        psk="Your_WiFi_Password"
    }
    ```

### 2. Enable IP Forwarding
1. Enable IP forwarding to allow the Raspberry Pi to route traffic from Wi-Fi to the Ethernet port:
   
   ```bash
   sudo sysctl -w net.ipv4.ip_forward=1
   ```

2. Make this change permanent by adding the following line to `/etc/sysctl.conf`:

   ```conf
   net.ipv4.ip_forward=1
   ```

### 3. Configure NAT with `iptables`
1. Set up Network Address Translation (NAT) so that devices connected to the Ethernet port can access the internet through the Pi's Wi-Fi:

    ```bash
    sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
    sudo iptables -A FORWARD -i eth0 -o wlan0 -j ACCEPT
    ```

2. Save the `iptables` rules to persist after reboot:

    ```bash
    sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
    ```

3. To automatically load these rules on boot, add the following line to `/etc/rc.local` before `exit 0`:

    ```bash
    iptables-restore < /etc/iptables.ipv4.nat
    ```

### 4. Set up a DHCP Server (Optional)
If you want the Raspberry Pi to provide IP addresses to devices connected over Ethernet, install and configure `dnsmasq` as a DHCP server.

1. Install `dnsmasq`:

   ```bash
   sudo apt update
   sudo apt install dnsmasq
   ```

2. Configure `dnsmasq` by editing `/etc/dnsmasq.conf`. Here’s an example configuration:

    ```conf
    interface=eth0      # Use interface eth0
    dhcp-range=192.168.2.2,192.168.2.100,255.255.255.0,24h
    ```

3. Restart `dnsmasq` to apply changes:

    ```bash
    sudo systemctl restart dnsmasq
    ```

## Conclusion

Your Raspberry Pi 4 should now accept a Wi-Fi connection and share it over its Ethernet port. Devices connected via Ethernet will be able to access the internet through the Raspberry Pi's Wi-Fi connection.
