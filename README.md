<p align="center">
  <img src="https://img.shields.io/badge/HOMELAB-Proxmox_CyberLab-00ff41?style=for-the-badge&logo=proxmox&logoColor=white" alt="Homelab Proxmox CyberLab"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Host-Beelink_SER5_MAX-ff6600?style=flat-square&logo=linux&logoColor=white"/>
  <img src="https://img.shields.io/badge/Hypervisor-Proxmox_VE_9.2-E57000?style=flat-square&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/VPN-Tailscale-0052FF?style=flat-square&logo=tailscale&logoColor=white"/>
  <img src="https://img.shields.io/badge/VMs-5-00ff41?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-Private-red?style=flat-square"/>
</p>

---

```
 ██████╗██╗   ██╗██████╗ ███████╗██████╗ ██╗      █████╗ ██████╗ 
██╔════╝╚██╗ ██╔╝██╔══██╗██╔════╝██╔══██╗██║     ██╔══██╗██╔══██╗
██║      ╚████╔╝ ██████╔╝█████╗  ██████╔╝██║     ███████║██████╔╝
██║       ╚██╔╝  ██╔══██╗██╔══╝  ██╔══██╗██║     ██╔══██║██╔══██╗
╚██████╗   ██║   ██████╔╝███████╗██║  ██║███████╗██║  ██║██████╔╝
 ╚═════╝   ╚═╝   ╚═════╝ ╚══════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═════╝ 
```

<p align="center">
  <strong>🖥️ Virtualización · Ciberseguridad · DevOps · Docencia</strong><br/>
  <em>Laboratorio completo en un mini PC. Accesible desde cualquier sitio.</em>
</p>

---

## 🔮 ¿Qué es CyberLab?

**CyberLab** es un laboratorio de virtualización sobre [Proxmox VE](https://www.proxmox.com/) corriendo en un **Beelink SER5 MAX**. Sirve para tres cosas:

- 🎓 **Docencia** — Demos en clase de ASIR, DAM y Máster de Ciberseguridad
- 🛡️ **Ciberseguridad** — Pentesting con Kali, CTFs con DockerLabs, labs de PortSwigger
- ☁️ **DevOps / Cloud** — Docker, Kubernetes (K3s), Terraform, Ansible, CLIs de AWS/Azure

Todo accesible por **Tailscale** desde el PC, el portátil o el móvil. Sin abrir puertos. Sin estar en casa.

---

## 🖥️ Hardware

```
┌─────────────────────────────────────────────┐
│  BEELINK SER5 MAX                           │
│                                             │
│  CPU ··· AMD Ryzen · 16 hilos               │
│  RAM ··· 19 GB DDR4                         │
│  SSD ··· 98 GB                              │
│  NET ··· Tailscale (100.75.225.5)           │
│          LAN (192.168.1.141)                │
│  OS  ··· Proxmox VE 9.2                    │
└─────────────────────────────────────────────┘
```

---

## 🌐 Arquitectura

```
  ┌──────────────────┐     ┌──────────────────┐
  │  PC Windows      │     │  Portátil Omarchy │
  │  (puesto control)│     │  (admin secundario)│
  └────────┬─────────┘     └────────┬──────────┘
           │         Tailscale      │
           └────────────┬───────────┘
                        │
              ┌─────────▼──────────┐
              │   PROXMOX VE 9.2   │
              │   Beelink SER5 MAX │
              │                    │
              │  ┌──────────────┐  │
              │  │ kali-cyber   │  │  VM · 4GB · Pentesting
              │  ├──────────────┤  │
              │  │ srv-win      │  │  VM · 4GB · Windows Server
              │  ├──────────────┤  │
              │  │ lab-infra    │  │  LXC · 2GB · Docker/K3s/Terraform
              │  ├──────────────┤  │
              │  │ jhon-bot     │  │  LXC · 512MB · Telegram bot 24/7
              │  ├──────────────┤  │
              │  │ lab-ctf      │  │  LXC · 1GB · DockerLabs/CTFs
              │  └──────────────┘  │
              └────────────────────┘
```

> ⚡ No todo encendido a la vez. Se levanta lo que se necesite en cada momento.

---

## 🧩 Máquinas Virtuales y Contenedores

### 🐉 kali-cyber `VM`

Kali Linux con entorno **BSPWM cyberpunk** ([auto-bspwm](https://github.com/r1vs3c/auto-bspwm)).

| Spec | Valor |
|------|-------|
| RAM | 4 GB |
| vCPU | 2 |
| Disco | 25 GB |
| Acceso | SSH · RDP (xrdp) · noVNC |

**Uso:** demos en clase, pentesting, máster de ciberseguridad, CTFs

**Post-instalación:**
- `auto-bspwm` → entorno tiling cyberpunk
- `kali-linux-large` → herramientas completas
- DockerLabs + PortSwigger labs

---

### 🪟 srv-win `VM`

Windows Server 2025 (evaluación 180 días).

| Spec | Valor |
|------|-------|
| RAM | 4 GB |
| vCPU | 2 |
| Disco | 30 GB |
| Acceso | RDP desde el PC |

**Uso:** Active Directory, DNS, DHCP, GPOs, usuarios/grupos/permisos (ASO)

---

### ⚙️ lab-infra `LXC`

Ubuntu Server — laboratorio DevOps y Cloud.

| Spec | Valor |
|------|-------|
| RAM | 2 GB |
| vCPU | 2 |
| Disco | 10 GB |
| Acceso | SSH |

**Stack instalado:**
- Docker CE + Docker Compose
- K3s (Kubernetes ligero, single node)
- Terraform + provider Proxmox
- Ansible
- AWS CLI + Azure CLI

---

### 🤖 jhon-bot `LXC`

Bot de Telegram **@JhonSystembot** (HEO-AGENT). Siempre encendido.

| Spec | Valor |
|------|-------|
| RAM | 512 MB |
| vCPU | 1 |
| Disco | 5 GB |
| Acceso | Telegram |

**Stack:** OpenClaw · Gemini · n8n · Edge-TTS

---

### 🏴‍☠️ lab-ctf `LXC`

Entorno de prácticas de ciberseguridad para alumnos.

| Spec | Valor |
|------|-------|
| RAM | 1 GB |
| vCPU | 1 |
| Disco | 5 GB |
| Acceso | SSH (usuarios limitados por alumno) |

**Stack:** Docker + DockerLabs (máquinas vulnerables) + PortSwigger

---

## ✅ Progreso

### Fase 0 — Instalación de Proxmox
- [ ] Grabar ISO de Proxmox 9.2 en USB (Rufus)
- [ ] Instalar Proxmox en el Beelink
- [ ] Configurar red (IP estática en LAN)
- [ ] Instalar Tailscale en Proxmox
- [ ] Acceder al panel web (`https://IP:8006`)
- [ ] Configurar repositorios (quitar enterprise, añadir no-subscription)
- [ ] Subir ISOs (Kali, Windows Server)

### Fase 1 — Kali Cyber
- [ ] Crear VM kali-cyber
- [ ] Instalar Kali Linux (XFCE, español)
- [ ] Instalar SSH + xrdp
- [ ] Ejecutar auto-bspwm (entorno cyberpunk)
- [ ] Instalar kali-linux-large
- [ ] Snapshot `base-limpia`
- [ ] Probar RDP desde el PC

### Fase 2 — Windows Server
- [ ] Descargar ISO Windows Server 2025 (evaluación)
- [ ] Crear VM srv-win
- [ ] Instalar Windows Server
- [ ] Promocionar a controlador de dominio (AD)
- [ ] Crear usuarios, grupos y OUs
- [ ] Configurar DNS + DHCP
- [ ] Aplicar GPO de prueba
- [ ] Snapshot `ad-limpio`

### Fase 3 — Lab Infra (DevOps/Cloud)
- [ ] Crear LXC lab-infra
- [ ] Instalar Docker + Docker Compose
- [ ] Instalar K3s
- [ ] Instalar Terraform + provider Proxmox
- [ ] Instalar Ansible
- [ ] Instalar AWS CLI + Azure CLI
- [ ] Crear cuentas gratuitas (AWS Free Tier, Azure for Students)
- [ ] Snapshot `devops-base`

### Fase 4 — Jhon Bot
- [ ] Crear LXC jhon-bot
- [ ] Migrar OpenClaw + configuración
- [ ] Configurar bot de Telegram
- [ ] Configurar n8n workflows
- [ ] Verificar arranque automático con Proxmox

### Fase 5 — Lab CTF
- [ ] Crear LXC lab-ctf
- [ ] Instalar Docker
- [ ] Desplegar primera máquina de DockerLabs
- [ ] Probar ataque desde kali-cyber
- [ ] Documentar acceso para alumnos

### Fase 6 — Acceso para alumnos
- [ ] Crear usuarios SSH limitados
- [ ] Documentar conexión desde el aula
- [ ] Probar acceso por Tailscale invitación
- [ ] (Opcional) LXC individual por alumno para CTFs

### 🔮 Extras (futuro)
- [ ] LocalStack (simular AWS local)
- [ ] Nginx Proxy Manager (reverse proxy)
- [ ] Ampliar disco (SSD externo o NVMe M.2)
- [ ] Segundo nodo Proxmox (otro mini PC)

---

## 🔌 Conexiones Rápidas

### Desde Windows (PC principal)

```powershell
# Panel de Proxmox
https://100.75.225.5:8006

# SSH al host Proxmox
ssh root@100.75.225.5

# SSH a Kali
ssh heo@<IP-kali>

# Escritorio remoto a Windows Server
mstsc /v:<IP-srv-win>

# Escritorio remoto a Kali (xrdp)
mstsc /v:<IP-kali>
```

### Desde Omarchy (portátil)

```bash
# Panel de Proxmox
https://100.75.225.5:8006

# SSH al host
ssh root@100.75.225.5

# SSH a cualquier VM/LXC
ssh usuario@<IP-contenedor>
```

---

## 📚 Recursos

| Recurso | Link |
|---------|------|
| Proxmox VE Docs | [pve.proxmox.com](https://pve.proxmox.com/pve-docs/) |
| auto-bspwm | [r1vs3c/auto-bspwm](https://github.com/r1vs3c/auto-bspwm) |
| DockerLabs | [dockerlabs.es](https://dockerlabs.es/) |
| PortSwigger | [portswigger.net](https://portswigger.net/web-security/all-topics) |
| Tailscale | [tailscale.com](https://tailscale.com/download) |
| Win Server 2025 | [Microsoft Eval](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025) |
| Kali Linux | [kali.org](https://www.kali.org/get-kali/) |
| K3s | [k3s.io](https://k3s.io/) |
| Terraform Proxmox | [registry.terraform.io](https://registry.terraform.io/providers/Telmate/proxmox/) |

---

## 📝 Notas

- Las IPs de VMs/LXC se documentan aquí conforme se crean
- Snapshots obligatorios antes de cada cambio: `YYYY-MM-DD-descripcion`
- Windows Server evaluación caduca a los 180 días (se reinstala)
- Tailscale permite acceso desde cualquier sitio sin abrir puertos

---

## 🛠️ Stack

<p>
  <img src="https://img.shields.io/badge/Proxmox-E57000?style=for-the-badge&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white"/>
  <img src="https://img.shields.io/badge/Windows_Server-0078D4?style=for-the-badge&logo=windows&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white"/>
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white"/>
  <img src="https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white"/>
  <img src="https://img.shields.io/badge/Tailscale-0052FF?style=for-the-badge&logo=tailscale&logoColor=white"/>
</p>

---

<p align="center">
  <strong>「 CyberLab 」</strong><br/>
  <em>Built by <a href="https://github.com/HEO-80">HEO-80</a> · Powered by Proxmox · Connected by Tailscale</em><br/><br/>
  <img src="https://img.shields.io/badge/STATUS-BUILDING-ff6600?style=for-the-badge"/>
</p>
