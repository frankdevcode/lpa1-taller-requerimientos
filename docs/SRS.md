# SRS — Sistema de Reservas de Hoteles
Versión: 1.0  
Fecha: 2026-03-25  
Proyecto: LPA1 POO — Requerimientos  

## Historial de versiones
|Versión|Fecha|Descripción|
|---:|---|---|
|1.0|2026-03-25|Primera versión del SRS derivada de entrevista.|

## 1. Introducción
### 1.1 Propósito
Este documento especifica los requerimientos del **Sistema de Reservas de Hoteles** (en adelante, “el Sistema”), a partir de la entrevista levantada con personal del hotel. El objetivo es definir de forma verificable las necesidades funcionales y no funcionales para el desarrollo del Sistema.

### 1.2 Alcance
El Sistema permite:
- Registrar hoteles y sus habitaciones con información, servicios, fotos, estados y políticas.
- Gestionar temporadas/calendarios que afectan precios y disponibilidad.
- Registrar clientes.
- Buscar habitaciones por criterios y consultar detalles.
- Formalizar reservas a través de confirmación y pago.
- Gestionar cancelaciones y reembolsos según políticas.
- Registrar calificaciones y comentarios posteriores a la estancia, y calcular promedios.

Fuera de alcance en esta versión:
- Integración con pasarelas de pago específicas.
- Facturación electrónica y contabilidad.
- Gestión interna operativa del hotel (housekeeping, asignación de personal).

### 1.3 Definiciones, acrónimos y abreviaturas
|Término|Definición|
|---|---|
|Hotel activo|Hotel habilitado para ser mostrado y considerado en búsquedas/reservas.|
|Habitación activa|Habitación habilitada para ser mostrada y reservada.|
|Temporada|Periodo con condiciones de precio/promoción específicas (alta/baja u otras).|
|Política de cancelación|Reglas que determinan penalidades, plazos y reembolso ante una cancelación.|
|Reserva formalizada|Reserva que existe luego de una confirmación de pago exitosa.|
|SRS|Software Requirements Specification.|

### 1.4 Referencias
- Entrevista: [entrevista.pdf](file:///c:/Universidad/lpa1-taller-requerimientos/docs/entrevista.pdf)

### 1.5 Convenciones del documento
Los requerimientos se identifican con:
- FR-XX: Requerimientos Funcionales
- NFR-XX: Requerimientos No Funcionales
- BR-XX: Reglas de Negocio
- CON-XX: Restricciones

## 2. Descripción general
### 2.1 Perspectiva del producto
El Sistema es una aplicación para administrar información de hoteles y habitaciones y permitir a clientes consultar disponibilidad y realizar reservas con pago, apoyándose en calendarios de ocupación y temporadas.

### 2.2 Funciones del producto (resumen)
- Gestión de hoteles (datos, ubicación, servicios, fotos, ofertas, estado).
- Gestión de habitaciones (tipo, descripción, servicios, capacidad, fotos, estado, calendario).
- Gestión de políticas (pago y cancelación) por hotel/habitación/temporada.
- Búsqueda de habitaciones por fecha, ubicación, calificación y precio (combinable).
- Flujo de reserva con confirmación de pago.
- Calificaciones y comentarios posteriores a la estancia y cálculo de promedios.

### 2.3 Clases de usuarios
|Usuario|Descripción|
|---|---|
|Cliente|Busca habitaciones, consulta detalles, realiza pagos, reserva, cancela, califica y comenta.|
|Administrador turístico|Registra y mantiene hoteles/habitaciones, temporadas, ofertas y políticas.|
|Asistente|Apoya en la gestión y actualización de hoteles/habitaciones.|

### 2.4 Restricciones
|ID|Restricción|
|---|---|
|CON-01|Solo hoteles y habitaciones con estado **activo** pueden ser mostrados y considerados para reservas.|
|CON-02|Una reserva no puede exceder la capacidad máxima definida para la habitación.|
|CON-03|El precio debe poder variar por temporada y por cantidad de personas (sin exceder capacidad).|

### 2.5 Suposiciones y dependencias
- Se asume que existe un mecanismo de confirmación de pago (interno o externo) que retorna un resultado exitoso o fallido.
- Se asume que las fechas se gestionan en una zona horaria única del Sistema.

## 3. Requerimientos específicos
### 3.1 Requerimientos funcionales (FR)
|ID|Requerimiento|Prioridad|Criterio de aceptación (resumen)|Fuente|
|---|---|---:|---|---|
|FR-01|El Sistema debe permitir registrar un hotel con **nombre, dirección, teléfono y correo electrónico**.|Alta|Al guardar un hotel, dichos campos quedan almacenados y son visibles al consultar el hotel.|Entrevista (registro de hotel)|
|FR-02|El Sistema debe permitir registrar la **ubicación geográfica** del hotel (p. ej., ciudad/zona o coordenadas).|Alta|El hotel puede ser filtrado por ubicación en la búsqueda.|Entrevista (ubicación)|
|FR-03|El Sistema debe permitir registrar una **descripción** del hotel y su **lista de servicios** (p. ej., restaurante, piscina, gimnasio).|Media|Al consultar el hotel, se visualiza la descripción y servicios registrados.|Entrevista (servicios)|
|FR-04|El Sistema debe permitir asociar **fotos** al hotel.|Media|Se pueden subir/registrar fotos y se listan al consultar el hotel.|Entrevista (fotos)|
|FR-05|El Sistema debe permitir registrar **ofertas/promociones** del hotel por temporada (p. ej., descuentos en temporada baja, paquetes).|Media|Una oferta queda asociada a un hotel y a un periodo/temporada y puede ser consultada.|Entrevista (ofertas)|
|FR-06|El Sistema debe permitir registrar habitaciones asociadas a un hotel con **tipo, descripción, precio, servicios incluidos, capacidad y fotos**.|Alta|Una habitación queda vinculada al hotel y muestra todos esos campos al consultarla.|Entrevista (habitaciones)|
|FR-07|El Sistema debe permitir configurar **condiciones de pago** por hotel y/o por tipo de habitación (p. ej., pago completo por adelantado o pago al llegar).|Alta|La reserva muestra la condición aplicable antes del pago/confirmación.|Entrevista (condiciones de pago)|
|FR-08|El Sistema debe permitir configurar **políticas de cancelación** por hotel y/o por tipo de habitación y/o por temporada.|Alta|Ante una cancelación, el Sistema determina la política aplicable.|Entrevista (políticas)|
|FR-09|El Sistema debe permitir calcular el **reembolso** o **penalidad** al cancelar una reserva según la política de cancelación aplicable.|Alta|Al cancelar una reserva, se obtiene el monto de penalidad/reembolso acorde a la política.|Entrevista (reembolsos)|
|FR-10|El Sistema debe permitir marcar hoteles con estado **activo/inactivo**.|Alta|Un hotel inactivo no aparece en resultados de búsqueda ni puede reservarse.|Entrevista (hoteles inactivos)|
|FR-11|El Sistema debe permitir marcar habitaciones con estado **activo/inactivo** y registrar un motivo (p. ej., mantenimiento, remodelación, desinfección).|Alta|Una habitación inactiva no puede ser reservada y su motivo queda consultable internamente.|Entrevista (habitaciones inactivas)|
|FR-12|El Sistema debe permitir configurar precios que varíen según **temporada**.|Alta|La consulta de precio para una fecha en temporada A difiere de temporada B si así se configuró.|Entrevista (temporadas y precios)|
|FR-13|El Sistema debe permitir calcular el precio en función de la **cantidad de personas** siempre que no exceda la capacidad máxima.|Alta|Si personas > capacidad, la reserva no puede continuar. Si no, se calcula precio según reglas configuradas.|Entrevista (precio por personas/capacidad)|
|FR-14|El Sistema debe gestionar un **calendario regional** de temporadas y un **calendario por hotel** (alineado o complementario).|Media|Se pueden consultar temporadas regionales y por hotel y aplicarlas a precios/ofertas.|Entrevista (calendario regional y por hotel)|
|FR-15|El Sistema debe mantener un **calendario de disponibilidad por habitación**, indicando fechas reservadas y disponibles.|Alta|Al consultar una habitación, se visualiza su disponibilidad y se evita reservar fechas ocupadas.|Entrevista (calendario por habitación)|
|FR-16|El Sistema debe permitir registrar clientes con **nombre completo, teléfono, correo electrónico y dirección**.|Alta|Un cliente puede registrarse y luego iniciar un proceso de búsqueda/reserva.|Entrevista (registro cliente)|
|FR-17|El Sistema debe permitir buscar habitaciones por **fecha**.|Alta|Al buscar con rango de fechas, solo se listan habitaciones disponibles en ese rango.|Entrevista (búsqueda por fecha)|
|FR-18|El Sistema debe permitir buscar habitaciones por **ubicación**.|Alta|Al buscar por ubicación, se listan hoteles/habitaciones en la ubicación seleccionada.|Entrevista (búsqueda por ubicación)|
|FR-19|El Sistema debe permitir buscar habitaciones por **calificación**.|Media|Se puede filtrar por un umbral de calificación promedio.|Entrevista (búsqueda por calificación)|
|FR-20|El Sistema debe permitir buscar habitaciones por **precio**.|Media|Se puede filtrar por rango de precio.|Entrevista (búsqueda por precio)|
|FR-21|El Sistema debe permitir combinar múltiples criterios de búsqueda (fecha + ubicación + calificación + precio).|Alta|La búsqueda aplica todos los filtros seleccionados simultáneamente.|Entrevista (combinar criterios)|
|FR-22|El Sistema debe permitir ver el **detalle de una habitación**: descripción, características, servicios incluidos, fotos, calificación y comentarios.|Alta|Al seleccionar una habitación desde resultados, se muestra la información completa.|Entrevista (detalle de habitación)|
|FR-23|El Sistema debe permitir que el cliente confirme la selección de habitación y proceda a realizar el **pago**.|Alta|Existe un paso explícito de confirmación previo al pago.|Entrevista (confirmación y pago)|
|FR-24|El Sistema debe formalizar una reserva solo cuando exista **confirmación de pago exitosa**.|Alta|Si el pago falla, no se crea (o no queda activa) la reserva; si el pago es exitoso, la reserva queda registrada.|Entrevista (reserva formalizada por pago)|
|FR-25|El Sistema debe invitar al cliente a registrar una **calificación** y un **comentario** después de cada estancia.|Media|Solo después de una estancia completada se habilita calificar/comentar.|Entrevista (post-estancia)|
|FR-26|El Sistema debe asociar calificaciones y comentarios a una **habitación** y calcular una **calificación promedio** por habitación.|Media|Una habitación muestra su calificación promedio basada en calificaciones registradas.|Entrevista (promedio por habitación)|
|FR-27|El Sistema debe calcular una **calificación promedio general** del hotel en base a la información de sus habitaciones y/o estancias.|Media|El hotel muestra su calificación promedio general.|Entrevista (promedio de hotel)|

### 3.2 Reglas de negocio (BR)
|ID|Regla|
|---|---|
|BR-01|Una habitación en estado inactivo no debe considerarse disponible ni reservable para ninguna fecha.|
|BR-02|Un hotel en estado inactivo no debe aparecer en búsquedas ni admitir reservas.|
|BR-03|El precio final de una reserva debe calcularse aplicando: temporada vigente + cantidad de personas (sin exceder capacidad) + ofertas aplicables, si existen.|
|BR-04|Una cancelación debe evaluar la política aplicable en el momento de la cancelación para determinar penalidad y/o reembolso.|
|BR-05|Solo un cliente con una estancia completada asociada a una reserva debe poder publicar calificación/comentario sobre esa habitación.|

### 3.3 Requerimientos no funcionales (NFR)
|ID|Requerimiento|
|---|---|
|NFR-01|El Sistema debe validar campos obligatorios (p. ej., correo electrónico) y rechazar registros incompletos de hoteles, habitaciones y clientes.|
|NFR-02|El Sistema debe evitar conflictos de reserva: no debe permitir confirmar una reserva si existe ocupación superpuesta en el calendario de la habitación.|
|NFR-03|El Sistema debe mantener trazabilidad mínima entre reservas, pagos, cancelaciones y reembolsos.|
|NFR-04|El Sistema debe proteger datos personales (cliente) mediante controles de acceso según rol y evitando exposición innecesaria en listados públicos.|
|NFR-05|El Sistema debe ofrecer una experiencia de búsqueda que permita aplicar filtros múltiples sin perder el contexto de resultados.|

## 4. Modelo conceptual (alto nivel)
Entidades principales identificadas a partir de la entrevista:
- Hotel (datos básicos, ubicación, descripción, servicios, fotos, estado, ofertas, políticas, temporadas)
- Habitación (hotel, tipo, descripción, servicios, capacidad, fotos, estado, calendario, precios)
- Cliente (datos de registro)
- Reserva (cliente, habitación, fechas, ocupación, estado)
- Pago (reserva, monto, estado, confirmación)
- Cancelación/Reembolso (reserva, política aplicada, penalidad, monto)
- Comentario/Calificación (reserva o estancia completada, habitación, puntuación, texto)

## 5. Anexo A — Matriz de trazabilidad (entrevista → requerimientos)
|Tema de entrevista|Requerimientos|
|---|---|
|Registro de hotel: datos, ubicación, servicios, fotos|FR-01, FR-02, FR-03, FR-04|
|Ofertas por temporada y servicios adicionales|FR-05, FR-12, BR-03|
|Registro de habitación: tipo, precio, servicios, capacidad, fotos|FR-06, FR-13|
|Pagos, cancelaciones, reembolsos|FR-07, FR-08, FR-09, FR-23, FR-24, BR-04|
|Estados activo/inactivo en hotel/habitación|FR-10, FR-11, BR-01, BR-02, CON-01|
|Temporadas (regional y por hotel)|FR-12, FR-14|
|Calendario por habitación (ocupada/disponible)|FR-15, NFR-02|
|Registro de cliente|FR-16|
|Búsqueda por fecha/ubicación/calificación/precio y combinación|FR-17, FR-18, FR-19, FR-20, FR-21|
|Detalle de habitación (fotos, servicios, comentarios, calificación)|FR-22|
|Calificaciones/comentarios y promedios|FR-25, FR-26, FR-27, BR-05|
