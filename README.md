# eyesea-dashboard
## calculate qualif time for VG
```shell
RLBSTART=$(date -d '2023-11-30T17:00:00' +%s)
RLBEND=$(date -d '2023-12-09T17:03:48' +%s)
RLBTREF=$(($RLBEND - $RLBSTART))
date -ud "@$RLBTREF" +"$(( $RLBTREF/3600/24 )) days %H hours %M minutes %S seconds"
9 days 00 hours 03 minutes 48 seconds
VGQUALIFTREF=$(echo - |awk -v rlbtref=$RLBTREF '{print(rlbtref * 1.5)}')
date -ud "@$VGQUALIFTREF" +"$(( VGQUALIFTREF/3600/24 )) days %H hours %M minutes %S seconds"
13 days 12 hours 05 minutes 42 seconds
VGQUALIFEND=$(($RLBSTART + $VGQUALIFTREF))
date -ud "@$VGQUALIFEND"
Thu Dec 14 05:05:42 UTC 2023
```
