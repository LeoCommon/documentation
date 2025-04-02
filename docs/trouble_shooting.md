# Trouble Shooting

### Locally connect to the Pi via UART
- Serial-Port: /dev/ttyUSB0 
- Speed: 115200
- login: root, no pw required


### get logs from a service:
- `journalctl -u client.service -b --no-pager` (since the last boot)
- `journalctl -u client.service -f`  (live view of the journal)
- `journalctl --list-boots` (shows list of available boots in the journal)

### network:
- `nmcli device show`
- `nmcli connection show`
- `nmcli con up/down name`

### Temperature (in milli°C):
- `cat /sys/class/thermal/thermal_zone0/temp`

### run/manage services
- list all running services: `systemctl status`
- get status: `systemctl status client.service`
- start/stop service: `systemctl start/stop client.service`
- start (as root) in terminal: `client --debug`
- start (as apogee-user) in terminal: `su -m apogee -c "client --debug"`

### test iridium-recording locally:
- (connect via UART)
- `cd /data/leocommon-config/job_files/`
- `mkdir test`
- `cd test`
- `echo $'[osmosdr-source]\nsample_rate=5000000\ncenter_freq=1624000000\nbandwidth=5000000\ngain=14\nif_gain=40\nbb_gain=20' > hackrf.conf`
- `cat hackrf.conf` (verify everything is written correct)
- `iridium-extractor -D 4 hackrf.conf > output1.bits`

Errors:

- "sample rate missing!" means either wrong file-ending or wrong content
- "RuntimeError: Faild to create FFTW wisdom lockfile..." <br />
first mount in read-write `mount -o remount,rw /`, then execute iridium-extractor, then mount in read-only `mount -o remount,ro /`.

