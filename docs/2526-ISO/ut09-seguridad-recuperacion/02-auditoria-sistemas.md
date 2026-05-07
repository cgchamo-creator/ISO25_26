---
title: Auditoría de Sistemas
sidebar_position: 2
description: Configuración de políticas de seguimiento para controlar el acceso a archivos y el uso del sistema.
---

## ¿Qué es la Auditoría?

La auditoría de sistemas es el proceso de registrar acciones realizadas por los usuarios o el propio sistema operativo. Mientras que los logs de sistema nos dicen si un servicio falló, los **logs de auditoría** nos dicen quién borró una carpeta compartida o quién intentó cambiar su propia contraseña.

## Auditoría en Windows

En Windows, la auditoría no está activa por defecto para todo. Se gestiona mediante **Directivas de Grupo (GPO)**, ya sean locales o de dominio.

### Pasos para auditar el acceso a archivos:
1.  **Activar la Directiva**: En `secpol.msc` (Directiva de seguridad local), activar "Auditar el acceso a objetos".
2.  **Configurar el Objeto**: En la carpeta que queremos vigilar, ir a Propiedades -> Seguridad -> Opciones avanzadas -> **Auditoría**.
3.  **Seleccionar el evento**: Podemos elegir auditar solo los fallos (intentos denegados) o también los éxitos (lecturas/escrituras correctas).

:::info[Impacto en el Rendimiento]
Auditar el "éxito" de lectura en una carpeta con miles de archivos generará una cantidad masiva de logs y puede ralentizar el servidor. Audita solo lo estrictamente necesario.
:::

## Auditoría en Linux (auditd)

En Linux, el sistema estándar es el demonio `auditd`. Es extremadamente potente y permite vigilar llamadas al sistema (syscalls) en tiempo real.

*   **Archivo de configuración**: `/etc/audit/auditd.conf`
*   **Reglas de auditoría**: `/etc/audit/rules.d/audit.rules`
*   **Herramienta de consulta**: `ausearch` y `aureport`.

### Ejemplo de Regla de Auditoría
Si queremos vigilar quién modifica el archivo crítico `/etc/passwd`:
```bash title="Terminal Linux"
# Añadir una regla de vigilancia (Watch) con una clave (Key) para buscarla luego
auditctl -w /etc/passwd -p wa -k cambios_usuarios
# -p wa: vigila Write (escritura) y Attribute changes (cambios de atributos)
```

## El Registro de Seguridad

Todos los eventos de auditoría se guardan en lugares específicos que hemos visto en la UT anterior:
*   En **Windows**: Se consultan en el "Registro de Seguridad" del Visor de Eventos.
*   En **Linux**: Se consultan en `/var/log/audit/audit.log`.

:::tip[Principio de Responsabilidad]
La auditoría es la herramienta que garantiza el **no repudio**: un usuario no puede negar haber realizado una acción si existe un registro de auditoría fiable que lo vincula a ella.
:::
