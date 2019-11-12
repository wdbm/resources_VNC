# VNC

# introduction

VNC creates a virtual display for applications. As for most displays, it has a size and a color depth. By default, it should start with a display size of 1024 x 768 and a color depth of 24 bits (16M colors). Sometimes it may be sensible to tune these parameters to better fit the memory constraints of the server or the display resolution of the client. For example, to use all of the screen of a 1600 x 1200 display, the option `-geometry 1600x1200` can be passed to `vncserver`.

Once started, `vncserver` runs in the background and listens at two TCP ports ranging from 5800 to 5900 respectively. The actual port numbers depend on the virtual display numeric identification. For example, if `vncserver` reports that it started the desktop on `hostname:5`, then it listens at the ports 5805 and 5905 (respectively 5800 + 5 and 5900 + 5). To be able to connect to the server, there should be SSH tunnels for them.

---

# client setup (Ubuntu)

Install TightVNC, an X viewer client for VNC.

```Bash
sudo apt install xtightvncviewer
```

---

# server setup

The configuration file for the VNC server is `~/.vnc/xstartup`.

## LXPLUS Scientific Linux CERN

The default configuration on Scientific Linux CERN 6 is as follows:

```Bash
#!/bin/sh

# Uncomment the following two lines for a normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &

# Display a dialog box to acquire the AFS token.
(zenity --entry --title "AFS token" \
 --hide-text --text "passcode for $USER@cern.ch" | kinit \
 || zenity --error --title "AFS token" \
 --text "These are invalid credentials. You must run 'kinit' on the
terminal.")&

wm=`which icewm mwm twm  | head -1`
$wm &
```

This causes IceWM to be the desktop environment. In order to have GNOME as the desktop environment, the following configuration could be used:

```Bash
#!/bin/sh

# Uncomment the following two lines for a normal desktop:
unset SESSION_MANAGER
exec /etc/X11/xinit/xinitrc

[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &

# Display a dialog box to acquire the AFS token.
(zenity --entry --title "AFS token" \
 --hide-text --text "passcode for $USER@cern.ch" | kinit \
 || zenity --error --title "AFS token" \
 --text "These are invalid credentials. You must run 'kinit' on the
terminal.")&

#wm=`which icewm mwm twm  | head -1`
#$wm &
```

## Lubuntu

The default configuration on Lubuntu 12.04 is as follows:

```Bash
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
x-window-manager &
```

In order to set LXDE as the desktop environment, the following package can be installaed:

```Bash
sudo apt-get -y install lxde-common
```

Then, the following configuration could be used:

```Bash
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
startlxde &
```

---

# TightVNC client use

## CERN LXPLUS

Start the server on CERN LXPLUS.

```Bash
vncserver
```

The terminal output should be something like the following:

```Bash
New 'lxplus0132:2 (user)' desktop is lxplus0132:2

Starting applications specified in /afs/cern.ch/user/u/user/.vnc/xstartup
Log file is /afs/cern.ch/user/u/user/.vnc/lxplus0132:2.log
```

Connect to the server using the client.

```Bash
vncviewer -via user@lxplus0132.cern.ch :2
```

![](https://raw.githubusercontent.com/wdbm/resources_VNC/master/media/PPELX.png)

The server can be stopped in the following way:

```Bash
vncserver -kill :2
```

## PPELX

Connect to PPELX and switch to some PPELX node. Start the server on PPELX (preferably on some node other than the login nodes).

```Bash
$ vncserver
```

The terminal output should be something like the following:

```Bash
New 'ppelogin1.ppe.gla.ac.uk:1 (user)' desktop is ppelogin1.ppe.gla.ac.uk:1

Starting applications specified in /afs/phas.gla.ac.uk/user/u/user/.vnc/xstartup
Log file is /afs/phas.gla.ac.uk/user/u/user/.vnc/ppelogin1.ppe.gla.ac.uk:1.log
```

Connect to the server using the client.

```Bash
vncviewer -via user@ppelogin1.ppe.gla.ac.uk :1`
```

The server can be stopped in the following way:

```Bash
vncserver -kill :1
```

## Vinagre client use

Set up Vinagre.

```Bash
sudo apt-get -y install vinagre
```

Launch Vinagre.

```Bash
vinagre lxplus0575.cern.ch:1
```

Select "Connect". Setup details are of the following form:

- host: user@lxplus0575.cern.ch:1
- user name: user
