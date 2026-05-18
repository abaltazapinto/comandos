## Ação — 1 passo ✅

Adiciona esta secção ao `.md`:

````md
---

## Desktop Bragança

Ver IP do desktop Linux:

```bash
ip a
hostname -I
ip route
````

Ver rapidamente apenas IPv4:

```bash
ip -4 addr
```

Ver interface principal:

```bash
ip -br addr
```

Exemplo esperado:

```text
eth0    UP   192.168.1.X/24
```

ou

```text
wlan0   UP   192.168.1.X/24
```

Ping do Raspberry Pi para desktop:

```bash
ping 192.168.1.X
```

SSH para desktop:

```bash
ssh user@192.168.1.X
```

---

## IPs importantes

Raspberry Pi Wi-Fi:
192.168.1.21

Raspberry Pi Ethernet portátil:
10.42.0.53

Router NOS:
192.168.1.1

Desktop Bragança:
192.168.1.X

````

## Objetivo

Criar um mapa real da tua LAN doméstica.

Em homelab / embedded Linux isto torna-se crítico para:
- SSH
- NFS/SMB
- Docker services
- automação
- monitoring
- remote debugging

## Como pensar

A tua rede agora já tem:

```text
router
├── desktop
├── raspberry pi
└── telemóvel
````

Isto já é arquitetura de sistemas distribuídos em pequena escala 🎯
