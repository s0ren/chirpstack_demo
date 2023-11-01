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

### 1.1 `ssh` til pi'en

Med din pc på `TEC-IOT`;

I powershell eller noget efter egetvalg;
Brug bruger `pi`, og hostnavnet `chirp.local`

    ssh pi@chirp.local

Sig `yes` til at oprette fingerprint

og vi er inde!

    pi@chirp:~ $ 

## 2.1 Kør chirpstack som docker compose stack

### 2.1.1 Docker på Raspberry pi

Det er lidt besværligt fordi vi _ikke_ bare kan installere desktop udgaven.

Jeg følger vejledningen på <https://docs.docker.com/engine/install/raspberry-pi-os/>, og nogle af trinene på <https://docs.docker.com/engine/install/linux-postinstall/>

Som altid først: 

    sudo apt -y update
    sudo apt -y upgrade

Der efter vælger jeg at installere med apt metoden, fordi den er lettest at vedligeholde.
<https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-repository> (samme side som ovenfor, men til bogmærket `install-using-the-repository`).  

Først skal dockers repo installeres, dvs tilføjes til apt's pakkeliste system.

Det er nok bedst at kopiere en linje over ad gangen... husk at nogel linjer autowrappes, men skal udvføres som een

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/raspbian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Set up Docker's APT repository:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/raspbian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

og så selve installationen, med _alle_ pakkerne:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Check installationen:

    sudo docker run hello-world

I mellem meget andet skulle du gerne se:

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

Så __virker__ det!

Nu skulle det være muligt at opgradere docker med `sudo apt upgrade`. Kan jeg ikke lige teste, da det jo allerede er nyeste version...

#### 2.1.1.1 Docker skal kunne køres af ikke-root brugere

Se <https://docs.docker.com/engine/install/linux-postinstall/> og bemærk advarslerne.

Oprette en gruppe af docker brugere

    sudo groupadd docker

Oups: den findes allerede:

    groupadd: group 'docker' already exists

tilføj mig selv (her `pi`) til gruppen

    sudo usermod -aG docker $USER

(eksekverer uden output...) "intet nyt er godt nyt"

Exit ud af `ssh`forbindelsen, og login igen

test helloworld, uden sudo

    docker run hello-world


#### 2.2.1.2 Docker som service

Enable:

    sudo systemctl enable docker.service
    sudo systemctl enable containerd.service

Disable:

    sudo systemctl disable docker.service
    sudo systemctl disable containerd.service


Jeg tror det var det... med docker installation



