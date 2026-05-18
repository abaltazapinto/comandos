/## Ação — 1 passo ✅

Guarda isto no teu `.md`:

````md
# Raspberry Pi 5 + Nextcloud — Comandos úteis

## URLs Nextcloud

Wi-Fi NOS:

```text
http://192.168.1.21:8080
````

Ethernet via portátil:

```text
http://10.42.0.53:8080
```

Status Nextcloud:

```text
http://192.168.1.21:8080/status.php
http://10.42.0.53:8080/status.php
```

---

## Rede — diagnóstico

```bash
ip a
ip -br addr
ip route
hostname -I
ping -c 3 8.8.8.8
ping -c 3 google.com
ping -c 3 10.42.0.1
```

---

## NetworkManager

Listar ligações:

```bash
nmcli connection show
```

Ativar Wi-Fi NOS 5 GHz:

```bash
sudo nmcli connection up "NOS-51F6-5"
```

Fixar IP Wi-Fi:

```bash
sudo nmcli connection modify "NOS-51F6-5" \
ipv4.method manual \
ipv4.addresses 192.168.1.21/24 \
ipv4.gateway 192.168.1.1 \
ipv4.dns "8.8.8.8 1.1.1.1"
```

Fixar IP Ethernet via portátil:

```bash
sudo nmcli connection modify "Wired connection 1" \
ipv4.method manual \
ipv4.addresses 10.42.0.53/24 \
ipv4.gateway 10.42.0.1 \
ipv4.dns "8.8.8.8 1.1.1.1"
```

---

## Docker / Nextcloud

Ver containers:

```bash
sudo docker ps
```

Logs Nextcloud:

```bash
sudo docker logs nc_app --tail 50
```

Entrar no container:

```bash
sudo docker exec -it nc_app bash
```

Adicionar IP aos trusted domains:

```bash
su -s /bin/bash www-data -c 'php occ config:system:set trusted_domains 2 --value=10.42.0.53'
```

Ver trusted domains:

```bash
su -s /bin/bash www-data -c 'php occ config:system:get trusted_domains'
```

Sair do container:

```bash
exit
```

---

## Storage / memória

```bash
df -h
free -h
```

---

## Temperatura / throttling

```bash
vcgencmd measure_temp
vcgencmd get_throttled
```

---

## Reinício

```bash
sudo reboot
```

---

## Estado atual

```text
Raspberry Pi 5
SSD NVMe boot
Docker Compose
Nextcloud + MariaDB + Redis

Wi-Fi:
192.168.1.21

Ethernet via portátil:
10.42.0.53

Gateway Wi-Fi:
192.168.1.1

Gateway Ethernet:
10.42.0.1
```

```


# Raspberry Pi 5 + Nextcloud + Docker — Useful Commands

# =========================
# SYSTEM OBSERVABILITY
# =========================

## CPU temperature
vcgencmd measure_temp

## Clock / throttling state
vcgencmd get_throttled

## Watch temperature continuously
watch -n 1 vcgencmd measure_temp

## CPU information
cat /proc/cpuinfo

## Memory usage
free -h

## Storage usage
df -h

## Mounted devices
lsblk

## PCIe / NVMe devices
lspci

## USB devices
lsusb

## Kernel version
uname -a

## OS release
cat /etc/os-release

---

# =========================
# NETWORKING
# =========================

## IP addresses
ip a

## Routing table
ip route

## Ping test
ping 8.8.8.8

## DNS test
ping google.com

## Open listening ports
sudo ss -tulpn

## Active connections
ss -tunap

## NetworkManager connections
nmcli connection show

---

# =========================
# SYSTEMD / SERVICES
# =========================

## Service status
systemctl status docker

## Running services
systemctl list-units --type=service --state=running

## Failed services
systemctl --failed

## Restart service
sudo systemctl restart docker

## Enable service on boot
sudo systemctl enable docker

## View logs
journalctl -xe

## Live logs
journalctl -f

---

# =========================
# DOCKER
# =========================

## Running containers
docker ps

## All containers
docker ps -a

## Images
docker images

## Container logs
docker logs nc_app

## Follow logs live
docker logs -f nc_app

## Restart container
docker restart nc_app

## Stop container
docker stop nc_app

## Start container
docker start nc_app

## Remove stopped containers
docker container prune

## Remove unused images
docker image prune

## Docker disk usage
docker system df

## Full Docker cleanup
docker system prune -a

---

# =========================
# DOCKER COMPOSE
# =========================

## Start stack
sudo docker compose up -d

## Stop stack
sudo docker compose down

## Restart stack
sudo docker compose restart

## View compose logs
sudo docker compose logs

## Live logs
sudo docker compose logs -f

## Rebuild containers
sudo docker compose up -d --build

---

# =========================
# NEXTCLOUD
# =========================

## Enter Nextcloud container
docker exec -it nc_app bash

## Enter MariaDB container
docker exec -it nc_db bash

## Nextcloud OCC commands
docker exec -u www-data nc_app php occ

## Scan files
docker exec -u www-data nc_app php occ files:scan --all

## Maintenance mode ON
docker exec -u www-data nc_app php occ maintenance:mode --on

## Maintenance mode OFF
docker exec -u www-data nc_app php occ maintenance:mode --off

## Repair database
docker exec -u www-data nc_app php occ maintenance:repair

## List apps
docker exec -u www-data nc_app php occ app:list

## Check config
docker exec -u www-data nc_app php occ config:list system

---

# =========================
# NEXTCLOUD CONFIG
# =========================

## Open config.php
nano ~/containers/nextcloud/app/config/config.php

## Trusted domains section
'trusted_domains' =>
array (
  0 => '192.168.137.190:8080',
  1 => '192.168.1.21:8080',
),

---

# =========================
# STORAGE / FILESYSTEM
# =========================

## Disk usage by folders
du -sh *

## Biggest directories
du -h --max-depth=1 | sort -hr

## Find large files
find . -type f -size +500M

## NVMe SMART info
sudo smartctl -a /dev/nvme0

---

# =========================
# SSH
# =========================

## Enable SSH
sudo systemctl enable ssh
sudo systemctl start ssh

## SSH into Raspberry
ssh abaltaza@192.168.X.X

## Copy files via SCP
scp file.txt abaltaza@192.168.X.X:/home/abaltaza/

---

# =========================
# UPDATES
# =========================

## Update packages
sudo apt update

## Upgrade packages
sudo apt upgrade -y

## Full upgrade
sudo apt full-upgrade -y

## Firmware updates
sudo rpi-update

NOTE:
rpi-update is experimental.

---

# =========================
# LOGS & DEBUGGING
# =========================

## Kernel logs
dmesg

## Live kernel logs
dmesg -w

## Boot logs
journalctl -b

## Docker logs
docker logs nc_app

## Failed login attempts
sudo journalctl | grep ssh

---

# =========================
# FILE TRANSFER / NAS
# =========================

## Install Samba
sudo apt install samba

## Install NFS
sudo apt install nfs-kernel-server

## Shared folders
sudo nano /etc/samba/smb.conf

---

# =========================
# PERFORMANCE
# =========================

## CPU usage
top

## Better top
htop

## Install htop
sudo apt install htop

## IO monitoring
iostat

## Install iostat
sudo apt install sysstat

---

# =========================
# SECURITY
# =========================

## Open ports
sudo ss -tulpn

## Firewall status
sudo ufw status

## Enable firewall
sudo ufw enable

## Allow SSH
sudo ufw allow ssh

## Allow Nextcloud
sudo ufw allow 8080/tcp

---

# =========================
# BACKUPS
# =========================

## Backup Nextcloud config
cp config.php config.php.backup

## Backup Docker compose
cp docker-compose.yml docker-compose.yml.backup

## Tar backup
tar -czvf backup.tar.gz folder/

---

# =========================
# ENGINEERING MINDSET
# =========================

Before changing anything:
1. check logs
2. verify storage
3. verify networking
4. understand current state
5. backup config

Avoid:
- changing config during sync
- rebooting during writes
- deleting local files before sync finishes
- editing containers manually without persistence


###############################################

# Raspberry Pi 5 — Daily Health Check 🌡️🌀

## Temperatura atual

```bash
vcgencmd measure_temp
```

## Throttling

```bash
vcgencmd get_throttled
```

```text
throttled=0x0      -> sem throttling reportado
throttled=0x80000  -> já aconteceu evento térmico no passado
```

## Fan / cooler detetado pelo sistema

```bash
for c in /sys/class/thermal/cooling_device*; do
  echo "=== $c ==="
  cat "$c/type"
  cat "$c/cur_state"
  cat "$c/max_state"
done
```

Exemplo:

```text
=== /sys/class/thermal/cooling_device0 ===
pwm-fan
3
4
```

```text
pwm-fan      -> fan reconhecida
cur_state 3 -> fan comandada em nível 3
max_state 4 -> nível máximo é 4
```

## Monitorizar temperatura + fan

```bash
watch -n 1 'vcgencmd measure_temp; echo; for c in /sys/class/thermal/cooling_device*; do echo -n "$(cat "$c/type"): "; cat "$c/cur_state"; done'
```

Sair:

```text
Ctrl + C
```

## Monitorizar temperatura + fan + throttling

```bash
watch -n 1 'vcgencmd measure_temp; echo; for c in /sys/class/thermal/cooling_device*; do echo -n "$(cat "$c/type"): "; cat "$c/cur_state"; done; echo; vcgencmd get_throttled'
```

Sair:

```text
Ctrl + C
```

## Criar comando curto `pifan`

Executar uma vez:

```bash
cat <<'EOF' >> ~/.bashrc

pifan() {
  watch -n 1 'vcgencmd measure_temp; echo; for c in /sys/class/thermal/cooling_device*; do echo -n "$(cat "$c/type"): "; cat "$c/cur_state"; done; echo; vcgencmd get_throttled'
}
EOF

source ~/.bashrc
```

Depois usar:

```bash
pifan
```

Sair:

```text
Ctrl + C
```

## Interpretação rápida da fan

```text
pwm-fan: 0 -> fan desligada ou sem necessidade térmica
pwm-fan: 1 -> arrefecimento baixo
pwm-fan: 2 -> arrefecimento médio
pwm-fan: 3 -> arrefecimento ativo
pwm-fan: 4 -> arrefecimento máximo
```

## Padrão saudável

```text
temperatura sobe
fan aumenta
temperatura estabiliza ou baixa
```

Exemplo:

```text
temp=72.0'C
pwm-fan: 4

depois

temp=68.5'C
pwm-fan: 3
```

## Padrão preocupante

```text
temp > 70'C constante
pwm-fan: 4 constante
throttled muda frequentemente
```

Possíveis causas:

```text
- má ventilação
- caixa fechada
- mau contacto do dissipador com o SoC
- fan ligada mas pouco eficaz
- carga alta: Chromium, Docker, Nextcloud, updates
```

## Ver processos que estão a aquecer o Pi

```bash
top
```

Ou:

```bash
htop
```

## Ver carga do sistema

```bash
uptime
```

Exemplo:

```text
load average: 0.80, 1.20, 1.50
```

## Ver Docker / Nextcloud

```bash
docker ps
```

Se estiveres na pasta do Nextcloud com `compose.yml`:

```bash
docker compose ps
```

## Teste de idle térmico

Fechar Chromium e deixar só:

```bash
pifan
```

Observar durante 3 minutos.

```text
Baixa para 55-65'C -> carga do desktop/Chromium era relevante
Fica sempre 70-72'C -> investigar cooling/contacto térmico/caixa
```

## Nota de engenharia

```text
pwm-fan mostra o nível pedido pelo sistema.
Não mede RPM real.

pwm-fan: 3
=
Linux/firmware está a pedir arrefecimento em nível 3.
```

## Comando diário recomendado

```bash
pifan
```
