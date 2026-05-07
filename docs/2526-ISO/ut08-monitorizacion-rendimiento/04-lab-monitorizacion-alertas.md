---
title: "Tutorial: Monitorización y Alertas Automáticas"
sidebar_position: 4
description: Configuración de contadores de rendimiento y automatización de avisos en Windows y Linux.
---

## Introducción

En este laboratorio aprenderás a configurar **contadores de rendimiento** que activen acciones automáticas cuando se superen ciertos umbrales de seguridad. Esto es vital para prevenir caídas por falta de espacio en disco o saturación de CPU.

## Parte 1: Alertas en Windows Server (Data Collector Sets)

Windows permite crear "Recopiladores de datos" que vigilan el sistema en segundo plano.

1.  Abre el **Monitor de Rendimiento** (`perfmon.exe`).
2.  Despliega **Conjuntos de recopiladores de datos** -> **Definido por el usuario**.
3.  Haz clic derecho -> **Nuevo** -> **Conjunto de recopiladores de datos**.
4.  Nombre: `Alerta_CPU_Alta`. Selecciona **Crear manualmente (avanzado)**.
5.  Elige **Alerta de contador de rendimiento**.
6.  Añade el contador: `Procesador` -> `% de tiempo de procesador` -> `_Total`.
7.  Configura la alerta: **Alertar cuando sea superior a: 90**.
8.  En la pestaña **Acción de alerta**, puedes configurar que se ejecute un script o se cree una entrada en el log de eventos.

:::tip[Aplicación Profesional]
Puedes configurar que, si la CPU supera el 90%, se ejecute un script de PowerShell que reinicie un servicio específico o limpie archivos temporales.
:::

## Parte 2: Alertas en Linux con Bash Scripting

En Linux, a menudo usamos pequeños scripts que se ejecutan periódicamente mediante **Cron**. Vamos a crear un monitor de espacio en disco.

### Script de Monitorización de Disco
Crea un archivo llamado `monitor_disco.sh`:

```bash title="scripts/monitor_disco.sh"
#!/bin/bash

# Umbral de alerta (en porcentaje)
THRESHOLD=80
# Correo del administrador (simulado)
ADMIN_EMAIL="admin@agora.local"

# Obtener el porcentaje de uso de la partición raíz (/)
USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

if [ $USAGE -gt $THRESHOLD ]; then
    echo "¡ALERTA! El uso de disco en / es del $USAGE%."
    # Aquí podrías enviar un correo real con 'mail' o un log
    logger -p local0.crit "Alerta de sistema: Disco superando el $THRESHOLD% ($USAGE%)"
fi
```

### Automatización del Script
Para que este script se ejecute cada 5 minutos, lo añadimos al crontab:

```bash title="Terminal Linux"
# Editar el crontab del usuario root
sudo crontab -e

# Añadir esta línea al final del archivo
*/5 * * * * /ruta/al/script/monitor_disco.sh
```

## Parte 3: Análisis de Rendimiento en Tiempo Real (htop)

Aunque las alertas son automáticas, a veces necesitas ver qué está pasando *ahora*.

1.  Instala htop: `sudo apt install htop`.
2.  Ejecuta `htop`.
3.  **Filtros rápidos**:
    *   `F6`: Ordenar por CPU, Memoria o Prioridad.
    *   `F5`: Ver procesos en árbol (para ver qué proceso "padre" ha lanzado a los "hijos").
    *   `F9`: Matar (kill) un proceso que esté bloqueando el sistema.

:::info[¿Por qué htop?]
A diferencia del `top` clásico, `htop` permite usar el ratón, tiene colores que indican el uso de caché/buffer y facilita enormemente la gestión de procesos sin tener que memorizar IDs.
:::

---

## Tarea Final de Unidad

Crea un informe (puedes usar Markdown) donde documentes:
1.  Una captura de pantalla de tu **Vista Personalizada** en Windows con al menos un error detectado.
2.  La salida del comando `journalctl -p 3` de tu máquina Linux.
3.  El funcionamiento de tu script de Bash tras llenar artificialmente el disco (puedes usar `fallocate -l 1G archivo_basura` para simular llenado).
