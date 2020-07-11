* ~/.xsetroot.sh

```
DATETIME=`date '+%A %d %B %H:%M'`
VOLUME=$(amixer sget Master | tail -1 | sed 's/Mono: Playback [0-9]* \[\(.*\)\] \[.*\] \[\(.*\)\]$/\1 \2/' | sed 's/off/M/' | sed 's/on //' )
BATTERYSTATE=$( acpi -b | awk '{ split($5,a,":"); print substr($3,0,2), $4, "["a[1]":"a[2]"]" }' | tr -d ',' )
xsetroot -solid gray40 -name "${VOLUME} | ${DATETIME} | ${BATTERYSTATE}"
```

* ~/.xprofile

```
while true
do
    sh .xsetroot.sh

    BATT=$( acpi -b | sed 's/.*[charging|unknown], \([0-9]*\)%.*/\1/gi' )
    STATUS=$( acpi -b | sed 's/.*: \([a-zA-Z]*\),.*/\1/gi' )
    if ([ $BATT -le 5 ] && [ $STATUS == 'Discharging' ]); then
        xbacklight -set 0 && sleep 1
        xbacklight -set 100
    fi

    sleep 10s
done &
```

- [status monitor](https://dwm.suckless.org/status_monitor/)
- [slstatus - suckless status](https://tools.suckless.org/slstatus/)
