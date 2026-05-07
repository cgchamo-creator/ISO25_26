---
title: "Lab: Servidor Samba Híbrido"
sidebar_position: 4
description: Configuración paso a paso de un servidor Samba en Linux accesible desde clientes Windows.
---

## Objetivo
Configurar un servidor Linux (Ubuntu/Debian) para compartir carpetas en una red local, permitiendo el acceso tanto a usuarios invitados (lectura) como a usuarios registrados (escritura).

## Paso 1: Instalación de Samba

Primero, actualizamos los repositorios e instalamos el paquete necesario:

```bash
sudo apt update
sudo apt install samba -y
```

Verificamos que el servicio esté corriendo:
```bash
systemctl status smbd
```

## Paso 2: Preparación del Directorio

Crearemos una carpeta que servirá como recurso compartido:

```bash
sudo mkdir -p /srv/samba/compartido
sudo chown -R nobody:nogroup /srv/samba/compartido
sudo chmod -R 777 /srv/samba/compartido
```

:::warning[Permisos 777]
En un entorno de producción real, nunca usarías `777`. Aquí lo usamos para simplificar el laboratorio y asegurar que Samba tiene acceso total antes de aplicar las restricciones del protocolo.
:::

## Paso 3: Configuración de Samba

El archivo de configuración principal es `/etc/samba/smb.conf`. Antes de editarlo, haremos una copia de seguridad:

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo nano /etc/samba/smb.conf
```

Añade lo siguiente al final del archivo:

```ini
[Publico]
   comment = Recurso compartido publico
   path = /srv/samba/compartido
   browseable = yes
   read only = no
   guest ok = yes
   force user = nobody
```

## Paso 4: Creación de Usuarios de Red

Samba utiliza su propia base de datos de contraseñas. Para añadir un usuario (que ya debe existir en el sistema Linux):

```bash
sudo smbpasswd -a nombre_usuario
```

## Paso 5: Reinicio y Verificación

Reiniciamos el servicio para aplicar los cambios:

```bash
sudo systemctl restart smbd
```

### Verificación desde Windows
1.  Abre el Explorador de Archivos.
2.  En la barra de direcciones, escribe: `\\<IP_DEL_SERVIDOR>\Publico`.
3.  Deberías poder ver y crear archivos dentro de la carpeta.

:::tip[Aplicación Profesional]
Si quieres que el recurso se conecte automáticamente cada vez que inicies sesión en Windows, usa el comando:
`net use Z: \\IP_SERVIDOR\Publico /persistent:yes`
:::

## Reto de Administración
Modifica el archivo `smb.conf` para crear un nuevo recurso llamado `[Privado]` que:
1.  No permita el acceso a invitados (`guest ok = no`).
2.  Solo sea accesible para un grupo de usuarios específico (`valid users = @profesores`).
