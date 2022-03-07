## Prevent Sleep mode on Ubuntu

solution to completely disable all power-management and stop the machine from hibernating, suspend and sleep.

`sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target`

So what does this actually do?

Sudo executes it as root because we need root rights to change the settings.

systemctl is the actual command and it says in the man-pages:

systemctl may be used to introspect and control the state of the systemd(1) system and service manager.

So it changes the “systemsettings”.

The mask command does the following according to the man-pages:

Mask one or more unit files, as specified on the command line. This will link these units to /dev/null, making it impossible to start them. This is a stronger version of disable, since it prohibits all kinds of activation of the unit, including manual activation. Use this option with care. This honors the –runtime option to only mask temporarily until the next reboot of the system.

So it disables everything that we define afterwards:

sleep.target suspend.target hibernate.target hybrid-sleep.target
Now the netbook runs through, even if i close the lid. Perfect.

The reason why i chose an old netbook over a raspberry pi or similar is because of the battery. It will work for a few hours even if there is a power cut. So it is kinda like a build in UPS. And i already had the netbook and no other use for it.
