# Ficha inicial

| Campo | Contenido |
|---|---|
| Identificador del alumno | Javier Bernabeu |
| URL GitHub | https://github.com/JavierBernabeu-sis/pi-proxmox-bernab-u-javier |
| Acceso docente comprobado | Comprobado |
| Modalidad fisica o virtualizada | Virtualizada |
| Recursos conocidos | PC de prácticas con VirtualBox 7.2.4, CPU Intel con VT-x, 15,4 GiB de RAM y 65 GiB libres en disco. VM `pve-bernabeu` con 3 vCPU, 8 GiB de RAM y disco dinámico de 80 GiB. Equipo físico de la fase 1 (Pentium G4560, 8 GB, SSD 250 GB) solo para el inventario |
| Nombre del nodo | pve-bernabeu (pve-bernabeu.lab.test) |
| Servicio ASO previsto | Samba en `srv-aso` (Ubuntu Server sin interfaz gráfica) con la carpeta compartida `/srv/grupo_asir` solo para el grupo `grupo_asir`, probada desde `cli-linux` |
| Red de laboratorio | 192.168.60.0/24 solo-anfitrión sin DHCP (`vmbr1`): nodo 192.168.60.10, `srv-aso` 192.168.60.20 y `cli-linux` 192.168.60.30. Salida a Internet por el NAT de VirtualBox (`vmbr0`, 10.0.2.15) |
| Riesgos y dudas iniciales | Poco espacio libre en el PC para el disco virtual (por eso es dinámico y hay que vigilar su crecimiento). Las copias se guardan en el mismo disco que el laboratorio. La virtualización anidada hace las VMs más lentas. Los repositorios `enterprise` de Proxmox necesitan suscripción (resuelto con el repositorio No-Subscription) |

[Volver al indice](../README.md)
