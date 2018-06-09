Sample init scripts and service configuration for pepcashd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/pepcashd.service:    systemd service unit configuration
    contrib/init/pepcashd.openrc:     OpenRC compatible SysV style init script
    contrib/init/pepcashd.openrcconf: OpenRC conf.d file
    contrib/init/pepcashd.conf:       Upstart service configuration file
    contrib/init/pepcashd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three Linux startup configurations assume the existence of a "pepcashcore" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes pepcashd will be set up for the current user.

2. Configuration
---------------------------------

At a bare minimum, pepcashd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, pepcashd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that pepcashd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If pepcashd is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running pepcashd without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/pepcash.conf`.

3. Paths
---------------------------------

3a) Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/pepcashd`
Configuration file:  `/etc/pepcashcore/pepcash.conf`
Data directory:      `/var/lib/pepcashd`
PID file:            `/var/run/pepcashd/pepcashd.pid` (OpenRC and Upstart) or `/var/lib/pepcashd/pepcashd.pid` (systemd)
Lock file:           `/var/lock/subsys/pepcashd` (CentOS)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the pepcashcore user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
pepcashcore user and group.  Access to pepcash-cli and other pepcashd rpc clients
can then be controlled by group membership.

3b) Mac OS X

Binary:              `/usr/local/bin/pepcashd`
Configuration file:  `~/Library/Application Support/PepCashCore/pepcash.conf`
Data directory:      `~/Library/Application Support/PepCashCore`
Lock file:           `~/Library/Application Support/PepCashCore/.lock`

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start pepcashd` and to enable for system startup run
`systemctl enable pepcashd`

4b) OpenRC

Rename pepcashd.openrc to pepcashd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/pepcashd start` and configure it to run on startup with
`rc-update add pepcashd`

4c) Upstart (for Debian/Ubuntu based distributions)

Drop pepcashd.conf in /etc/init.  Test by running `service pepcashd start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

4d) CentOS

Copy pepcashd.init to /etc/init.d/pepcashd. Test by running `service pepcashd start`.

Using this script, you can adjust the path and flags to the pepcashd program by
setting the PCHD and FLAGS environment variables in the file
/etc/sysconfig/pepcashd. You can also use the DAEMONOPTS environment variable here.

4e) Mac OS X

Copy org.pepcash.pepcashd.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.pepcash.pepcashd.plist`.

This Launch Agent will cause pepcashd to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run pepcashd as the current user.
You will need to modify org.pepcash.pepcashd.plist if you intend to use it as a
Launch Daemon with a dedicated pepcashcore user.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
