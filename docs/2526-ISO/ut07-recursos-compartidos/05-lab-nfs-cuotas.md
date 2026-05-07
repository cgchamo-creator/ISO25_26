---
title: "Lab: NFS y Cuotas de Disco"
sidebar_position: 5
description: Configuración de almacenamiento NFS en Linux e implementación de límites de espacio por usuario.
---

## Objetivo
Configurar un servidor NFS para compartir directorios entre sistemas Linux y aplicar límites de almacenamiento mediante cuotas de disco para evitar el agotamiento del espacio.

## Parte 1: Configuración del Servidor NFS

### 1. Instalación
En el servidor (Ubuntu):
```bash
sudo apt update
sudo apt install nfs-kernel-server -y
```

### 2. Exportar el directorio
Crearemos un directorio para compartir:
```bash
sudo mkdir -p /mnt/nfs_share
sudo chown nobody:nogroup /mnt/nfs_share
```

Editamos el archivo `/etc/exports`:
```bash
sudo nano /etc/exports
```

Añadimos la siguiente línea para permitir el acceso a toda nuestra red (ej. 192.168.1.0/24):
```text
/mnt/nfs_share  192.168.1.0/24(rw,sync,no_subtree_check)
```

### 3. Aplicar cambios
```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

## Parte 2: Implementación de Cuotas de Disco

### 1. Preparación del Sistema de Archivos
Las cuotas deben estar habilitadas en el montaje del disco. Editamos `/etc/fstab`:

```text
UUID=xxxx-xxxx / ext4 defaults,usrquota,grpquota 0 1
```

:::info[Remontaje]
Para aplicar los cambios en el sistema de archivos raíz sin reiniciar:
`sudo mount -o remount /`
:::

### 2. Creación de la base de datos de cuotas
```bash
sudo quotacheck -cum /
sudo quotaon -v /
```

### 3. Asignación de límites
Vamos a limitar al usuario `alumno1` a un máximo de 500MB de espacio:

```bash
sudo edquota -u alumno1
```

Se abrirá un editor de texto. Cambia los valores de **soft** y **hard** (en bloques de 1K):
*   **soft**: 450000 (aprox 450MB)
*   **hard**: 512000 (500MB)

## Parte 3: Verificación del Cliente

En otra máquina Linux (Cliente):
1. Instala el cliente: `sudo apt install nfs-common -y`
2. Monta el recurso: `sudo mount <IP_SERVIDOR>:/mnt/nfs_share /mnt`
3. Comprueba el espacio: `df -h`

:::tip[¿Cómo ver mis límites?]
Como usuario, puedes ver cuánto espacio te queda disponible ejecutando simplemente el comando:
`quota -s`
:::

## Reto de Administración
Configura el servidor NFS para que el recurso sea **solo lectura** (`ro`) para todos los equipos de la red, excepto para uno específico (la IP de tu cliente) que debe tener permisos de escritura (`rw`).
