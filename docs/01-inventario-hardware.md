# Inventario investigado y viabilidad

## Contexto de la observacion

- Fecha y sistema utilizado: 13/10/2026, Ubuntu 22.04.
- Modalidad, equipo anfitrion y VM VirtualBox si procede: .
- Version de Proxmox prevista: Proxmox VE 9.2

## Inventario inicial

| Area | Dato observado y evidencia | Especificacion y fuente | Limite o diferencia | Justificacion |
|---|---|---|---|---|
| CPU | Intel Pentium G4560 @ 3.50GHz, 2 nucleos / 4 hilos, 1 socket; flag `vmx` y `Virtualizacion: VT-x` presentes (lscpu -> evidencias/cpu.txt) | Kaby Lake, 2C/4T, 3.50 GHz, cache 3 MB, TDP 54 W, VT-x + VT-d + EPT, 2 canales de memoria (Intel ARK, ficha G4560) | CPU sin turbo y de gama baja; 4 hilos es el tope para repartir vCPU. Soporta VT-d, no imprescindible aqui | Cumple el requisito de virtualizacion por hardware de Proxmox (VT-x + EPT). Con 4 hilos se puede repartir vCPU entre Proxmox, srv-aso y cli-linux, aunque sin margen para cargas pesadas |
| RAM | 8 GB totales (7,6 GiB), 2x4 GB DDR4 Crucial a 2400 MT/s en ChannelA-DIMM1 y ChannelB-DIMM1 (doble canal); 2 slots libres; ~5,7 GiB disponibles (dmidecode + free -> evidencias/ram.txt) | Modulos Crucial CT4G4DFS824A.C8FBD2, DDR4-2400 UDIMM 1,2 V. Placa: max. 64 GB en 4 slots. CPU: max. 64 GB, DDR4-2133/2400 (dmidecode type 16; Intel ARK G4560) | Solo 8 GB instalados frente a 64 GB admitidos; quedan 2 slots libres, ampliable | Es el recurso mas ajustado. 8 GB limita cuantas maquinas pueden estar encendidas a la vez; hay margen de ampliacion fisica sin cambiar placa |
| Almacenamiento | SSD SATA Crucial MX500 250 GB (`sda`, ROTA=0), particion unica en `/`: 229 GB, 41 GB usados, 177 GB libres (19%); ademas USB Kingston 231 GB y lector optico `sr0` (lsblk + df -> evidencias/disco.txt) | Crucial MX500 CT250MX500SSD1: SATA III 6 Gb/s, TLC 3D NAND, lectura 560 MB/s, escritura 510 MB/s, resistencia 100 TBW (ficha Crucial MX500) | 177 GB libres para Proxmox + VM + ISOs + backups; un unico disco, sin redundancia | SSD aporta buen IOPS para varias VM. El espacio libre es suficiente pero justo si acumulas ISOs y copias; al ser un solo disco, no hay tolerancia a fallo fisico |
| Red | Realtek RTL8111/8168/8411 PCIe Gigabit, `eth0`, driver `r8169`, firmware rtl8168h-2, capacidad 1 Gbit/s; `link=no` / eth0 DOWN (sin cable) (lshw + ip -> evidencias/red.txt) | Controladora Realtek Gigabit Ethernet: 10/100/1000 Mb/s, autonegociacion, par trenzado (lshw; Realtek RTL8111H) | Tarjeta de 1 Gb por cable, pero en la captura el enlace estaba caido; interfaces virtuales de Docker/libvirt activas (docker0, virbr0, br-...) | Ethernet por cable a 1 Gb es lo idoneo para el bridge de Proxmox. Hay que conectar el cable: con `link=no` no hay conectividad real. Conviene vigilar el direccionamiento de las redes virtuales existentes para no solaparlo |
| Plataforma | Equipo PC613; BIOS American Megatrends (AMI) version 0810, revision 5.12, fecha 09/08/2017 (dmidecode -t bios -> evidencias/plataforma.txt) | BIOS AMI; placa con chipset compatible Kaby Lake (socket LGA1151), soporte VT-x activable en firmware | BIOS de 2017 sin actualizar; conviene verificar en el firmware que VT-x esta habilitado y el modo de arranque (UEFI/Legacy) | VT-x aparece activo en `lscpu`, luego la virtualizacion esta habilitada en BIOS. La antiguedad del firmware no impide instalar Proxmox, pero es un dato a registrar para la defensa |

## 1. Puedo instalar Proxmox en este equipo?

Contrastar el inventario con requisitos oficiales de la version prevista.
Responder por separado y justificar con datos y fuentes:

- Puedo instalar y arrancar Proxmox: Sí Proxmox exige CPU Intel 64 o AMD64 con el flag Intel VT o AMD-V, un mínimo de 2 GB de RAM para el sistema,
los servicios de Proxmox más la memoria de los invitados, y una NIC (fuente: proxmox.com, System Requirements). El Pentium G4560 es de 64 bits y tiene VT-x y EPT
( el comando lscpu muestra el flag vmx) la RAM es de 8 GB, el SSD de 250 GB y la NIC Realtek de 1 Gbit. Cumple los requisitos mínimos con bastante margen.

- Puedo ejecutar el servidor ASO y el cliente del proyecto: SÍ, con carga ligera. Con 8 GB de RAM y 4 hilos se pueden encender a la vez Proxmox
(2 GB), srv-aso (2 GB) y cli-linux (2 GB), y aún quedan unos 2 GB de margen. No hay recursos para una tercera VM pesada ni para cargas intensivas.

- Condiciones o comprobaciones pendientes: COMPROBADO con las salidas: la CPU tiene VT-x y el flag vmx aparece (lscpu -> evidencias/cpu.txt), 
tengo 2 módulos de 4 GB DDR4 a 2400 MT/s en doble canal y me quedan 2 slots libres (dmidecode y free -> evidencias/ram.txt), 
el disco es un SSD SATA Crucial MX500 y tengo 177 GB libres de los 229 GB de la partición (lsblk y df -> evidencias/disco.txt), 
la tarjeta de red es una Realtek Gigabit y el driver r8169 la reconoce bien (lshw -> evidencias/red.txt),
y la BIOS es una AMI versión 0810 de 2017 (dmidecode -> evidencias/plataforma.txt).
PENDIENTE: conectar el cable de red y ver si el enlace llega a 1 Gbit/s, mirar si arranco en UEFI o en Legacy,revisar el estado SMART del disco, 
que el instalador de Proxmox reconozca la tarjeta de red, resolver qué hacer con los 317 GiB de datos actuales del disco antes de formatearlo, y medir el rendimiento real con los dos invitados encendidos (sesión 5).

## 2. Que limitaciones tendre con este hardware?

Analizar RAM, CPU, disco y red para las cargas previstas. Identificar lo
medido y lo estimado. En VirtualBox, investigar host real y VM: version,
RAM reservada al anfitrion, vCPU, VDI, espacio fisico y red local.
No se utiliza el Proxmox del centro como recurso.

### Reparto de recursos

| Elemento | CPU/vCPU | RAM | Disco | Justificacion |
|---|---|---|---|---|
| Proxmox | 2 vCPU (uso base) | 2 GB | 40 GB | Sistema operativo anfitrión y servicios de virtualización (KVM/LXC). Necesita base para moverse con soltura. |
| srv-aso | 2 vCPU | 2 GB | 20 GB | Servidor Linux para la asignatura. 2 vCPU para buen rendimiento sin asfixiar al host. |
| cli-linux | 2 vCPU | 2 GB | 20 GB | Cliente Linux. Con 2 GB y 2 hilos es suficiente para entorno gráfico ligero o entorno de consola. |
| Plantilla, ISOs y copias | No aplica | No aplica | 40 GB | Espacio reservado en local para almacenar imágenes ISO de instalación y backups rápidos de las VM. |
| Margen de operacion | Oversubscription leve (6 vCPU total sobre 4 hilos) | 2 GB | ~109 GB libres | Queda margen para la caché del sistema de ficheros del host, picos de carga y más de 100 GB libres en el SSD. |

Mostrar calculos de RAM y disco, reparto de vCPU, unidades y supuestos.
Explicar las cargas que pueden funcionar simultaneamente.

| Limitacion | Evidencia o estimacion | Consecuencia | Medida y limite que permanece |
|---|---|---|---|
| Memoria RAM justa | 8 GB instalados (2x4GB), quedando ~5.7 GiB libres en host inactivo (evidencias/ram.txt). | No se pueden ejecutar más de 2 o 3 máquinas virtuales simultáneamente con fluidez. | Asignar máximo 2 GB por VM. Límite: No se podrán montar topologías grandes. |
| Disco sin redundancia | 1 solo disco SATA SSD de 250GB (`sda`) (evidencias/disco.txt). | Al no existir RAID, un fallo físico en el SSD supone la caída del sistema y pérdida de todas las VM. | Configurar copias de seguridad obligatorias hacia almacenamiento externo. Límite: Tolerancia a fallos de disco nula. |
| Red física única | 1 sola interfaz física `eth0` a 1Gbps (evidencias/red.txt). | El tráfico de gestión de Proxmox y el de los servicios de las VM comparten un mismo enlace sin respaldo. | Segmentar lógicamente con VLANs si fuera necesario. Límite: Cuello de botella de 1 Gbps y sin alta disponibilidad de red. |

## 3. Que componentes son mas criticos en mi servidor y por que?

Ordenar al menos tres componentes para este equipo y su servicio.
Distinguir cuello de botella, disponibilidad e integridad de los datos.

| Prioridad y componente | Funcion | Efecto si es insuficiente | Efecto si falla | Evidencia y justificacion |
|---|---|---|---|---|
| 1. Memoria RAM | Almacenar en tiempo real los procesos del host y de los sistemas invitados. | El host usará memoria de intercambio (swap), hundiendo dramáticamente el rendimiento del servidor. | Caída del sistema operativo o corrupción de memoria en las máquinas virtuales. | 8 GB en total. Es el recurso (cuello de botella) que antes se agotará al desplegar escenarios de red. |
| 2. Disco SSD (`sda`) | Almacenamiento persistente del hipervisor, archivos de configuración y discos virtuales. | Alta latencia de E/S, haciendo que las máquinas virtuales respondan con lentitud. | Pérdida total de datos, configuraciones y servicios. Nodo inoperativo. | Único Crucial MX500 de 250GB sin configuración RAID. Es un punto único de fallo crítico. |
| 3. Tarjeta de Red (`eth0`) | Proporcionar conectividad de gestión con Proxmox y salida al exterior para las VM. | Saturación del enlace si coinciden transferencias masivas (ej. backups externos) con peticiones web. | Pérdida de administración remota y aislamiento total del servidor. | Interfaz única Realtek a 1 Gbps. Si el cable o la tarjeta fallan, el servidor queda incomunicado. |

Componente que mejoraria primero y motivo para priorizarlo frente a otro:
La memoria RAM. Al comprobar con `dmidecode` que la placa tiene 4 slots y solo 2 ocupados, añadiría otros 8 GB (2 módulos de 4 GB adicionales) para alcanzar los 16 GB.

## Fuentes tecnicas

Consultar al menos tres fuentes primarias, incluyendo Proxmox y
fabricante o plataforma virtual. Relacionarlas con las tres respuestas.

| Fuente y URL | Version/modelo y fecha de consulta | Afirmacion respaldada | Aplicacion al nodo |
|---|---|---|---|
| Documentación Proxmox (proxmox.com/en/proxmox-virtual-environment/requirements) | Proxmox VE 8/9.x (Octubre 2026) | Requisitos mínimos de CPU de 64 bits con VT-x y mínimo 2 GB de RAM. | Confirma la viabilidad de instalar el hipervisor en el Pentium G4560. |
| Intel ARK (ark.intel.com) | Intel Pentium G4560 (Octubre 2026) | Soporte completo para virtualización por hardware (Intel VT-x y EPT). | Asegura que el procesador podrá manejar las extensiones KVM de manera nativa. |
| Fichas técnicas Crucial (crucial.es) | SSD Crucial MX500 SATA 2.5" (Octubre 2026) | Rendimiento del SSD con memorias TLC 3D NAND (lectura/escritura >500MB/s). | Valida que el SSD tiene IOPS suficientes para soportar varias máquinas a la vez. |

## Dictamen de viabilidad

Apto, apto con condiciones o no apto para la propuesta actual:

El equipo cumple con los requisitos de hardware necesarios para ejecutar Proxmox VE,
soportado principalmente por la tecnología VT-x del procesador Intel Pentium y un disco
SSD que agilizará las operaciones de entrada/salida. Sin embargo, su uso está restringido a un entorno de
laboratorio moderado debido al límite de 8 GB de memoria RAM, lo que obliga a medir muy bien los recursos asignados
al servidor y cliente del proyecto. Además, al contar con un solo disco físico,
existe un punto único de fallo: la integridad y disponibilidad del sistema dependerán por completo de realizar y
mantener copias de seguridad rigurosas en soportes externos.

Sintetizar las tres respuestas y las condiciones pendientes de comprobar.

## Evidencias y revision docente
Enlazar salidas revisadas desde ../evidencias/. Un listado automático no sustituye la investigación. Registrar la pregunta de defensa y respuesta.

A continuación se detallan y enlazan las salidas originales de los comandos ejecutados para realizar este inventario:

* [Inventario general del sistema y CPU (lscpu, etc.)](../evidencias/capturas-o-salidas/comandos/scrit.txt)
* [Información de almacenamiento y discos (lsblk)](../evidencias/capturas-o-salidas/comandos/almacenamiento.txt)
* [Detalles de la memoria RAM instalada (dmidecode)](../evidencias/capturas-o-salidas/comandos/memoriaram.txt)
* [Información de la plataforma y BIOS](../evidencias/capturas-o-salidas/comandos/plataforma.txt)
* [Detalles de la interfaz de red física](../evidencias/capturas-o-salidas/comandos/red.txt)

**Pregunta de defensa:** ¿Por qué consideras que la memoria RAM es el principal cuello de botella de este nodo y no el procesador (CPU)?
**Respuesta:** Porque el entorno base (host + servicios de Proxmox) ya reserva aproximadamente 2 GB. Al tener 8 GB totales, tras encender dos máquinas virtuales de 2 GB cada una (cliente y servidor), el host se queda sin apenas memoria disponible, impidiendo levantar más entornos de red sin recurrir a la memoria Swap (lo que hundiría el rendimiento). La CPU, en cambio, aunque es modesta (2 núcleos / 4 hilos), soporta sobreasignación (oversubscription) y puede repartir el tiempo de procesamiento entre las máquinas sin bloquearse, siempre que no ejecuten tareas pesadas.

[Volver al indice](../README.md)
