---
title: Gestión de Logs y Registros
sidebar_position: 1
description: Aprende cómo los sistemas operativos registran eventos críticos y cómo interpretarlos para resolver incidencias.
---

## Fundamentos de los Registros

Como administrador, los **logs** son tu caja negra. Cada vez que un servicio falla, un usuario intenta acceder sin permiso o el hardware detecta un error, el sistema operativo genera una entrada en un registro. Sin una buena gestión de logs, estarías "ciego" ante los problemas de tu infraestructura.

Un buen log debe responder siempre a: **¿Cuándo ocurrió?**, **¿Dónde ocurrió?**, **¿Qué componente falló?** y **¿Qué importancia tiene?**.

## Niveles de Severidad (Estándar Syslog)

En entornos profesionales (especialmente en Linux), se utiliza el estándar **Syslog** (RFC 5424) para categorizar la importancia de los eventos. Estos niveles van del 0 al 7:

*   **0 - Emergency**: El sistema es inusable (pánico del kernel).
*   **1 - Alert**: Acción inmediata requerida (ej. base de datos corrupta).
*   **2 - Critical**: Condiciones críticas (ej. fallo en dispositivo de almacenamiento).
*   **3 - Error**: Condiciones de error que impiden el funcionamiento de una función.
*   **4 - Warning**: Advertencias sobre posibles problemas futuros.
*   **5 - Notice**: Eventos normales pero significativos.
*   **6 - Informational**: Mensajes puramente informativos.
*   **7 - Debug**: Mensajes detallados para depuración de software.

:::tip Consejo de SysAdmin
Nunca ignores los niveles **2 (Critical)** y **3 (Error)** en tus revisiones diarias. Un sistema saludable puede tener cientos de "Informational", pero los "Errors" son los que indican una caída inminente.
:::

## Logs en Sistemas Windows

Windows utiliza una arquitectura centralizada llamada **Visor de Eventos**. A diferencia de Linux, donde los logs suelen ser archivos de texto plano, Windows los almacena en archivos binarios (`.evtx`). Los registros principales son:

*   **Sistema**: Eventos generados por los componentes del sistema operativo (drivers, servicios del kernel).
*   **Seguridad**: Registros de auditoría (logins, intentos de acceso a archivos, cambios en privilegios).
*   **Aplicación**: Eventos registrados por aplicaciones de terceros (SQL Server, navegadores, etc.).

## Logs en Sistemas Linux

En las distribuciones modernas (como **Ubuntu** o **Debian**), la gestión de logs recae sobre **systemd-journald**. Aunque todavía existen archivos tradicionales en `/var/log/`, la herramienta principal para consultar logs es el diario binario del sistema.

*   `/var/log/auth.log`: Intentos de login y uso de sudo.
*   `/var/log/syslog`: Registro general del sistema (excepto temas de autenticación).
*   `/var/log/kern.log`: Mensajes directos del kernel.

:::info El Diario de Systemd
El `journald` almacena los logs en formato binario para permitir búsquedas ultrarrápidas y asegurar que los logs no sean manipulados fácilmente. Veremos cómo consultarlo en la sección de tutoriales.
:::
