# Fedora Sleep Settings


## systemd-logind

This settings layer applies for headless or console sessions.

> [!NOTE]
> Use a drop-in file instead of directly editing `/etc/systemd/logind.conf`. That file will not survive package updates.

```bash
sudo mkdir -p /etc/systemd/logind.conf.d
sudo tee /etc/systemd/logind.conf.d/99-idle-timeout.conf <<'EOF'
[Login]
IdleAction=suspend
IdleActionSec=1h
EOF

# WARN: will log out and restart machine
sudo systemctl restart systemd-logind
```

##### `IdleActionSec` Options
Supports multiple units. Could be: `1h`, `90min`, `3600`, etc.

##### `IdleAction` Options 
* `ignore` (*daemon will not act on idle, decision left to GNOME*)
* `poweroff` 
* `suspend` (*go to sleep*)
* `hibernate`
* `suspend-then-hibernate`

`IdleAction` set to `ignore` means the daemon will never act on idle.

`IdleAction` only fires for a quote **seated** session that `logind` tracks as idle (keyboard/mouse IO or local console).
Sessions reached purely over SSH usually never register as idle; keep in mind when working with a headless box.


Sleep on a headless system is usually a result of:
* a cron job
* `systemctl suspend` in a script
* the host


# Gnome Desktop Settings

These settings only come into play if a desktop session is running.
Fedora Workstation w/ GNOME respects these settings as another layer above `systemctl-logind`. This is still true if launched without an active desktop.
Gnome keeps its own idle timer.

```bash
# NOTE: set timing & type for AC
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-timeout 3600
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-type 'suspend'

# NOTE: set timing & type for battery
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-battery-timeout 3600
gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-battery-type 'suspend'
gsettings set org.gnome.desktop.session idle-delay 3600
```


