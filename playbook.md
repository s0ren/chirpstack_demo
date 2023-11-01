# Docker compose variant

## 0. TEC-IOT WiFi

Jeg kobler alle enheder, og min pc op på WiFi-accespointet `TEC-IOT`. Det kan vi styre muligheder og begrænsninger, uden at vente på sagsbehandling fra ITCN.

SSID
: TEC-IOT

pw
: 42090793

### 0.1 Admin login

user
: admin

pw
: 7ellowsTon3


## 1. Raspberry Pi image

Benyt imager <https://www.raspberrypi.com/software/>

![General settings](2023-10-31-10-50-55.png)

Hostname
:   chirp.local

User
:   pi

passwd
: raspberry

SSID
: TEC-IOT

pw
: 42090793

Wireless LAN Country 
: DK

![](2023-10-31-11-09-59.png)

Enable SSH
:  [x] _Use password auth..._

### `ssh` til pi'en

Med din pc på `TEC-IOT`;

I powershell eller noget efter egetvalg;
Brug bruger `pi`, og hostnavnet `chirp.local`

    ssh pi@chirp.local

Sig `yes` til at oprette fingerprint

og vi er inde!

    pi@chirp:~ $ 

