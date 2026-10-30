# homelab-proxmox-cyberlab

Laboratorio de virtualización sobre Proxmox VE en un Beelink SER5 MAX. Entorno para docencia (ASIR, DAM, Ciberseguridad), práctica de DevOps/Cloud y proyectos personales.

## Hardware

| Componente | Detalle |
|---|---|
| Equipo | Beelink SER5 MAX |
| CPU | AMD Ryzen (16 hilos) |
| RAM | 19 GB |
| Disco | 98 GB SSD |
| SO base | Proxmox VE 9.2 |
| Red | Tailscale VPN (IP: 100.75.225.5) / LAN (192.168.1.141) |

## Arquitectura

```
PC Windows (puesto de control)
    │
    ├── Tailscale ──► Proxmox Web UI (https://IP:8006)
    │                    │
    │                    ├── [VM] kali-cyber        (4 GB RAM, 2 vCPU, 25 GB)
    │                    ├── [VM] srv-win           (4 GB RAM, 2 vCPU, 30 GB)
    │                    ├── [LXC] lab-infra        (2 GB RAM, 2 vCPU, 10 GB)
    │                    ├── [LXC] jhon-bot         (512 MB RAM, 1 vCPU, 5 GB)
    │                    └── [LXC] lab-ctf          (1 GB RAM, 1 vCPU, 5 GB)
    │
Portátil Omarchy (administración secundaria)
    │
    └── Tailscale ──► SSH / Proxmox Web UI
```

> No todo encendido a la vez. Se levanta lo que se necesite en cada momento.

## Máquinas virtuales y contenedores

### kali-cyber (VM)

Kali Linux con escritorio BSPWM personalizado (estilo cyberpunk).

- **Uso:** demos en clase, pentesting, máster de ciberseguridad
- **Acceso:** SSH, RDP (xrdp), noVNC desde Proxmox
- **Post-instalación:**
  - `auto-bspwm` de r1vs3c para el entorno cyberpunk
  - `kali-linux-large` para herramientas completas
  - Tailscale dentro de la VM (opcional, para acceso directo)

### srv-win (VM)

Windows Server 2025 (evaluación 180 días).

- **Uso:** Active Directory, DNS, DHCP, GPOs, usuarios/grupos/permisos (ASO)
- **Acceso:** Escritorio remoto (RDP) desde el PC

### lab-infra (LXC)

Ubuntu Server ligero como laboratorio de DevOps y Cloud.

- **Uso:** Docker, K3s (Kubernetes ligero), Terraform, Ansible, CLIs de AWS/Azure
- **Acceso:** SSH
- **Contenido:**
  - Docker CE + Docker Compose
  - K3s (single node)
  - Terraform + provider de Proxmox
  - Ansible
  - AWS CLI + Azure CLI

### jhon-bot (LXC)

Bot de Telegram @JhonSystembot (HEO-AGENT).

- **Uso:** agente personal, automatizaciones, notificaciones
- **Acceso:** siempre encendido, se gestiona por Telegram
- **Contenido:**
  - OpenClaw + Gemini
  - n8n (workflows)
  - Edge-TTS

### lab-ctf (LXC)

Entorno de prácticas de ciberseguridad.

- **Uso:** máster ciberseguridad, prácticas con alumnos
- **Acceso:** SSH, puede exponerse a alumnos con usuarios limitados
- **Contenido:**
  - Docker para DockerLabs (máquinas vulnerables)
  - PortSwigger labs (acceso web)
  - Posibilidad de crear un LXC por alumno para CTFs

## Progreso

### Fase 0 — Instalación de Proxmox
- [ ] Grabar ISO de Proxmox 9.2 en USB con Rufus
- [ ] Instalar Proxmox en el Beelink (borrar Ubuntu)
- [ ] Configurar red (IP estática en LAN)
- [ ] Instalar Tailscale en Proxmox
- [ ] Acceder al panel web desde el PC (https://IP:8006)
- [ ] Configurar repositorios (quitar enterprise, añadir no-subscription)
- [ ] Subir ISOs de Kali y Windows Server

### Fase 1 — Kali Cyber
- [ ] Crear VM kali-cyber
- [ ] Instalar Kali Linux (XFCE, idioma español)
- [ ] Instalar SSH y xrdp
- [ ] Instalar auto-bspwm (entorno cyberpunk)
- [ ] Instalar kali-linux-large
- [ ] Hacer snapshot "base-limpia"
- [ ] Probar conexión RDP desde el PC

### Fase 2 — Windows Server
- [ ] Descargar ISO de Windows Server 2025 (evaluación)
- [ ] Crear VM srv-win
- [ ] Instalar Windows Server
- [ ] Promocionar a controlador de dominio (AD)
- [ ] Crear usuarios, grupos y OUs
- [ ] Configurar DNS y DHCP
- [ ] Aplicar GPO de prueba
- [ ] Hacer snapshot "ad-limpio"

### Fase 3 — Lab Infra (DevOps/Cloud)
- [ ] Crear LXC lab-infra (Ubuntu)
- [ ] Instalar Docker + Docker Compose
- [ ] Instalar K3s
- [ ] Instalar Terraform + provider Proxmox
- [ ] Instalar Ansible
- [ ] Instalar AWS CLI + Azure CLI
- [ ] Crear cuentas gratuitas (AWS Free Tier, Azure for Students)
- [ ] Hacer snapshot "devops-base"

### Fase 4 — Jhon Bot
- [ ] Crear LXC jhon-bot
- [ ] Migrar OpenClaw + configuración
- [ ] Configurar bot de Telegram
- [ ] Configurar n8n workflows
- [ ] Verificar que arranca con Proxmox

### Fase 5 — Lab CTF
- [ ] Crear LXC lab-ctf
- [ ] Instalar Docker
- [ ] Desplegar primera máquina de DockerLabs
- [ ] Probar ataque desde kali-cyber
- [ ] Documentar cómo crear usuarios para alumnos

### Fase 6 — Acceso para alumnos
- [ ] Crear usuarios SSH limitados en lab-ctf
- [ ] Documentar cómo se conectan los alumnos
- [ ] Probar acceso desde la red del aula
- [ ] (Opcional) Crear LXC individual por alumno para CTFs

### Extras (futuro)
- [ ] Montar LocalStack para simular AWS
- [ ] Reverse proxy con Nginx Proxy Manager
- [ ] Ampliar disco (SSD externo o NVMe M.2)
- [ ] Segundo nodo Proxmox (otro mini PC)

## Conexiones rápidas

### Desde el PC Windows

```powershell
# Panel de Proxmox (navegador)
https://100.75.225.5:8006

# SSH al host Proxmox
ssh root@100.75.225.5

# SSH a kali-cyber (una vez configurada)
ssh heo@IP-de-kali

# Escritorio remoto a Windows Server
mstsc /v:IP-de-srv-win

# Escritorio remoto a Kali (xrdp)
mstsc /v:IP-de-kali
```

### Desde Omarchy (portátil)

```bash
# Panel de Proxmox (navegador)
https://100.75.225.5:8006

# SSH al host Proxmox
ssh root@100.75.225.5

# SSH a cualquier LXC/VM
ssh usuario@IP-del-contenedor
```

## Notas

- Las IPs de las VMs y LXC se asignan por DHCP del bridge de Proxmox o se fijan manualmente. Se documentan aquí conforme se crean.
- Tailscale en Proxmox permite acceso desde cualquier sitio. En clase depende de la red del aula.
- Los snapshots son obligatorios antes de cada cambio importante. Nombrarlos con fecha: `2026-10-30-base-limpia`.
- Windows Server evaluación caduca a los 180 días. Se puede reinstalar.

## Recursos

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [auto-bspwm (r1vs3c)](https://github.com/r1vs3c/auto-bspwm)
- [DockerLabs](https://dockerlabs.es/)
- [PortSwigger Web Security](https://portswigger.net/web-security/all-topics)
- [Tailscale Download](https://tailscale.com/download)
- [Windows Server 2025 Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025)
- [Kali Linux Downloads](https://www.kali.org/get-kali/)
- [K3s - Lightweight Kubernetes](https://k3s.io/)
- [Terraform Proxmox Provider](https://registry.terraform.io/providers/Telmate/proxmox/)
