---
title: Gestión de Impresión en Red
sidebar_position: 3
description: Cómo funcionan los servidores de impresión y la gestión de colas en entornos corporativos.
---

## El Rol del Servidor de Impresión

Un servidor de impresión gestiona la comunicación entre los clientes (que envían trabajos) y las impresoras físicas. Sus funciones principales son:
*   **Almacenamiento (Spooling)**: Guardar temporalmente los trabajos en disco hasta que la impresora esté lista.
*   **Gestión de Colas**: Priorizar y organizar el orden de impresión.
*   **Distribución de Drivers**: Proporcionar automáticamente los controladores necesarios a los clientes.

## Arquitectura de Impresión en Windows

Windows utiliza el servicio **Cola de impresión (Spooler)**. 
*   **Puertos de impresión**: Pueden ser locales (USB/LPT), de red (TCP/IP estándar) o WSD (Web Services for Devices).
*   **Prioridades**: Puedes crear varias impresoras lógicas para una misma impresora física, asignando mayor prioridad a una de ellas (ej. para el departamento de dirección).
*   **Pool de impresión**: Agrupar varias impresoras físicas idénticas bajo un mismo nombre para repartir la carga.

:::info[¿Qué es el Spooler?]
El *spooler* es un archivo en disco (normalmente en `C:\Windows\System32\spool\PRINTERS`) donde se escriben los datos antes de enviarse. Si una impresión se queda bloqueada, reiniciar este servicio suele ser la primera medida de soporte técnico.
:::

## Impresión en Linux: CUPS

**CUPS (Common Unix Printing System)** es el estándar en Linux. Utiliza el protocolo **IPP (Internet Printing Protocol)**.

*   **Interfaz Web**: Por defecto, CUPS ofrece una administración muy potente a través del puerto **631** (ej. `http://localhost:631`).
*   **Compatibilidad**: Gracias a CUPS y Samba, un servidor Linux puede actuar como servidor de impresión para clientes Windows sin problemas.

:::tip[Protocolo IPP]
IPP es el futuro (y presente) de la impresión. Permite imprimir a través de HTTP/HTTPS, lo que facilita enormemente la gestión a través de firewalls y redes complejas.
:::

## Gestión de Colas y Permisos

Al igual que con los archivos, la impresión tiene seguridad:
*   **Imprimir**: Permiso básico para enviar trabajos.
*   **Administrar documentos**: Permite pausar, reiniciar o cancelar trabajos de otros usuarios.
*   **Administrar impresora**: Permite cambiar la configuración de la propia impresora.

### Problemas Comunes
1.  **Drivers corruptos**: La causa número uno de fallos en servidores Windows. Se recomienda usar "Controladores de impresión universales" siempre que sea posible.
2.  **Espacio en disco**: Si el disco donde está el *spool* se llena, nadie podrá imprimir.
3.  **Permisos de puerto**: A veces el usuario tiene permiso en la impresora pero no para comunicarse con el puerto TCP/IP.
