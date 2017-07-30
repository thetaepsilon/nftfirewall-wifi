# nftfirewall-wifi: wpa_supplicant integration for nftfirewall

This set of scripts and systemd units can be considered an extension to arch linux's
default per-interface wpa\_supplicant template unit.
A directory in /run houses per-interface network selectors,
and when the interface's unit is started a wpa\_supplicant config file is chosen based on the selector value.

Before wpa_supplicant begins executing,
the [nftfirewall][1] selectors are also updated and the firewall reloaded.
The chosen firewall profile is the same as the network name for now.

Essentially, with this setup one still starts the per-interface units in a similar fashion
\(`systemctl start wifi@$interface`\),
but choosing among multiple wpa\_supplicant configuration files at runtime is possible,
as well as a per-wifi-network firewall profile to match.
Per-wifi-network files are particularly useful if one doesn't want wpa\_supplicant to look for every possible network all the time.

A systemd drop-in fragment is provided for the wireless units to stop them when the system goes into suspend.
It's not a perfect detection of the user's intent to switch networks,
but I personally always suspend my laptop when moving it,
so I just restart the wifi template unit after adjusting the profile selector when I resume my system.

[1]: http://github.com/thetaepsilon/nftfirewall
