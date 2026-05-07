---
title: "Tutorial: Recuperación en Windows Server"
sidebar_position: 4
description: Uso de Copias de Seguridad de Windows Server y restauración mediante Shadow Copies.
---

## Prerrequisitos

1.  Un servidor con **Windows Server 2019/2022**.
2.  Un segundo disco virtual añadido al servidor (mínimo 20GB) para almacenar las copias.
3.  La característica **Copia de seguridad de Windows Server** instalada.

## Paso 1: Instalación de la Herramienta

Windows Server no trae la herramienta de backup activa por defecto.
1.  Abre el **Administrador del servidor**.
2.  Haz clic en **Administrar** -> **Agregar roles y características**.
3.  Avanza hasta **Características** y marca **Copia de seguridad de Windows Server**.
4.  Instala y reinicia si es necesario.

## Paso 2: Programar una Copia de Seguridad

1.  Abre la herramienta desde Herramientas -> **Copia de seguridad de Windows Server**.
2.  En el panel derecho, selecciona **Programación de copia de seguridad...**.
3.  Elige **Servidor completo** (recomendado para poder hacer bare-metal recovery).
4.  Especifica el tiempo (ej: una vez al día a las 21:00).
5.  **Destino**: Elige el disco dedicado que añadimos en los prerrequisitos.

:::warning[¡Cuidado!]
Al elegir un disco dedicado para backups, Windows formateará el disco y no será visible en el Explorador de Archivos. Esto es una medida de seguridad para evitar que el Ransomware borre las copias fácilmente.
:::

## Paso 3: Restauración Instantánea (Shadow Copies)

Las Copias de Sombra permiten a los usuarios recuperar versiones anteriores de archivos sin que el administrador tenga que intervenir.

1.  En el Explorador de Archivos, haz clic derecho sobre un disco (ej: `D:`) -> **Configurar copias de sombra...**.
2.  Selecciona el volumen y haz clic en **Habilitar**.
3.  Configuración: Puedes programar que se haga una "foto" cada mañana a las 07:00.

### Simulación de Recuperación:
1.  Crea un archivo de texto, escribe algo y guárdalo.
2.  Haz clic derecho sobre el archivo -> **Propiedades** -> **Versiones anteriores**.
3.  Modifica el archivo y guárdalo de nuevo.
4.  Vuelve a versiones anteriores y verás que puedes **Restaurar** el archivo al estado original.

## Paso 4: Recuperación ante Desastres (Bare Metal)

Si el servidor no arranca, el procedimiento es:
1.  Arrancar con el ISO de instalación de Windows Server.
2.  Seleccionar **Reparar el equipo**.
3.  Elegir **Solucionar problemas** -> **Recuperación de imagen del sistema**.
4.  El instalador buscará automáticamente en los discos conectados la última copia realizada con la herramienta de Windows Server Backup.

:::info[¿Qué es Bare Metal?]
Significa "Metal Desnudo". Es la capacidad de restaurar todo el sistema operativo, drivers y datos en un hardware nuevo totalmente vacío.
:::

---

## Tarea Final del Módulo

Para dar por concluido el módulo de ISO, realiza esta última práctica integradora:
1.  Configura una **Shadow Copy** en tu disco de datos de Windows.
2.  Crea un script de **rsync** en Linux que haga una copia de tu carpeta `/etc` hacia un directorio de backup.
3.  Simula un borrado en ambos sistemas y documenta el proceso de restauración exitoso.

¡Enhorabuena! Has completado el temario de **Implantación de Sistemas Operativos**.
