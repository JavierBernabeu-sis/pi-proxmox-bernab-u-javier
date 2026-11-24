# Proyecto Intermodular: nodo Proxmox individual

Alumno/a: Bernabeu_Javier

URL GitHub: https://github.com/JavierBernabeu-sis/pi-proxmox-bernab-u-javier

## Objetivo

Montar un laboratorio de virtualización con Proxmox VE para practicar la administración de un nodo y de un servicio de red (Samba en `srv-aso`, probado desde `cli-linux`), con automatización, copias y revisión de seguridad, sin coste de licencias y sin modificar la red del centro.

## Modalidad

Virtualizada. Proxmox VE 9.2 se instala en una máquina virtual de VirtualBox (`pve-bernabeu`, 3 vCPU, 8 GiB de RAM y 80 GiB de disco) en el PC de prácticas. Se elige así porque no hay un equipo físico libre para formatear, el PC de prácticas tiene más RAM (15,4 GiB) que el equipo del inventario (8 GB) y el laboratorio queda aislado en una red solo-anfitrión sin tocar la red del aula.

## Indice de documentacion

- [Ficha inicial](docs/00-ficha-inicial.md)
- [Bitacora de sesiones](docs/00-bitacora.md)
- [Inventario](docs/01-inventario-hardware.md)
- [Diagrama de red](docs/02-diagrama-red.md)
- [Instalacion de Proxmox](docs/03-instalacion-proxmox.md)
- [Plantilla Linux](docs/04-plantilla-linux.md)
- [VMs y LXC](docs/05-vms-lxc.md)
- [Servicio ASO](docs/06-servicio-aso.md)
- [Automatizacion Bash y Ansible](docs/07-automatizacion.md)
- [Copias y restauracion](docs/08-copias-restauracion.md)
- [Rendimiento y seguridad](docs/09-rendimiento-seguridad.md)
- [Memoria tecnica](docs/10-memoria-tecnica.md)
- [Evidencias](evidencias/README.md)

## Estado

- [x] GitHub creado y primer commit publicado
- [x] URL entregada y acceso docente comprobado
- [x] Modalidad indicada
- [x] Inventario
- [x] Diagrama de red
- [x] Instalacion Proxmox
- [ ] Plantilla Linux
- [ ] VMs/LXC
- [ ] Servicio ASO
- [ ] Scripts Bash
- [ ] Ansible
- [ ] Copias y restauracion
- [ ] Rendimiento y seguridad
- [ ] Memoria final
