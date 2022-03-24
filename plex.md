# Plex

https://www.reddit.com/r/PleX/comments/s0hcf3/plex_crashing_on_qnap_nas_with_qts_50_does_not/

This did not work

Plex crashing on Qnap NAS with QTS 5.0 does not auto restart (How I addressed it)
Help
I created a script called PMS_cron.sh that runs every 5 minutes that checks for the pid (process ID) and if it's not found, restarts the service. There is also a command at the bottom that causes an alert to show in the QTS gui.
```console

#!/bin/sh

#############################################
# Plex Media Server kill and restart script #
#############################################

# Location of QPKG conf file.
CONF=/etc/config/qpkg.conf

# Name of Plex Media Server install directory.
QPKG_NAME="PlexMediaServer"

# Grab Plex Media Server install directory, regardless of disk layout.
QPKG_DIR=$(getcfg -f $CONF $QPKG_NAME Install_path)

# Grab Plex Media Server script
QPKG_SHELL=$(getcfg -f $CONF $QPKG_NAME Shell)

# PID file
PID_FILE="${QPKG_DIR}/Library/Plex Media Server/plexmediaserver.pid"

# PID number
PID_NUMBER=$(cat "$PID_FILE")

# Is PMS enabled in QTS App Center?
ENABLED=$(/sbin/getcfg $QPKG_NAME Enable -u -d FALSE -f $CONF)
if [ "$ENABLED" != "TRUE" ]
  then
    echo -e "$(date)\t$QPKG_NAME is disabled."
    exit 1
  else
    echo -e "$(date)\t$QPKG_NAME is enabled."
#    exit 1
fi

# Test if the process ID exists
if [ -d /proc/$PID_NUMBER ];
    then
#        echo
#        echo $PID_NUMBER

        echo -e "$(date)\tPlex Media Server process ID $PID_NUMBER exists... exiting"
        exit 0
    else
        echo -e "$(date)\tPlex Media Server process ID does not exist, restarting it"
        # Kill all the PIDs

        echo -e "$(date)\tKilling all of the PIDs"
        for pms in $(ps faux | grep -i plex | grep -v grep | awk '{print $2 }')
            do
                echo -e "$(date)\tKilling PID $pms"
                kill -9 $pms
            done
        echo -e "$(date)\tA PIDs are killed!!!"

        echo -e "$(date)\tStopping our awesome Plex Media Server"
        $QPKG_SHELL stop

        echo -e "$(date)\tRestarting our awesome Plex Media Server"
        $QPKG_SHELL restart

        # Send a notification to Qnap GUI
        notice_log_tool -a "Plex Server restarted by cron" -t 7
fi

exit 0

```
I then added this crontab entry using

`sudo vim /etc/config/crontab`
`
that runs the above script every 5 minutes, and logs the output to a file called PMS_cron_output.txt. I may eventually just remove the logging output, but for now is good for at least the initial time I'm monitoring it.

`*/5 * * * * /share/homes/admin/PMS_cron.sh >> /share/homes/admin/PMS_cron_output.txt`

I've read that there is a possibility that the crontab entry may not persist through a reboot, but I've not tested that part...yet.
