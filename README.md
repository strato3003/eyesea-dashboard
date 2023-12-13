# eyesea-dashboard
## calculate qualif time for VG
```shell
COEF=$1
RLBSTART=$(date -d '2023-11-30T17:00:00' +%s)
RLBEND=$(date -d '2023-12-09T17:03:48' +%s)
RLBTREF=$(($RLBEND - $RLBSTART))
date -ud "@$RLBTREF" +"$(( $RLBTREF/3600/24 )) days %H hours %M minutes %S seconds"
#9 days 00 hours 03 minutes 48 seconds
VGQUALIFTREF=$(echo - |awk -v rlbtref=$RLBTREF -v coef=$COEF '{printf "%.0f", (rlbtref * coef)}')
date -ud "@$VGQUALIFTREF" +"$(( VGQUALIFTREF/3600/24 )) days %H hours %M minutes %S seconds"
#13 days 12 hours 05 minutes 42 seconds
VGQUALIFEND=$(($RLBSTART + $VGQUALIFTREF))
date -ud "@$VGQUALIFEND"
echo "in seconds : $VGQUALIFEND"
#gain entre 50% et moins de 51%, soit 50.9999 (les neufs supplementaire ne sont plus significatifs (sous la seconde)
#date -ud @"$((1702538120-1702530342))" +"%H:%M:%S"
```

## exemple
  - si 50% du temps signifie "moins de 51%", alors on applique non pas +50% mais +50.9999% et du coup...2h09mn38s de gain :p)
```shell
ubuntu@ingress:~$
ubuntu@ingress:~$ ./calcul-qualif-vg.sh "1.5"
9 days 00 hours 03 minutes 48 seconds
13 days 12 hours 05 minutes 42 seconds
Thu Dec 14 05:05:42 UTC 2023
in seconds : 1702530342
ubuntu@ingress:~$ ./calcul-qualif-vg.sh "1.509999"
9 days 00 hours 03 minutes 48 seconds
13 days 14 hours 15 minutes 20 seconds
Thu Dec 14 07:15:20 UTC 2023
in seconds : 1702538120
ubuntu@ingress:~$ date -ud @"$((1702538120-1702530342))" +"%H:%M:%S"
02:09:38
```
