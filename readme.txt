# PA!.Store - Software de Gestión para Talleres Técnicos

## Descripción General
**PA!.Store** es una aplicación web (Software como Servicio - SaaS) diseñada para ayudar a los talleres de reparación técnica a gestionar eficientemente sus operaciones. La aplicación permite registrar equipos, seguir el progreso de las reparaciones, gestionar un inventario de repuestos, administrar proveedores y notificar a los clientes sobre el estado de sus equipos.

La arquitectura está construida sobre **Firebase** y está diseñada para ser **multiusuario**, donde cada taller opera en su propio entorno de datos seguro y privado.

## Estado Actual del Proyecto
El proyecto se encuentra en una fase funcional avanzada, con las siguientes características implementadas y funcionando:

* **Autenticación Segura:** Sistema de registro e inicio de sesión de usuarios.
* **Módulo de Reparaciones (Remitos):**
    * Creación de órdenes de reparación con detalles del cliente y del equipo.
    * Visualización detallada de cada orden, incluyendo un historial de cambios.
    * Actualización de estados de reparación (Ej: "Recibido", "En revisión", "Listo para retirar").
* **Módulo de Inventario:**
    * Gestión completa (Crear, Leer, Editar, Borrar) de repuestos.
    * Interfaz visual avanzada con un dashboard de resumen (valor total, items bajos) y vistas de tabla y tarjetas.
    * Indicadores visuales de stock bajo.
* **Módulo de Proveedores:**
    * Gestión completa (CRUD) de proveedores.
    * Integración con el módulo de inventario, permitiendo vincular cada repuesto a su proveedor.
* **Automatización de Stock:**
    * Funcionalidad para asignar repuestos a una orden de reparación.
    * El stock del inventario se descuenta automáticamente de forma segura (usando transacciones de Firestore).
* **Análisis Financiero por Reparación:**
    * Cálculo y visualización en tiempo real del costo de los repuestos y la ganancia estimada para cada orden de reparación.
* **Notificaciones por WhatsApp:**
    * Sistema para generar y enviar mensajes de WhatsApp pre-configurados y profesionales a los clientes.
* **Seguridad a Nivel de Servidor:**
    * **Reglas de Seguridad de Firestore:** Implementadas para asegurar que cada taller solo pueda acceder a sus propios datos (`remitos`, `inventario`, `proveedores`).
    * **Validación de Datos:** Reglas que previenen la escritura de datos corruptos o inválidos (ej. stock negativo).
* **Personalización del Perfil:**
    * Los dueños de talleres pueden configurar el nombre de su negocio y personalizar las plantillas de mensajes de WhatsApp.

---

## Historial de Cambios y Errores Solucionados (Resumen)

Este es un resumen de la evolución del proyecto y los problemas clave que solucionamos:

1.  **Análisis Inicial y Estructura:**
    * **Revisión Completa:** Se analizaron todos los archivos (HTML, CSS, JS) para entender la estructura y encontrar errores iniciales.
    * **Errores Corregidos:** Se solucionaron errores de sintaxis en HTML (scripts duplicados, etiquetas mal cerradas) y fallos críticos en las importaciones de módulos de JavaScript.

2.  **Refactorización de `localStorage`:**
    * **Problema:** La aplicación dependía en exceso de `localStorage` para pasar datos entre páginas y guardar configuraciones, lo cual no es seguro ni escalable para un SaaS.
    * **Solución:** Se eliminó por completo el uso de `localStorage`.
        * Los datos temporales entre páginas ahora se pasan a través de **parámetros de URL**.
        * Toda la configuración del usuario (perfil, plantillas de mensajes) ahora se lee y escribe directamente en **Firestore**.

3.  **Mejoras Visuales y de UX (Inventario y Detalles):**
    * **Problema:** La interfaz de inventario y de detalles era funcional pero muy básica.
    * **Solución:** Se implementó un rediseño completo inspirado en los mejores sistemas de gestión:
        * Creación de un **dashboard de resumen** en el inventario.
        * Implementación de **vistas de tabla y tarjetas** intercambiables.
        * Creación de un **panel de resumen financiero** en la vista de detalles del remito.
    * **Errores de CSS Solucionados:** Se depuraron varios problemas de CSS que surgieron al separar los estilos en componentes, asegurando que el diseño se renderice correctamente.

4.  **Implementación de Módulos Clave:**
    * Se construyó desde cero el módulo de **Proveedores**.
    * Se conectó el módulo de **Inventario** con el de **Reparaciones**, implementando la lógica de **descuento automático de stock** mediante transacciones seguras de Firestore.

---

## Funcionalidad Pendiente (Próximos Pasos)

Lo que falta para completar el sistema de gestión de roles y llevar el SaaS al siguiente nivel:

* **Activar el Plan Blaze de Firebase:**
    * **Bloqueo Actual:** No podemos desplegar Cloud Functions (necesarias para la gestión de equipos) hasta que el proyecto se actualice del plan "Spark" al plan "Blaze".
    * **Acción Requerida:** Registrar una tarjeta de crédito en la consola de Firebase. El costo seguirá siendo de **$0** gracias al generoso nivel gratuito.

* **Desplegar la Cloud Function de "Crear Técnico":**
    * **Código Listo:** Ya tenemos el código de la función que permite a un "dueño" de taller crear una cuenta segura para un "técnico".
    * **Acción Requerida:** Una vez activado el plan Blaze, ejecutar el comando `firebase deploy --only functions` en la terminal.

* **Implementar la Interfaz de Gestión de Equipo:**
    * **Tarea:** Añadir la sección "Mi Equipo" en la página de "Mi Perfil" para que el dueño del taller pueda usar la Cloud Function para invitar a sus técnicos.
    * **Acción Requerida:** Modificar `perfil.html` y `perfil.js` para añadir el formulario y la lógica de llamada a la función.

* **Adaptar la Interfaz a los Roles:**
    * **Tarea:** Hacer que la aplicación oculte ciertas secciones (como el "Resumen Financiero" o la página de "Proveedores") si el usuario que ha iniciado sesión tiene el rol de "técnico".
    * **Acción Requerida:** Aplicar pequeñas modificaciones en varios archivos JavaScript para que lean el `rol` del usuario desde la sesión y ajusten la visibilidad de los elementos. 