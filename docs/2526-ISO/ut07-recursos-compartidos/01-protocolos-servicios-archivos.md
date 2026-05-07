---
title: Protocolos de Servicios de Archivos
sidebar_position: 1
description: Conceptos fundamentales sobre el intercambio de archivos en red y los protocolos SMB y NFS.
---

## Introducción a los Sistemas de Archivos en Red

En una red corporativa, la información no puede residir únicamente en los discos locales de cada puesto de trabajo. Los **Sistemas de Archivos en Red (NAS - Network Attached Storage)** permiten que múltiples clientes accedan a un repositorio centralizado de datos, facilitando la colaboración, el backup y la seguridad.

El sistema operativo actúa como un **servidor de archivos** cuando exporta partes de su sistema de archivos local para que sean montadas o mapeadas por otros sistemas a través de la red.

## Protocolo SMB/CIFS (El estándar de Windows)

**SMB (Server Message Block)** es un protocolo de capa de aplicación utilizado principalmente por sistemas Microsoft Windows para compartir archivos, impresoras y puertos serie.

*   **SMB 1.0 (CIFS)**: La versión antigua (ahora considerada insegura).
*   **SMB 2.x/3.x**: Versiones modernas con mejoras significativas en rendimiento y seguridad (cifrado de extremo a extremo).

:::tip[Seguridad en SMB]
Desde el ataque de ransomware *WannaCry*, es una práctica obligatoria para cualquier administrador deshabilitar SMBv1 en toda la red y forzar el uso de SMBv3, que incluye firmas digitales para evitar ataques de tipo "Man-in-the-Middle".
:::

### Funcionamiento de SMB
SMB funciona sobre el modelo cliente-servidor. En Windows, esto se gestiona mediante el servicio **LanmanServer** (Servidor) y **LanmanWorkstation** (Cliente). Utiliza el puerto **TCP 445** en versiones modernas.

## Protocolo NFS (El estándar de Linux/Unix)

**NFS (Network File System)** fue desarrollado por Sun Microsystems y es el estándar de facto en entornos Unix/Linux. A diferencia de SMB, NFS está diseñado para que el recurso remoto parezca, a todos los efectos, un disco local más.

*   **NFSv3**: Muy rápido pero menos seguro (basado en IP/UID).
*   **NFSv4**: Incluye soporte para autenticación fuerte (Kerberos) y atraviesa mejor los firewalls.

:::info[¿Cuándo usar cada uno?]
En una red puramente Windows, **SMB** es la elección natural por su integración con Active Directory. En centros de datos con servidores Linux comunicándose entre sí, **NFS** ofrece un rendimiento superior y menor sobrecarga de red.
:::

## Comparativa Técnica

| Característica | SMB / CIFS | NFS |
| :--- | :--- | :--- |
| **Origen** | Microsoft / IBM | Sun Microsystems |
| **Entorno ideal** | Windows / Redes mixtas | Linux / Unix / HPC |
| **Autenticación** | Basada en Usuario (NTLM/Kerberos) | Basada en IP/Host (o Kerberos en v4) |
| **Puerto principal** | TCP 445 | TCP 2049 |
| **Bloqueo de archivos** | Muy estricto (oportunista) | Menos estricto (Stateful en v4) |

## Resolución de Nombres y Descubrimiento

Para acceder a estos recursos, no solemos usar direcciones IP, sino nombres.
1.  **NetBIOS/WINS**: Método antiguo de Windows para resolución de nombres.
2.  **DNS**: El método estándar actual.
3.  **mDNS (Avahi/Bonjour)**: Utilizado para descubrimiento automático en redes locales sin servidor central.
