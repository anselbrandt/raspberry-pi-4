# Install Transmission

```
sudo apt update
sudo apt upgrade
sudo apt install transmission-daemon
sudo systemctl stop transmission-daemon
sudo mkdir -p /media/NASHDD1/torrent-inprogress
sudo mkdir -p /media/NASHDD1/torrent-complete
sudo chown -R pi:pi /media/NASHDD1/torrent-inprogress
sudo chown -R pi:pi /media/NASHDD1/torrent-complete
sudo nano /etc/transmission-daemon/settings.json
```

```
"incomplete-dir": "/media/NASHDD1/torrent-inprogress",
"download-dir": "/media/NASHDD1/torrent-complete",
"incomplete-dir-enabled": true,
"rpc-password": "Your_Password",
"rpc-username": "Your_Username",
"rpc-whitelist": "192.168.*.*",
```

```
sudo nano /etc/init.d/transmission-daemon
```

```
USER=pi
```

```
sudo nano /etc/systemd/system/multi-user.target.wants/transmission-daemon.service
```
user=pi
```
sudo systemctl daemon-reload
sudo chown -R pi:pi /etc/transmission-daemon
sudo mkdir -p /home/pi/.config/transmission-daemon/
sudo ln -s /etc/transmission-daemon/settings.json /home/pi/.config/transmission-daemon/
sudo chown -R pi:pi /home/pi/.config/transmission-daemon/
sudo systemctl start transmission-daemon
```

```
http://<IPADDRESS>:9091
```
