
## Alternative
Ansible-Role aus [Github](https://github.com/FreifunkBremen/ansible)

# Verbindung Rechner an Freifunk Netzwerk über fastd
## Software
### Einbindung des [freifunk-rheinland](https://wiki.archlinux.org/index.php/Unofficial_user_repositories#freifunk-rheinland)-Repositories

In /etc/pacman.conf hinzufügen:
```
[freifunk-rheinland]
Server = http://mirror.fluxent.de/archlinux-custom/$repo/os/$arch
```

### Installation
```
pacman -S ethtools lsb-release batman-alfred fastd batman-adv-dkms batctl
```

## Konfiguration
### fastd (und batman)
Zunächst muss ein privater Schlüssel mit `fastd --generate-key` generiert werden, welcher sicher aufbewahrt werden muss.

Danach die folgende Konfigurationsdatei (Dateiname-hier:ffhb-service-fastd.conf) anpassen:
Diese muss ggf. aktualisiert werden, hierzu dient [Site.conf für gluon-Nodes](https://github.com/FreifunkBremen/gluon-site-ffhb/blob/master/site.conf).
IP-Adresse kann per `dhclient ffhb` oder wie manuell gesetzt werden (beachte erlaubte IP-Bereiche und die gewählten IP-Adresse ins Wiki [Dienste] eintragen):

Zu editierende Werte:
* PRIVATE-KEY (der vorher generiert wurde)
* FREIE-IP-ADRESSE
* MACADDRESS (Welche sich ausgedacht wurde - vllt. einfach einmal ohne fastd aufrufen)

```
interface "ffhbVPN";
mode tap;
method "salsa2012+gmac";
mtu 1426;
secure handshakes yes;
mac "ghash" use "builtin";
secret "PRIVATE-KEY DER VORHIN GENERIERT WURDE";

## VPNSERVER aus site.conf
peer "vpn01.ffhb" {
  remote "vpn01.bremen.freifunk.net" port 10000;
  key "df444c4366880735d8101ec4e8bcd8288a7e617024c08f0f81027927ba794f9f";
}
peer "vpn02.ffhb" {
  remote "vpn02.bremen.freifunk.net" port 10000;
  key "93a15a14d85a165b86624c89d69c481043cb60542bfa3d2896615b66a57ab02c";
}
peer "vpn03.ffhb" {
  remote "vpn03.bremen.freifunk.net" port 10000;
  key "6c16136b58da06e3cc83bc92ef09bb19b7dfa787e390929a0ac0d4bbebddee2b";
}

on pre-up "
  echo 'Pre-up';
  modprobe batman-adv

";

on up "
### MINISKRIPT damit der TRAFFIC über das FFHB gelegt werden kann.
#  ROUTE=$(/sbin/ip route | awk '/default/ { printf(\"%s\", $3) }');
#  VPNS=$(dig +short vpn01.bremen.freifunk.net vpn02.bremen.freifunk.net vpn03.bremen.freifunk.net);

#  for VPN in $VPNS; do
#    ip route add $VPN via $ROUTE proto static metric 600
#  done

  ip link set up $INTERFACE address MACADDRESS;

";
on establish "
  echo 'establish';
  batctl -m ffhb if add $INTERFACE;

  echo 'Setup FFHB'
  ip link set up ffhb address MACADDRESS;
  ip addr add FREIE-IP-ADRESSE/17 broadcast 10.196.127.255 dev ffhb
  ip route add 10.196.0.0/17 dev ffhb  proto kernel  scope link  src FREIE-IP-ADRESSE metric 50


##
# TRAFFIC ÜBER EIN DER GATEWAYS
##
#  echo 'establish: dangerouse route';
#  ip route add default via 10.196.0.1 proto static metric 52
#  ip route add default via 10.196.0.2 proto static metric 51
#  ip route add default via 10.196.0.3 proto static metric 50

  echo -e 'nameserver 208.67.222.222\\nnameserver 208.67.200.200\\nnameserver 10.196.0.1\\nnameserver 10.196.0.2\\nnameserver 10.196.0.3\\nnameserver fd2f:5119:f2c::1' > /etc/resolv.conf

  echo 'Setup Alfred'
";
on disestablish "
  echo 'disestablish';
  killall alfred batadv-vis
";

on down "
  echo 'down';
  batctl -m ffhb if del $INTERFACE
";


on post-down "
  echo 'post-down';
  modprobe -r batman-adv

";


```

Dannach sich mit Hilfe diese fastd starten.
`fastd -c ffhb-service-fastd.conf`

### Achtung
Dies ist nur ein Prototyp, der mit Vorsicht zu genießen ist.
Später sollten daraus mehrer systemd-units und netcfg werden.

Es werden Fehlermeldungen erscheinen, wenn man sich mit mehreren VPNs Verbindet.
Diese können ignoriert werden oder der 'on establish'-Teil einmalig per Hand oder Skript ausgeführt werden.

## Auftauchen in der Alfred-Statistik
```
alfred -i ffhb -b ffhb
batadv-vis -i ffhb -s
```
**Achtung:** MAC-Adresse von ffhb und ffhbVPN sollten gleich heißen!

### Alfred Annoucen (Namen und Statistik übermitteln)
Aus [Github](https://github.com/ffnord/ffnord-alfred-announce) clonen.
und in Cronjob-Eintragen:
```
*/1    * * * * /root/ffnord-alfred-announce/announce.sh -b ffhb -i ffhb > /dev/null
```
Falls ein andere Hostname als der systemweite für alfred benutzt werden soll,
 ändern sie die Datei '/root/ffnord-alfred-announce/nodeinfo.d/hostname' ab.

# TODO
Anpassung von fastd durch das Anlegen von systemd-units und netctl.

# Author(en)
genofire (mail,jid:geno@fireorbit.de - jid:genofire@chat.ffhb.de)
