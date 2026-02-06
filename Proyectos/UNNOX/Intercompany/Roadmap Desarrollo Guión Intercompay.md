## ✅ UNNOX-F005E — Roadmap de desarrollo (BC Intercompany)

### 0) Contexto y objetivo general

- Automatizar el flujo **Intercompany** entre empresas del mismo tenant (A → B/C) para:
    - Convertir una **Venta** en Empresa A (al cliente final) en **Compra IC** hacia la Empresa vinculada que tiene stock/fabrica (Empresa B).
    - Ejecutar el flujo estándar de **Hoja de Demanda → mensajes de acción → pedido compra → envío IC** sin intervención manual.
    - Asegurar **trazabilidad** y **traspaso de campos** clave.
    - Ajustar **cabecera del albarán** emitido por B para que muestre los datos corporativos de A.
    - Automatizar el **cierre/recepción** en A cuando B factura

---

# 1) Preparación y análisis (Momento 0)

### 1.1 Revisión de configuración estándar (pre-requisitos)

> Antes de codificar, comprobar que el escenario estándar está listo y consistente.

- Empresas vinculadas configuradas entre A y B (y/o C según escenario real)
- Uso de bandejas intercompany:
    - **Transacciones de bandeja de salida/entrada/procesadas** (trazabilidad del envío/recepción)
- Artículos:
    - Reposición = **Compra** cuando aplique envío directo.
    - “Nº proveedor” del artículo debe corresponder a un proveedor que sea una empresa vinculada (socio IC)
- Proveedor IC (en A):
    - Debe estar marcado como **“socio IC”** para permitir “Enviar pedido compra empresas vinculadas”

✅ **Deliverable de esta fase:** checklist de configuración + mini “happy path” manual hecho una vez (sin automatización) para entender puntos de enganche.

---

# 2) Diseño técnico (arquitectura y objetos)

### 2.1 Decisiones de arquitectura (para no morir en complejidad)

- **Separar por módulos**:
    1. **Campos y propagación** (RF001, RF003, RF009)
    2. **Automatización de Hoja de Demanda** (RF005, RF006)
    3. **Albarán cabecera “empresa A”** (RF007)
    4. **Cierre/recepción en A al facturar en B** (RF008)
- Considerar **2 botones** como pide el DDT:
    - Botón 1: automatización Demand Worksheet (RF006 + RF005 + RF009)
    - Botón 2: albarán con cabecera alternativa (RF007)
    - (Opcional Botón 3/refactor): cierre automático IC (RF008) si conviene desacoplar..

### 2.2 Estrategia para “viaje de datos” entre empresas

- Identificar qué campos viajan:
    - A → B: “Nº documento original”, y campos logísticos de líneas (RF009).
- Identificar puntos estándar donde enganchar:
    - Creación/recepción documento IC.
    - Conversión desde Hoja de Demanda a compra.
    - Registro factura venta en B para disparar lógica en A.

✅ **Deliverable:** diagrama simple (A:SO→Demand→PO→IC Outbox) → (B:IC Inbox→SO→Prod→Ship/Inv) + flujos de datos.

---

# 3) Implementación por requisitos (RF)

> Aquí va el “mapa de trabajo” paso a paso desde lo más fundacional (campos) hasta automatizaciones y reportes.

---

## RF001 — Campo “Nº documento original” en pedidos de venta

### 3.1 Crear campo y persistencia

- Crear campo **“Nº documento original”** en **Pedido de venta**.
- Objetivo: que en B, el pedido de venta recibido refleje el número del pedido de venta original creado en A entre empresa vinculada A y cliente final.

### 3.2 Viaje del dato A → B por Intercompany

- Implementar que el campo viaje en el procedimiento IC al enviar/recibir el documento.

✅ Checkpoint:

- Creo SO en A (cliente final) → se genera/recibe SO en B → “Nº documento original” aparece con el nº del SO de A.

---

## RF002 — Listado del campo en lista de pedidos de venta (Empresa B)

### 3.3 Extensión de lista

- Añadir “Nº documento original” como columna visible/listable en lista de pedidos de venta en B

✅ Checkpoint: filtro/ordenación por “Nº documento original” funciona.

---

## RF003 + RF004 — Campos IC en órdenes de producción y listados

### 3.4 Campos requeridos en Orden de Producción

Deben aparecer (y poder listarse) estos campos en órdenes de producción creadas desde pedidos de venta IC (en 4 estados):

- “Nº documento original”
- “Nombre” (del apartado Envío y Facturación, nombre del cliente final)
- “Nº documento referencia IC”.

Aplicar a órdenes en estados:

- Planificada
- Planificada en firme
- Lanzada
- Terminada.

### 3.5 Listados de órdenes de producción (RF004)

- Añadir esos campos como columnas listables en:
    - Lista planificadas
    - planificadas en firme
    - lanzadas
    - terminadas.

✅ Checkpoint: desde una SO IC, creo la OP y veo datos en OP + listas.

---

## RF005 — Marcar “Envío directo” automáticamente en el pedido de venta original (Empresa A)

### 3.6 Automatización de marcado “Envío directo”

Se debe marcar automáticamente el check “Envío directo” en el pedido de venta original con cliente final cuando:

1. La línea tiene sistema reposición = **Compra**
2. El “Nº proveedor” del artículo corresponde a alguna empresa vinculada.

✅ Checkpoint:

- Item con reposición compra + proveedor IC → al introducirlo en línea, se marca Envío directo.

---

## RF006 — Acción a nivel de línea en pedido de venta (botón principal de automatización)

> Este es el corazón del desarrollo: “un click” para generar todo lo que el usuario hacía en Hoja de Demanda + envío IC.

### 3.7 Acción en línea del pedido de venta

Crear una acción a nivel de **línea** que ejecute automáticamente:

#### Paso 1 — “Traer pedidos de venta” en Hoja de Demanda

- Ejecutar “Traer pedidos de venta” para obtener líneas marcadas como “Envío directo” y pendientes de procesar, sin hacerlo manual.

#### Paso 2 — Rellenar “N.º proveedor” en Hoja de Demanda

- Para cada línea:
    - Identificar producto
    - Leer “Nº proveedor” de la ficha del artículo
    - Rellenar automáticamente “N.º proveedor” de la línea de la Hoja de Demanda.

#### Paso 3 — “Aceptar mensaje de acción” + “Ejecución de mensajes de acción”

- Ejecutar ambas acciones automáticamente para las líneas generadas desde ese pedido de venta,
- Garantizar creación automática de **un único pedido de compra** vinculado que contenga sus líneas intercompany.

#### Paso 4 — Enviar pedido de compra a empresa vinculada (IC)

- Ir al **último pedido de compra creado**, cuyo proveedor sea “socio IC”
- Ejecutar “Env. Ped. Compra empresas vinculadas” sin intervención manual.

✅ Checkpoint end-to-end (A):

- Click en línea → Demand Worksheet se rellena + proveedores se asignan + action messages se ejecutan → se crea PO → se envía a IC Outbox.

---

## RF009 — Traspaso de campos de línea (A → B)

### 3.8 Propagación de campos logísticos a pedido recibido en B

El proceso automático del RF006 debe contemplar el traspaso de estos campos (línea de venta original → línea de venta recibida en B):

- Almacén APQ
- ADR
- Tipo de paletización
- Nº de bultos
- Tipo de embalaje
- Fecha entrega planificada
- Fecha envío planificada
- Fecha envío.

✅ Checkpoint:

- Tras recibir SO en B, las líneas tienen esos campos replicados fielmente.

---

## RF007 — Albarán de venta en B con cabecera corporativa de A (request page)

### 3.9 Adaptación del informe de albarán

Requisito:

- Al generar o previsualizar Albarán desde B, permitir que la **cabecera** muestre los datos corporativos de **A** en lugar de los locales de B.
- Para ello:
    - Crear un campo en la **request page** del informe de albarán para seleccionar la empresa vinculada..

### 3.10 Datos a extraer desde “Información empresa” (Company Information) de la empresa seleccionada

Campos requeridos:

- Nombre, Dirección, C.P., Población, Provincia, País/Región,
- Teléfono, Fax, CIF/NIF,
- Web, Email.

✅ Checkpoint:

- Previsualizo albarán en B → selecciono “Empresa A” → la cabecera cambia a datos de A.

> Nota: esto sustituye el “botón antes de crear/enviar” que comentabas: el DDT pide selector en request page (más limpio y estándar).

---

## RF008 — Automatizar cierre/recepción en A cuando B registra factura de venta

### 3.11 Trigger funcional

Cuando se registra la factura de venta en B:

1. Localizar el Pedido de Compra vinculado en A por “Nº Documento Referencia IC”.
2. Actualizar “Cantidad a recibir” de cada línea con la cantidad realmente enviada (soporta envíos parciales).
3. Recibir el pedido en A, generando el albarán de compra correspondiente (transacción “ficticia” para sincronización contable/logística)..

✅ Checkpoint:

- Facturo SO en B → automáticamente PO en A se actualiza y se recibe por la cantidad realmente enviada.

---

# 4) Integración, seguridad y trazabilidad

### 4.1 Trazabilidad IC (puntos de observación)

- Validar que en cada fase el flujo queda reflejado en:
    - Bandeja de salida/entrada/procesadas (IC).
- Registrar en log o mensajes controlados:
    - Nº doc original
    - Nº referencia IC
    - Documento resultante (PO/ SO / envíos / facturas)
    - Errores de envío/recepción

### 4.2 Gestión de errores (mínimo imprescindible)

- Si falla:
    - “Traer pedidos de venta”
    - creación de mensajes
    - ejecución de mensajes
    - envío IC
- Dejar el sistema en estado consistente (sin duplicar PO, sin re-ejecutar a ciegas).

---

# 5) Plan de pruebas (lo que tú y el consultor vais a ejecutar)

> El DDT deja pendiente un guion técnico y funcional; aquí te lo estructuro para que lo rellenes luego con pantallas..

## 5.1 Guion técnico (por RF)

- RF005: set “Envío directo” automático por condiciones (2 casos: cumple / no cumple).
- RF006: click acción línea → genera demanda → rellena proveedor → acepta/ejecuta mensajes → crea PO → envía IC
- RF001/RF002: “Nº documento original” viaja y lista en B.
- RF009: campos de líneas viajan a B.
- RF003/RF004: OP creada desde SO IC contiene campos y listas muestran columnas.
- RF007: albarán en B con selector de empresa → cabecera cambia..
- RF008: facturar en B actualiza/recibe PO en A con cantidades reales (incluye caso parcial)..

## 5.2 Guion funcional (para consultor)

- Escenario “producto no disponible en A pero sí en B/C”.
- Validar:
    - usuario apenas toca 1–2 acciones
    - trazabilidad IC
    - documento final al cliente con cabecera “A” aunque expida “B”.

---

# 6) Entregables finales y “Definition of Done”

- ✅ Botón/acción principal (RF006) funcionando e idempotente (sin duplicidades).
- ✅ Campos creados, viajando IC, visibles en listas (RF001–RF004, RF009).
- ✅ Albarán con selector de empresa en request page y cabecera correcta (RF007).
- ✅ Cierre/recepción automático en A al facturar en B (RF008).
- ✅ Guion de pruebas técnico preparado y evidencias



## ✅ Orden recomendado de implementación (RF)

> Criterio: **primero fundamento de datos + propagación**, luego **automatizaciones**, luego **presentación/reporting**, y por último **automatismo cross-company al facturar** (lo más delicado). Todo basado en los RF del DDT..pdf).pdf) 

### 1) **RF001** — Campo “Nº documento original” en pedidos de venta + que “viaje” por IC 

- Es **la clave de trazabilidad** y además será útil como “ID” para relacionar documentos entre empresas.
		-Prompt: Quiero comenzar el desarrollo del RF001 del documento UNNOX-F005E. Necesito que me ayudes a crear el campo "Nº documento original" en el Sales Header, añadirlo a la página de Pedido de Venta, definir su comportamiento, y dejar preparada la base para que este valor pueda viajar en Intercompany desde Empresa A a Empresa B. El requisito funcional dice que: - El campo “Nº documento original” debe existir en el pedido de venta. - En la empresa B debe verse el número del pedido original creado en la empresa A. - Ese dato debe viajar por Intercompany. Estamos en un chat nuevo sin contexto previo, así que recuérdame: - Qué objetos debo crear/extender. - Qué nombres técnicos usar. - Cómo evitar que se sobreescriba en documentos IC creados desde la bandeja. - Cómo preparar el terreno para la propagación IC (sin implementarla aún). Debes guiarme paso a paso en AL.
### 2) **RF009** — Traspaso de campos de línea A → B (logística/fechas/etc.)

- Si lo haces pronto, evitas re-trabajo cuando ya tengas automatizaciones y documentos generados: **todo el flujo se apoya en que los datos viajen bien**.
		- Prompt: Vamos con el RF009 del documento UNNOX-F005E. En este chat no tienes historial, así que necesito que recuperes el contexto: El RF009 indica que ciertos campos de línea del Pedido de Venta de la Empresa A deben viajar a la Empresa B cuando se genera el Pedido de Venta Intercompany. Los campos son: - Almacén APQ - ADR - Tipo de paletización - Nº de bultos - Tipo de embalaje - Fecha entrega planificada - Fecha envío planificada - Fecha envío Necesito que me digas: - Qué extensiones crear (tabla + página si aplica). - Cómo engancharme al proceso Intercompany para que estos campos viajen correctamente. - Si debo usar eventos del IC Outbox/Inbox o OnAfterCopyFrom… entre Sales Header/Sales Line. - Cómo evitar conflictos cuando los documentos los crea BC automáticamente. Guíame paso a paso en AL.

### 3) **RF005** — Marcar “Envío directo” automáticamente en la venta original 

- Es una condición previa natural para que la Hoja de Demanda recoja las líneas correctas.
		- Prompt: Quiero trabajar el RF005 del documento UNNOX-F005E. Recuérdame el contexto: Este RF indica que el check "Envío directo" debe marcarse automáticamente en las líneas del Pedido de Venta cuando se cumplan ambas condiciones: 1) El artículo tiene reposición = Compra. 2) El proveedor del artículo es una empresa vinculada. En este chat no tienes historial, así que necesito: - Qué eventos debo suscribir. - Desde qué tabla leo reposición/proveedor. - Cómo validar si el proveedor pertenece a empresas vinculadas. - Cómo marcar el campo automáticamente sin interferir con otros procesos IC. Necesito ejemplo AL y explicación clara.

### 4) **RF006** — Acción/botón que automatiza Hoja de Demanda + mensajes + PO + envío IC

- Es el “botón gordo”. Conviene hacerlo cuando ya:
    - existen campos,
    - viajan por IC,
    - y el “envío directo” se marca solo.
			- Prompt: Este chat es para el RF006 del documento UNNOX-F005E. Contexto a recordar: Necesito crear una acción a nivel de línea en el Pedido de Venta que automatice estos pasos: 1) Ejecutar "Traer pedidos de venta" en la Hoja de Demanda. 2) Rellenar automáticamente el campo "N.º proveedor" en cada línea de la Hoja de Demanda. 3) Ejecutar “Aceptar mensaje de acción”. 4) Ejecutar “Ejecución de mensajes de acción”. 5) Ir al último Pedido de Compra creado cuyo proveedor sea socio IC y enviarlo con “Enviar Pedido Compra empresas vinculadas”. Quiero que en este chat me digas: - Cómo crear la acción en la página del Sales Order Line. - Cómo navegar y manipular la Hoja de Demanda desde AL. - Cómo ejecutar programáticamente los mensajes de acción. - Cómo identificar el último PO creado. - Cómo ejecutar la acción de envío IC sin intervención manual. - Qué eventos del sistema son los correctos para engancharse. Necesito todo con detalle y AL.

### 5) **RF002** — Mostrar “Nº documento original” en lista de pedidos venta de B 

- Es **UI** (rápido) y ya tendrá sentido porque el dato estará viajando.
		- Prompt: Quiero implementar el RF002 del documento UNNOX-F005E. Contexto necesario: Este RF solo pide que el campo “Nº documento original”, que viaja desde A, se muestre como columna visible/listable en la lista de Pedidos de Venta en la Empresa B. En este chat sin contexto previo necesito: - Qué pageextension tocar (Sales Order List). - Cómo añadir el campo de forma correcta. - Si es necesario aplicar Sorting/Importance. - Cómo asegurar que se puede filtrar y ordenar por ese campo. Guíame con AL sencillo y limpio.

### 6) **RF003** — Campos requeridos en Órdenes de Producción (desde esos pedidos)
- Depende de que exista y viaje “Nº doc original” y “Nº doc referencia IC”.
		- Prompt: Vamos con el RF003 del documento UNNOX-F005E. Contexto necesario: Este RF pide que varios campos IC aparezcan en la cabecera de las Órdenes de Producción (Planificada, Planificada en firme, Lanzada y Terminada): - Nº documento original - Nombre (del cliente final) - Nº documento referencia IC En un chat sin historial, dime: - En qué tabla(s) de producción debo extender (Production Order Header). - Cómo sincronizar los campos desde Sales Header. - Qué evento usar para copiar los valores cuando la OP se genera desde el pedido. - Cómo hacer que se muestren en las 4 páginas correspondientes. Explícamelo paso a paso con AL.

### 7) **RF004** — Listados de Órdenes de Producción con esos campos 

- Es UI/listas; mejor cuando RF003 ya está resuelto.
		- Prompt: Quiero implementar el RF004 del documento UNNOX-F005E. Contexto: Los campos del RF003 deben mostrarse en los listados de: - Órdenes de producción planificadas - Planificadas en firme - Lanzadas - Terminadas En un chat sin historial explícame: - Qué páginas debo extender exactamente (sus nombres técnicos). - Cómo añadir columnas. - Si conviene agruparlos en un FastTab "Intercompany". - Cómo garantizar que son filtrables/ordenables. Dame los AL necesarios.

### 8) **RF007** — Albarán de venta: cabecera con datos corporativos de empresa vinculada seleccionable 

- Es reporte/request page, independiente del núcleo, pero mejor cuando el flujo ya genera albaranes estables.
		- Prompt: Este chat es para desarrollar el RF007 del documento UNNOX-F005E. Resumen contextual que necesitas: Desde Empresa B, cuando el usuario previsualiza o imprime el Albarán de Venta, debe existir en la Request Page un campo para elegir la empresa vinculada cuyos datos corporativos aparecerán en la cabecera del informe. Los datos a reemplazar vienen de Company Information: - Nombre, Dirección, C.P., Ciudad, Provincia, País/Región, Teléfono, Fax, NIF, Web, Email. En un chat sin historial explícame: - Qué report o layout extender. - Cómo crear un `requestpage` con un Lookup de empresa. - Cómo cargar la Company Information de la empresa elegida. - Cómo sustituir los campos de cabecera antes de renderizar el informe. Necesito detallado y con AL.

### 9) **RF008** — Al facturar en B, cerrar/recibir automáticamente el pedido compra en A 

- Es el más sensible por ser **cross-company + cantidades reales + parciales**: ideal dejarlo al final cuando todo lo anterior ya está sólido.
		- Prompt: Este chat es para implementar el RF008 del documento UNNOX-F005E. Contexto que debes recordar: Cuando la Empresa B registra la factura de venta, el sistema debe: 1) Localizar el Pedido de Compra vinculado en la Empresa A usando "Nº Documento Referencia IC". 2) Actualizar “Cantidad a recibir” según la cantidad realmente enviada. 3) Recibir automáticamente el Pedido de Compra en A (crear albarán de compra). En este chat sin contexto previo, necesito que me expliques: - En qué evento engancharme del registro de factura. - Cómo acceder a datos de otra empresa mediante CHANGECOMPANY. - Cómo actualizar las líneas del PO en A sin problemas de bloqueo. - Cómo ejecutar programáticamente el Receive. - Cómo manejar envíos parciales. Dame ejemplos AL claros.