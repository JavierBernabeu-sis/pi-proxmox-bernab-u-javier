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


Anadir una entrada con esta estructura en cada sesion. El historial de
GitHub registra los commits; no hace falta incluir el hash del propio
commit dentro de su entrada de bitacora.

[Volver al indice](../README.md)
