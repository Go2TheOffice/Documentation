# How to install Wireguard (WIP guide)

This guide was scraped together from various sources of documentation, forum posts and some dude on discord who has more experience than me

## 1. Make sure kernel headers are installed
```
apt update
apt dist-upgrade
apt install pve-headers
```

## 2. Enable backports 
Since wireguard is not on the Debian stable repos, add a stanza to the sources list for [backports](https://wiki.debian.org/Backports). We can probably get away with just adding unstable, but I'd rather use backports. In `sources.list` add:
```
# Backports repository
deb http://deb.debian.org/debian buster-backports main contrib non-free # available after buster release
#deb http://deb.debian.org/debian buster-backports-sloppy main contrib non-free # available after bullseye release
```
and then use `aptitude update` to refresh the apt cache

## 3. Install wireguard
   `apt -t buster-backports install wireguard `

## 4. Enable packet fowarding
    Go into `/etc/sysctl.conf` and make sure the following line is uncommented:
    ```
    net.ipv4.ip_forward=1
    ```
    Then, use `sysctl -p` to reload the file and enable packet fowarding

## 5. Initilize LXC container
This should be staightfoward, standard settings for the container. Remember to enable 

## 6. Create keys in the container
First, create a wireguard directory, and the run the following commands to create a keypair
```
umask 077
wg genkey | tee server-privatekey | wg pubkey > server-publickey
wg genkey | tee mobile-privatekey | wg pubkey > mobile-publickey
```
## 7. Create a new interface configuration
This is an extreme simple case configuration, so we might need to do some fuckery to get this to work. This should be a good place to start though. The convention (according to some documentation) is that the new interface is called `wg0`. Therefore, in `wg0.conf` add:
```
[Interface]
Address = 192.168.2.1/24
PrivateKey = <server-privatekey file contents>
ListenPort = 51820
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
PublicKey = <mobile-publickey file contents>
AllowedIPs = 192.168.2.2/32
```

## 8. Create a configuration for the client:
Call the file client.conf or something. 
```
[Interface]
PrivateKey = <mobile-privatekey file contents>
Address = 192.168.2.2/24
DNS = 192.168.1.1

[Peer]
PublicKey = <server-publickey file contents>
Endpoint = <server-ip>:51820
AllowedIPs = 0.0.0.0/0, ::/0
PersistentKeepalive = 25
```

## 9. Fix permissions (if needed, using chown) and enable the services.
```
chown -R root:root /etc/wireguard
chmod -R og-rwx /etc/wireguard
systemctl enable wg-quick@wg0.service
systemctl start wg-quick@wg0.service
```

## 10. (Optional) Display QR code for mobile iOS client
This looks neat, we should try it:

```
apt install qrencode
qrencode -t ansiutf8 < android.conf
```

That should show a QR code that can then be scanned by the wireguard iOS client.