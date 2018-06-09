
Debian
====================
This directory contains files used to package pepcashd/pepcash-qt
for Debian-based Linux systems. If you compile pepcashd/pepcash-qt yourself, there are some useful files here.

## pepcash: URI support ##


pepcash-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install pepcash-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your pepcash-qt binary to `/usr/bin`
and the `../../share/pixmaps/pepcash128.png` to `/usr/share/pixmaps`

pepcash-qt.protocol (KDE)

