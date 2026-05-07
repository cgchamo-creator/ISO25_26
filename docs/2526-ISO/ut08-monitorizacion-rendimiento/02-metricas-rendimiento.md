---
title: Métricas y Rendimiento del Sistema
sidebar_position: 2
description: Conceptos clave para medir la salud del hardware y detectar cuellos de botella en el rendimiento.
---

## ¿Qué debemos medir?

Monitorizar el rendimiento no consiste en mirar una gráfica de vez en cuando. Como administrador, debes conocer los **KPIs (Key Performance Indicators)** de tus servidores para identificar qué componente está limitando la velocidad del sistema. Los cuatro pilares son: **CPU**, **Memoria RAM**, **Disco** y **Red**.

## Unidad Central de Procesamiento (CPU)

La CPU es el cerebro, pero a menudo es el componente que más tiempo pasa "esperando".

*   **% de tiempo de procesador**: Indica cuánto tiempo está el procesador ejecutando hilos que no son procesos inactivos. Si este valor supera el 80% de forma sostenida, necesitas más potencia o revisar qué procesos consumen tantos recursos.
*   **Longitud de la cola del procesador**: Si hay muchos hilos esperando a ser procesados, aunque el uso de CPU no sea del 100%, el sistema se sentirá lento.
*   **Interrupciones por segundo**: Un valor muy alto puede indicar fallos en drivers o hardware defectuoso que "molesta" constantemente a la CPU.

## Memoria RAM

No te fíes solo del "espacio libre". Los sistemas modernos usan la RAM libre para caché, lo cual es bueno.

*   **Memoria disponible**: Es la RAM que puede ser asignada inmediatamente a un proceso.
*   **Páginas/seg**: Indica cuántas veces el SO ha tenido que usar el disco (archivo de paginación o swap) porque no había espacio en la RAM física. 
    *   *Si este valor es alto constantemente, necesitas ampliar la RAM.*

## Almacenamiento (Disco)

El disco suele ser el cuello de botella más común, especialmente si no usas unidades NVMe/SSD.

*   **% de tiempo de disco**: Tiempo que la unidad pasa atendiendo peticiones de lectura o escritura.
*   **Longitud media de la cola de disco**: Número de peticiones esperando a ser escritas o leídas. Si es mayor a 2 de forma constante, el disco está saturado.

:::info[Latencia vs. Ancho de Banda]
En discos, a veces importa más la **latencia** (cuánto tarda en empezar a leer) que la velocidad máxima de transferencia (MB/s). Un servidor de base de datos sufre más por latencia que por ancho de banda.
:::

## Herramientas de Medición Nativas

Dependiendo de tu sistema operativo, usarás herramientas distintas para obtener estos datos:

### En Windows Server
*   **Administrador de Tareas**: Visión rápida y visual.
*   **Monitor de Recursos**: Detalle por proceso de disco y red.
*   **Monitor de Rendimiento (PerfMon)**: La herramienta profesional para crear gráficas personalizadas y logs históricos.

### En Linux (Ubuntu/Debian)
*   **top / htop**: El estándar interactivo para ver procesos y CPU.
*   **vmstat**: Estadísticas de memoria virtual, procesos y CPU en una sola línea.
*   **iostat**: Detalle exhaustivo del rendimiento de los discos.

:::tip[¿Sabías que?]
En Linux, el concepto de **Load Average** (Carga media) te da una idea rápida del estado del sistema en los últimos 1, 5 y 15 minutos. Un valor de 1.0 en una CPU de 1 núcleo significa que el sistema está al 100% de su capacidad.
:::
