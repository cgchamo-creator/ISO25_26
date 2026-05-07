---
title: Permisos, Seguridad y Cuotas
sidebar_position: 2
description: Dominando la seguridad de los recursos compartidos y el control de cuotas de disco.
---

## La Jerarquía de Permisos en Red

Uno de los mayores errores de los administradores noveles es no entender la interacción entre los permisos de **Recurso Compartido** y los permisos de **Seguridad (Sistema de Archivos)**.

### Permisos de Compartido (Share Permissions)
Se aplican únicamente cuando se accede al recurso a través de la red. Son básicos: *Lectura*, *Cambio* y *Control Total*.

### Permisos de Seguridad (NTFS/POSIX)
Se aplican siempre, tanto si el acceso es local como si es a través de la red. Son mucho más granulares (Ejecutar, Listar contenido, Escritura, etc.).

:::important[La Regla de Oro]
Cuando un usuario accede a una carpeta compartida, el sistema calcula los permisos efectivos aplicando la **restricción más estricta** entre ambos niveles.
*   *Ejemplo*: Si en el Compartido tienes "Control Total" pero en Seguridad tienes "Solo Lectura", el resultado será **Solo Lectura**.
:::

## Permisos en Linux: POSIX y ACLs

En entornos Linux, tradicionalmente usamos el modelo UGO (User, Group, Others) con permisos `rwx`. Sin embargo, para entornos profesionales esto suele ser insuficiente.

*   **ACLs (Access Control Lists)**: Permiten asignar permisos específicos a múltiples usuarios o grupos en un mismo archivo/directorio sin cambiar el propietario principal.
*   **Comandos clave**: `getfacl` para ver y `setfacl` para modificar.

:::tip[Buenas Prácticas]
Como administrador, lo ideal es dar "Control Total" a "Todos" en el nivel de **Compartido** y gestionar la seguridad real de forma granular en el nivel de **Seguridad (NTFS/POSIX)**. Esto evita confusiones y solapamientos difíciles de depurar.
:::

## Cuotas de Disco

Para evitar que un solo usuario (o un log fuera de control) sature el almacenamiento del servidor, implementamos **Cuotas de Disco**.

### Tipos de Cuotas
1.  **Cuotas Blandas (Soft Limits)**: El sistema avisa al usuario o registra un evento en el log, pero permite seguir escribiendo.
2.  **Cuotas Duras (Hard Limits)**: El sistema impide cualquier escritura adicional una vez alcanzado el límite.

### Implementación
*   **Windows**: Se gestionan por volumen (clásico) o mediante el "Administrador de recursos del servidor de archivos" (granular).
*   **Linux**: Requiere soporte en el sistema de archivos (ext4, XFS) y se gestiona con herramientas como `quota`, `edquota` y `repquota`.

## Herencias y Propietarios

*   **Herencia**: Por defecto, los archivos nuevos heredan los permisos de su carpeta padre. Romper la herencia es necesario para crear "compartimentos estancos" de información.
*   **Propiedad (Ownership)**: El propietario de un archivo suele tener control total sobre él. En Windows, un administrador puede "Tomar posesión" de un archivo si el propietario original ya no está en la empresa.
