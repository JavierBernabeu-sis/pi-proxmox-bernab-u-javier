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

## Sesion 06 - 10-11-2026
- Objetivo: reinstalar el nodo Proxmox, porque la VM `pve-bernabeu` se había borrado, aplicando los cambios indicados por el profesor.
- Acciones realizadas: Creación de nuevo de la VM `pve-bernabeu` en VirtualBox (Debian 64 bits, 3 vCPU, 8 GiB de RAM, VDI de 80 GiB en SATA, adaptador 1 NAT y adaptador 2 solo-anfitrión con modo promiscuo "Permitir todo"). Activación de la virtualización anidada (*Nested VT-x/AMD-V*) y de la paravirtualización KVM con *Nested Paging*. Instalación de Proxmox VE 9.2 sin configurar la red en el instalador. Configuración a mano de `/etc/network/interfaces` con `vmbr0` (10.0.2.15/24, gw 10.0.2.2, sobre `nic0`) y `vmbr1` (192.168.60.10/24, sobre `nic1`), aplicada con `ifreload -a`. Corrección de `/etc/hosts` para que `pve-bernabeu.lab.test` apunte a 192.168.60.10. Acceso al panel web en `https://192.168.60.10:8006`.
- Decisiones y motivos: Se suben las vCPU de 2 a 3 para tener más fluidez y mejor rendimiento, ya que el nodo ejecutará virtualización anidada. El profesor pidió reservar el disco completo (fijo), pero no había espacio suficiente en el anfitrión, así que se deja dinámico. La red no se configura en el instalador por indicación del profesor y se hace después desde la consola del nodo. Se mantiene el adaptador 1 en NAT, como en la guía del profesor, para no conectar el laboratorio a la red del aula.
- Pruebas y resultados: Resultado esperado: nodo reinstalado y administrable desde el anfitrión. Método: `ip -br addr`, `ip route`, `hostname -f` y acceso al panel desde el navegador. Resultado obtenido: `vmbr0` y `vmbr1` UP con sus IPs, ruta por defecto por 10.0.2.2, nombre del nodo resolviendo a 192.168.60.10 y panel accesible con `root`.
- Documentos o evidencias: Los cambios de la reinstalación se documentan en el apartado "Fase 4" de [Instalación de Proxmox](03-instalacion-proxmox.md).
- Incidencias y solucion o estado pendiente: La VM `pve-bernabeu` se había borrado; se reinstaló desde cero. Al crear el disco fijo de 80 GB, VirtualBox devolvió `VERR_DISK_FULL` (`VBOX_E_FILE_ERROR`) por falta de espacio en `/home/javbermar2`; se solucionó creando el disco como dinámico. El instalador dejó en `/etc/hosts` una IP de la red del aula (`10.112.7.107`), que se corrigió a mano.
- Siguiente paso: Pasar a la Fase 4 (configuración base del nodo): revisar almacenamiento, red, accesos y reparto de recursos.

## Sesion 07 - 17-11-2026
- Objetivo: realizar la Fase 4 del proyecto, comprobando que el nodo tiene una base estable antes de crear las máquinas, y documentarla en docs/03-instalacion-proxmox.md.
- Acciones realizadas: Revisión del almacenamiento en *Datacenter → Storage* y con `pvesm status` y `df -h`, y del espacio del anfitrión con `df -h` y `du -sh`. Revisión de la red en *Nodo → System → Network*, con `ip -br addr` e `ip route`, relacionando cada interfaz con su adaptador de VirtualBox por la MAC. Comprobación de la conectividad con `getent hosts`, `curl` y `apt update`. Comprobación de la virtualización anidada con `grep -Ec '(vmx|svm)' /proc/cpuinfo`. Revisión de las cuentas en *Permissions → Users*. Redacción de la tabla de reparto de recursos. Documentación en docs/03-instalacion-proxmox.md y subida de 17 capturas a GitHub.
- Decisiones y motivos: Las copias irán al almacenamiento `local`, porque admite *Backup*, mientras que `local-lvm` solo admite discos de VM y contenedores. No se crean usuarios ni roles adicionales ni se activa el firewall de Proxmox, porque el profesor no lo ha pedido y no es obligatorio. Reparto previsto: nodo 2 GiB, `srv-aso` 2 vCPU y 2 GiB, `cli-linux` 1 vCPU y 1 GiB, dejando 3 GiB de margen.
- Pruebas y resultados: Resultado esperado: panel accesible, conectividad prevista, almacenamientos disponibles y reparto con margen. Método: comandos y capturas indicados arriba. Resultado obtenido: `local` (23,6 GiB libres) y `local-lvm` (29,9 GiB libres) activos; red igual que el diseño de la fase 2; DNS resuelve y `curl` a `deb.debian.org` devuelve `HTTP/2 200`; `grep` devuelve 6, así que Proxmox ve la virtualización anidada; anfitrión con 69 GB libres.
- Documentos o evidencias: [Instalación de Proxmox, apartado Fase 4](03-instalacion-proxmox.md) y capturas `s07-*.png` en la carpeta de evidencias.
- Incidencias y solucion o estado pendiente: El `ping` está bloqueado por el filtrado del centro; la conectividad se comprobó con `getent hosts` y `curl`. `apt update` da `401 Unauthorized` en los repositorios `enterprise` de Proxmox por no tener suscripción; queda pendiente consultar con el profesor si se sustituyen por el repositorio No-Subscription. Queda pendiente probar el arranque de una VM KVM en la fase 6.
- Siguiente paso: Pasar a la Fase 5 (plantilla Linux con Ubuntu Server).

## Sesion 08 - 24-11-2026
- Objetivo: corregir los puntos indicados en la revisión del profesor del 19-11-2026 (nota 7,3): mezcla de redes en el diseño, errores `401` en los repositorios y cálculo del espacio para la plantilla y las dos máquinas.
- Acciones realizadas: Cambio en docs/02-diagrama-red.md de las direcciones 192.168.56.x a 192.168.60.x, y actualización de los nombres de interfaz (`nic0` y `nic1`), el DNS (10.0.2.3) y los recursos de cada máquina. Desactivación de los repositorios `enterprise` de `pve` y `ceph` y alta del repositorio `No-Subscription` desde *Actualizaciones → Repositorios*. Revisión del espacio con `pvesm status` y `lvs` en el nodo, y con `df -h` y `du -sh` en el anfitrión. Redacción del apartado "Correcciones" al final de docs/03-instalacion-proxmox.md y subida de 5 capturas `s08-*.png`.
- Decisiones y motivos: Se usa el repositorio `No-Subscription` porque el nodo no tiene suscripción y es un laboratorio. No se instalan las 224 actualizaciones pendientes para no cambiar el kernel sin necesidad. Se reducen los discos a 8 GiB (plantilla), 12 GiB (`srv-aso`) y 8 GiB (`cli-linux`) para que los tres quepan en los 29,93 GiB de `local-lvm` aunque se llenen.
- Pruebas y resultados: Resultado esperado: `apt update` sin errores y reparto de disco que quepa. Método: `cat /etc/apt/sources.list.d/*.sources`, `apt update`, `pvesm status`, `lvs`, `df -h` y `du -sh`. Resultado obtenido: los repos `enterprise` quedan con `Enabled: false` y `apt update` termina sin errores. Los tres discos suman 28 GiB de 29,93 GiB. El anfitrión tiene 65 GiB libres y la VM ocupa 4,5 GiB.
- Documentos o evidencias: [Correcciones en Instalación de Proxmox](03-instalacion-proxmox.md), [Diagrama de red](02-diagrama-red.md) y capturas `s08-apt-401-antes.png`, `s08-repositorios-panel.png`, `s08-apt-update-ok.png`, `s08-pvesm-lvs.png` y `s08-espacio-anfitrion.png`.
- Incidencias y solucion o estado pendiente: En el peor caso el VDI dinámico (máximo 80 GiB) no cabría en los 65 GiB libres del anfitrión. Se deja como limitación con una medida: revisar `df -h` antes de crear máquinas o copias y parar si quedan menos de 10 GiB libres. Queda pendiente completar el inicio de la memoria técnica.
- Siguiente paso: Completar el inicio de docs/10-memoria-tecnica.md (resumen, necesidad y objetivos, alcance) y pasar a la Fase 5 (plantilla Linux con Ubuntu Server).


Anadir una entrada con esta estructura en cada sesion. El historial de
GitHub registra los commits; no hace falta incluir el hash del propio
commit dentro de su entrada de bitacora.

[Volver al indice](../README.md)
