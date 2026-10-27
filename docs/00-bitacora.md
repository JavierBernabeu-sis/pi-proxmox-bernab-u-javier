# Bitacora de sesiones

La bitacora es el registro cronologico del trabajo. Anade una entrada al final
de cada sesion y conserva las anteriores. Escribe datos comprobables y enlaza
las evidencias utiles; evita frases generales como "he trabajado en Proxmox".

## Sesion 01 - FECHA 22-09-2026

- Objetivo: iniciar GitHub y la documentacion Markdown.
- Acciones realizadas: Seguida guía de primera sesión de GitHub, script ejecutado y formación de la estructura. Se crea el repositorio individual con la estructura obligatoria del enunciado (README.md, .gitignore, docs/, scripts/, ansible/ y evidencias/).
- Decisiones y motivos: Se respetan los nombres y la estructura del enunciado sin modificarlos, porque son obligatorios y así el profesor encuentra cada documento donde lo espera. No se valoraron alternativas porque la estructura viene fijada.
- Pruebas y resultados: Resultado esperado: repositorio con la estructura del enunciado. Método: script de la guía de la primera sesión. Resultado obtenido: estructura creada.
- Documentos o evidencias: [Ficha inicial](00-ficha-inicial.md).
- Incidencias y solucion o estado pendiente: Ninguna anotada en esta sesión.
- Siguiente paso: Completar la ficha inicial y empezar la Fase 1 (inventario del equipo y viabilidad de Proxmox).

## Sesion 02 - 07-10-2026
- Objetivo: iniciar la Fase 1: inventario del equipo y viabilidad de Proxmox (docs/01-inventario-hardware.md).
- Acciones realizadas: Arranque del equipo utilizando Ubuntu 22.04 y primera exploración de los componentes: procesador Intel Pentium G4560, 8 GB de RAM y almacenamiento en disco SSD Crucial MX500 de 250GB. Redacción del primer borrador del documento Markdown con la tabla de recursos.
- Decisiones y motivos: Se decide hacer el análisis sobre la máquina física real y no en VirtualBox para obtener datos precisos de los componentes. Al contar con un SSD, se prioriza este disco para la instalación del hipervisor y las futuras máquinas para garantizar una lectura/escritura fluida.
- Pruebas y resultados: Resultado esperado: confirmar soporte de virtualización. Método: revisión de los flags de la CPU en la terminal. Resultado obtenido: se confirma soporte VT-x (flag vmx), pero se pospone la captura de las evidencias definitivas a la siguiente sesión.
- Documentos o evidencias: Borrador inicial de `01-inventario-hardware.md`.
- Incidencias y solucion o estado pendiente: Se redactó el borrador pero faltó redirigir y guardar las salidas de texto de los comandos (lscpu, lsblk, dmidecode, etc.) en archivos `.txt` como evidencias reales.
- Siguiente paso: Ejecutar de nuevo los comandos, guardar sus salidas en la carpeta de evidencias y redactar el dictamen final del hardware.

## Sesion 03 - 13-10-2026
- Objetivo: finalizar la Fase 1 completando el análisis y la documentación del inventario de hardware (docs/01-inventario-hardware.md) y registrar las evidencias.
- Acciones realizadas: Ejecución de comandos en el sistema anfitrión para extraer el inventario real (lscpu, dmidecode, lsblk, etc.). Guardado de las salidas en archivos de texto dentro de `evidencias/capturas-o-salidas/comandos/` (scrit.txt, almacenamiento.txt, memoriaram.txt, plataforma.txt, red.txt). Redacción mediante `nano` de las tablas de reparto de recursos, limitaciones, componentes críticos, fuentes y el dictamen de viabilidad. Finalmente, se ha realizado el commit y push al repositorio de GitHub.
- Decisiones y motivos: Se establece que la memoria RAM (8 GB) es el cuello de botella principal, limitando la asignación a 2 GB por máquina virtual (cliente y servidor) para evitar el uso de swap en el host. El almacenamiento pasa a ser el segundo componente crítico por falta de redundancia (un único disco SSD), obligando a planificar backups externos. Se pospone la redacción de la ficha inicial para consultar dudas con el profesor.
- Pruebas y resultados: Resultado esperado: contrastar los requisitos de Proxmox con el hardware real. Método: análisis de las salidas de los comandos. Resultado obtenido: se confirma compatibilidad (VT-x activo, SSD con espacio suficiente), pero se detecta que la interfaz de red (`eth0`) aparece caída (`link=no`), requiriendo conexión física de cable.
- Documentos o evidencias: Archivo `docs/01-inventario-hardware.md` completado y los 5 ficheros `.txt` de salidas guardados en la carpeta de evidencias.
- Incidencias y solucion o estado pendiente: Quedan dudas pendientes de resolver sobre cómo rellenar apartados específicos de la "Ficha inicial"; se deja en pausa hasta poder preguntarle al profesor. Falta conectar el equipo por cable para verificar la red a 1 Gbps.
- Siguiente paso: Consultar con el profesor las dudas de la ficha inicial para poder cerrarla y preparar la siguiente fase (instalación de Proxmox VE).

## Sesion 04 - 20-10-2026
- Objetivo: realizar la Fase 2 del proyecto, diseñando la red del laboratorio antes de instalar servicios y documentarla.
- Acciones realizadas: Revisión de la práctica y de la teoría de redes virtuales del proyecto. Diseño de la red del laboratorio con Proxmox virtualizado en VirtualBox: nodo `pve-bernabeu`, bridges `vmbr0` y `vmbr1`, direccionamiento de `srv-aso` y `cli-linux`, gateway, DNS, puertos y medidas de aislamiento. Redacción del documento docs/02-diagrama-red.md con la tabla de direccionamiento, el diagrama Mermaid y la justificación de aislamiento y permisos. Finalmente, se ha realizado el commit y push al repositorio de GitHub.
- Decisiones y motivos: Se usan dos bridges: `vmbr0` (adaptador NAT) para la salida a Internet y `vmbr1` (adaptador solo-anfitrión) para la gestión y el laboratorio, de modo que los invitados quedan separados de la red del aula y solo salen a través del nodo. Se cambian las IPs del ejemplo de la teoría por las indicadas por el profesor (red de laboratorio 192.168.60.0/24). No se usan VLAN porque no son obligatorias y no se debe modificar la red del centro. Como DNS se usa el de Google (8.8.8.8). Solo se documenta el puerto del panel web de Proxmox (8006/TCP), porque los puertos del servicio ASO aún no están definidos.
- Pruebas y resultados: Esta fase es de diseño y no se han ejecutado pruebas todavía. Las comprobaciones de red (rutas, DNS, puertos abiertos) quedan para las fases de instalación y configuración del nodo.
- Documentos o evidencias: Archivo `docs/02-diagrama-red.md` completado con el diagrama de red, la tabla de direccionamiento y la justificación de aislamiento y permisos.
- Incidencias y solucion o estado pendiente: Sin incidencias. Queda pendiente confirmar con el profesor el DNS elegido y definir los puertos del servicio ASO cuando se decida el servicio.
- Siguiente paso: Pasar a la Fase 3 (instalación de Proxmox VE en VirtualBox) siguiendo el diseño de red documentado.

## Sesion 05 - 27-10-2026
- Objetivo: realizar la Fase 3 del proyecto, instalando Proxmox VE en una máquina virtual de VirtualBox y dejándolo accesible desde el equipo anfitrión.
- Acciones realizadas: Revisión del PC de prácticas (VirtualBox 7.2.4, 15,4 GiB de RAM, unos 287 GiB libres). Creación de la red solo-anfitrión 192.168.60.1/24 sin DHCP. Creación de la VM `pve-bernabeu` (Debian 64 bits, 2 vCPU, 8 GiB de RAM, VDI dinámico de 80 GiB en SATA, adaptador NAT y adaptador solo-anfitrión). Instalación de Proxmox VE 9.2 con hostname `pve-bernabeu.lab.test` e IP 10.0.2.15/24. Configuración del bridge `vmbr1` (192.168.60.10/24) en `/etc/network/interfaces`, ajuste de `/etc/hosts` y acceso al panel web desde el navegador del anfitrión. Redacción de docs/03-instalacion-proxmox.md y subida de las evidencias a GitHub.
- Decisiones y motivos: Se asignan 8 GiB de RAM a la VM porque el PC de prácticas tiene 16 GB y así el anfitrión conserva unos 7 GiB. Se dejan 2 vCPU, porque las vCPU de Proxmox se pueden sobreasignar y la carga del laboratorio es baja. El disco se coloca en un controlador SATA en lugar de IDE por rendimiento. Se usa ext4 en vez de ZFS para no consumir RAM extra. Como DNS se usa 10.0.2.3 (DNS del NAT de VirtualBox), recomendado por la guía. Las interfaces del nodo quedan con los nombres `nic0` y `nic1` por la opción "Pin network interface names" del instalador.
- Pruebas y resultados: Resultado esperado: nodo instalado y administrable desde el anfitrión. Método: comandos `hostnamectl`, `ip -br addr`, `ip route`, `pveversion` y `df -h` en el nodo, y acceso a `https://192.168.60.10:8006`. Resultado obtenido: Proxmox VE 9.2.2 (kernel 7.0.2-6-pve) sobre Debian 13, `vmbr0` y `vmbr1` activos con sus IPs, ruta por defecto por 10.0.2.2 y panel web accesible.
- Documentos o evidencias: [Instalación de Proxmox](03-instalacion-proxmox.md) y capturas `s05-panel-proxmox.png`, `s05-comandos-nodo.png` y `s05-interfaces.png`.
- Incidencias y solucion o estado pendiente: El instalador se quedaba en pantalla negra; el log de VirtualBox mostró que la VM estaba creada como de 32 bits ("Other"), y se solucionó cambiando el tipo a Debian (64-bit). Proxmox avisó de que no detectaba KVM porque la virtualización anidada estaba desactivada; queda pendiente activarla y comprobarla con `grep -c vmx /proc/cpuinfo`. También queda pendiente actualizar en docs/02-diagrama-red.md los nombres de interfaz (`nic0`/`nic1`) y el DNS (10.0.2.3).
- Siguiente paso: Activar la virtualización anidada, configurar la salida a Internet de los invitados (reenvío y NAT) y pasar a la Fase 4 (configuración base del nodo).


Anadir una entrada con esta estructura en cada sesion. El historial de
GitHub registra los commits; no hace falta incluir el hash del propio
commit dentro de su entrada de bitacora.

[Volver al indice](../README.md)
