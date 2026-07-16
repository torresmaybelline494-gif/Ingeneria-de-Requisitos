# Especificación de Requisitos de Software (SRS) - Sistema de Inventario

## 1. Alcance y Fronteras del Software

Este sistema está diseñado para el control interno de inventario de una tienda local.

### 1.1 Inclusiones (En Alcance)
* **Módulo de Registro de Productos:** Permitirá la creación, lectura, actualización y desactivación de productos en el inventario de la tienda.
* **Mantenimiento de Stock en Tiempo Real:** El sistema disminuirá e incrementará de manera automática las existencias ante transacciones o movimientos validados de entrada/salida[cite: 2].
* **Algoritmo de Control de Existencias Críticas:** Validación e identificación en tiempo real de productos que queden por debajo del límite mínimo establecido para cada producto[cite: 2].
* **Validación de Códigos de Barra de El Salvador:** El sistema solo permitirá el registro de productos con códigos de barra de formato oficial local y globales válidos en El Salvador (ej. estándares EAN-13, validando que el prefijo del país sea correcto)[cite: 2].

### 1.2 Exclusiones (Fuera de Alcance)
* **Pasarela de Pago Online (E-commerce):** El sistema no gestionará transacciones de pago electrónico orientadas al cliente final ni tendrá un carrito de compras web externo.
* **Módulo de Facturación Electrónica:** No incluye interfaces de integración ni envío de documentos tributarios electrónicos (DTE) con el Ministerio de Hacienda de El Salvador.
* **Gestión Multisucursal:** El software operará para una base de datos centralizada de una única sucursal física.

---

## 2. Restricciones Tecnológicas Obligatorias

* **Gestor de Base de Datos:** Se impone de manera estricta el uso de **PostgreSQL** para toda la persistencia del sistema, debido a que el cliente ya posee esta infraestructura de servidor configurada[cite: 2].
* **Entorno de Ejecución:** El sistema debe desarrollarse bajo arquitectura web nativa compatible con navegadores estándar modernos (Google Chrome, Mozilla Firefox y Safari)[cite: 2].

---

## 3. Requisitos de Calidad (Basados en IEEE 830 - Cero Ambigüedad)

Para evitar la ambigüedad que invalida los requisitos, se definen las siguientes métricas de verificación exactas[cite: 2]:

* **REQ-QA-01 (Velocidad de Respuesta):** El sistema deberá renderizar y actualizar el stock visual de un producto en la pantalla del usuario en un tiempo inferior a **1.2 segundos** tras registrarse una transacción, bajo una conexión de red intranet estándar de la tienda.
* **REQ-QA-02 (Concurrencia en PostgreSQL):** La base de datos PostgreSQL debe soportar hasta **20 transacciones simultáneas** de actualización de stock sin generar bloqueos (*deadlocks*) ni pérdida de integridad de datos.
* **REQ-QA-03 (Seguridad):** Todas las credenciales de los usuarios administradores u operarios almacenadas en la base de datos de PostgreSQL deben estar cifradas utilizando el algoritmo de cifrado irreversible **bcrypt**.

---

## 4. Historias de Usuario (Módulo de Inventario)

### US-01: Alerta de Existencias Críticas (Stock Bajo)
* **COMO:** Administrador de la tienda[cite: 2]
* **QUIERO:** Que el sistema me envíe una alerta visual en pantalla cuando un producto tenga pocas unidades disponibles[cite: 2]
* **PARA:** Gestionar a tiempo el reabastecimiento con el proveedor y no perder ninguna venta[cite: 2].

#### Criterio de Aceptación (Escrito en Formato Gherkin)[cite: 2]
```gherkin
Escenario: El stock físico baja de las unidades mínimas permitidas
  Dado que el producto "Refresco Cola 500ml" tiene un stock mínimo configurado de 10 unidades en el sistema
  Y su stock físico actual registrado es de 11 unidades
  Cuando un operario de inventario registre una salida/venta de 2 unidades
  Entonces el sistema deberá actualizar el stock a 9 unidades en la base de datos PostgreSQL
  Y desplegar una notificación visual con etiqueta de advertencia roja en el panel principal del Administrador.
