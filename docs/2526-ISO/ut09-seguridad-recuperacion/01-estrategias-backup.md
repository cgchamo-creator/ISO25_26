---
title: Estrategias de Backup y Recuperación
sidebar_position: 1
description: Conceptos fundamentales sobre la planificación de copias de seguridad y la regla 3-2-1.
---

## La Importancia del Backup

En administración de sistemas, no se trata de *si* un disco va a fallar, sino de *cuándo* lo hará. Una copia de seguridad es la única garantía de continuidad de negocio. Como administrador, tu responsabilidad es asegurar que los datos no solo existan, sino que sean **recuperables** en un tiempo razonable.

## La Regla de Oro: Estrategia 3-2-1

Para que una estrategia de backup se considere profesional, debe seguir la regla 3-2-1:

1.  **3 copias de tus datos**: El archivo original y al menos dos copias adicionales.
2.  **2 soportes diferentes**: No guardes el backup en el mismo servidor o cabina de discos. Usa discos externos, NAS o cintas.
3.  **1 copia fuera de la oficina (Off-site)**: Imprescindible en caso de incendio, robo o inundación. La nube (Cloud) es el destino ideal hoy en día.

:::tip[¿Copia en el mismo disco?]
Guardar una copia de seguridad en una partición distinta del mismo disco físico **NO** es un backup. Si el motor del disco falla, pierdes ambas particiones.
:::

## Tipos de Copias de Seguridad

Dependiendo de cómo gestionemos los cambios en los datos, tenemos tres tipos principales:

### 1. Copia Completa (Full Backup)
Copia todos los archivos seleccionados. 
*   **Ventaja**: Recuperación más rápida (solo necesitas un archivo).
*   **Desventaja**: Lenta de realizar y ocupa mucho espacio.

### 2. Copia Incremental
Solo copia los archivos que han cambiado desde la **última copia** (sea completa o incremental).
*   **Ventaja**: Muy rápida y ocupa poco espacio.
*   **Desventaja**: Para restaurar, necesitas la última completa y **todas** las incrementales posteriores. Si una falla, la cadena se rompe.

### 3. Copia Diferencial
Copia los archivos que han cambiado desde la **última copia completa**.
*   **Ventaja**: Más fácil de restaurar que la incremental (solo necesitas la completa y la última diferencial).
*   **Desventaja**: Con el tiempo, las diferenciales se vuelven tan grandes como la completa.

:::info[RPO y RTO]
*   **RPO (Recovery Point Objective)**: Cuántos datos estamos dispuestos a perder (ej. 24 horas).
*   **RTO (Recovery Time Objective)**: Cuánto tiempo puede estar la empresa parada mientras restauramos (ej. 2 horas).
:::

## Soportes de Almacenamiento

*   **NAS (Network Attached Storage)**: Discos en red, ideales para copias rápidas en la propia oficina.
*   **Cloud Backup**: Servicios como Azure Backup o AWS S3. Ofrecen el "1" de la regla 3-2-1.
*   **Cintas LTO**: Siguen vigentes en grandes empresas por su durabilidad y protección contra Ransomware (son *offline* por naturaleza).
