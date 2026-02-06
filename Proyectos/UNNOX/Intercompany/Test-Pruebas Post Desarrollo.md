# 🧪 CHECKLIST DE PRUEBAS POR RF

## **RF001 – Original Document No.**

☑ Crear pedido en Empresa A  
☑ Ver que se autocompleta  
☑ Enviar IC  
☑ Recibir en Empresa B y ver que llega bien  
☑ Ver que NO se puede modificar en B (bloqueo correcto)

## **RF002 – Listado**

☑ Abrir lista de pedidos de venta en B  
☑ Añadir columna → comprobar que aparece el campo

## **RF003 – Propagación a Production Order**

☑ Crear OP desde pedido de venta B  
☑ Revisar los 3 campos  
☑ Repetir en los 4 estados (Planificada, Firme, Lanzada, Terminada)

## **RF004 – Listados de OP**

☑ Abrir cada lista  
☑ Añadir las columnas personalizadas

## **RF005 – Drop Shipment automático**

☑ Crear venta en A  
☑ Usar item con Replenishment=Purchase y Vendor IC  
☑ Ver que se marca solo como Drop Shipment  
☑ Cambiar item → ver que NO se marca

## **RF006 – Proceso completo**

☑ Desde pedido de venta original  
☑ Acción RF006  
☑ Ver cargadas las líneas en Hoja Demanda  
☑ Ver Vendor autopuesto  
☑ Aceptar mensajes  
☑ Ejecutar mensajes  
☑ Ver que se genera 1 solo PO  
☑ Enviar IC  
☑ Comprobar presencia en empresa destino

## **RF007 – Información Empresa en albarán venta**

☑ Abrir informe  
☑ Seleccionar empresa vinculada  
☑ Previsualizar  
☑ Confirmar que aparece la cabecera correcta

## **RF008 – Recepción automática del PO en A**

➡ Este es el más importante de probar:

1. Desde Empresa B registrar factura de venta
2. Ver en Empresa A:
    - Purchase Order encontrado correctamente
    - Qty. to Receive actualizada según cantidades reales
    - Recepción creada automáticamente
    - Pedido queda parcialmente recibido si era parcial
    - Nuevo albarán de compra existe

Este RF es el más delicado y el que más bugs reales puede dar.

## **RF009 – Campos línea A→B**

☑ Procesar ciclo IC  
☑ Entrar en Sales Line de B  
☑ Comprobar que todos los campos custom llegan OK

# 🧹 5) Limpiezas pendientes

Sí, quedan cosas por limpiar:

### ✔ Mensajes debug

En RF006 tienes un `Message(...)` pendiente de eliminar.

### ✔ Regions sin usar o comentarios antiguos

Tienes un par de comentarios para RF006 que puedes borrar antes de entrega.


# 🟦 **PREPARACIÓN DEL ENTORNO (Obligatorio antes de cualquier prueba)**

### ✔ 1. Verificar que existen 3 empresas IC

En cada empresa abrir:

**Buscar → “Partners IC” → revisar:**

- Código del partner
- Empresa asociada
- Tipo inbox = **Database**

⚠ Si falta el Company Name no funcionará el RF008.

---

# 🟧 **BLOQUE 1 — RF001 + RF002 (Original Document No.)**

## 🎯 Objetivo

Asegurar que el pedido en Empresa A genera el “Original Document No.” y se propaga correctamente a Empresa B.

---

## **1.1 Crear un Pedido de Venta en Empresa A**

**Ruta:** Empresa A → Ventas → Pedidos de venta → “Nuevo”

**Qué hacer:**

- Crear pedido normal
- Poner cliente interno o real, da igual
- Añadir una línea de artículo

**Qué debe ocurrir:**

- Campo **Original Document No.** (nuestro campo)  
    → Se debe autocompletar con el nº del pedido.

---

## **1.2 Enviar el pedido por Intercompany**

En el pedido:

**Acción:**  
➡ “Enviar documento a empresas vinculadas”

**Qué debe ocurrir:**

- Se genera IC Outbox
- Empresa B recibe en su IC Inbox
- Se crea el pedido en Empresa B

---

## **1.3 Validar en Empresa B**

Abrir el pedido creado automáticamente.

**Comprobar:**

- El campo **Original Document No.** tiene el valor original de Empresa A
- Si intentas modificarlo → **debe dar error** (RF001 protección)

---

## **1.4 RF002**

Lista de pedidos de venta (Empresa B):  
➡ Añadir columna “Original Document No.”  
✔ Debe aparecer.

---

# 🟩 **BLOQUE 2 — RF003 + RF004 (Propagación a Producción)**

## 🎯 Objetivo

Ver que los 3 campos IC se pasan correctamente a las Órdenes de Producción.

---

## **2.1 Generar orden de producción**

En Empresa B:

Desde el pedido → “Crear acción producción” (o crea OP manualmente pero referenciada).

**Qué debe copiarse en la OP:**

- Original Document No
- Final Customer Name (Ship-to Name)
- IC Reference Document No

---

## **2.2 Validar en los 4 estados**

Cambiar estado de la orden:

- Planificada
- Planificada en firme
- Lanzada
- Terminada

En cada una: ✔ Los 3 campos deben mantenerse.

---

## **2.3 RF004**

En listas de:

- Órdenes planificadas
- Firme
- Lanzadas
- Terminadas

➡ Añadir columnas  
✔ Deben aparecer los 3 campos.

---

# 🟨 **BLOQUE 3 — RF005 (Drop Shipment automático)**

## 🎯 Objetivo

Marcar automáticamente el tick “Drop Shipment” en Empresa A cuando se cumplan las condiciones.

---

## **3.1 Configurar un artículo**

En Empresa A:

- Replenishment System = **Purchase**
- Vendor No = proveedor con **IC Partner Code**

---

## **3.2 Crear pedido de venta en Empresa A**

Añadir línea con el artículo anterior.

**Qué debe ocurrir:** ✔ Se marca automáticamente **Drop Shipment**  
✔ No pide Purchasing Code  
✔ No se marca en artículos equivocados/casos inválidos

---

# 🟪 **BLOQUE 4 — RF006 (Automatización Hoja de Demanda)**

## 🎯 Objetivo

Simular el proceso real completo que crea automáticamente el PO IC sin intervención humana.

---

## **4.1 En Empresa A, pedido original**

Debe tener:

- Líneas con artículo
- Drop Shipment marcado correctamente (RF005)

---

## **4.2 Ejecutar acción RF006**

La acción tuya personalizada debe:

### ✔ (Paso 1)

Crear líneas en **Hoja de Demanda** → Template “REQUISICIO”, Batch “DEFAULT”

### ✔ (Paso 2)

Rellenar automáticamente “Vendor No.” según el artículo

### ✔ (Paso 3)

Aceptar mensajes de acción  
y luego  
Ejecutarlos  
→ Se crea automáticamente **un solo Purchase Order**

### ✔ (Paso 4)

Buscar el PO recién creado  
Y enviarlo automáticamente por IC

---

## **4.3 Validar en Empresa B**

Debe aparecer un:

- Purchase Order (pedido de compra)
- Vendor = empresa A (IC partner)
- Lineas correctas
- Campos personalizados de RF009 sin aún validar (eso es RF009)

---

# 🟥 **BLOQUE 5 — RF007 (Cabecera del informe Albarán Venta)**

## 🎯 Objetivo

Que el albarán de venta de B muestre info corporativa de A.

---

## **5.1 Abrir un albarán de venta en Empresa B**

Acción → Imprimir

En la request page:

- Seleccionar “Empresa vinculada” = Empresa A

**Qué debe ocurrir:** ✔ La cabecera del informe usa la información de Empresa A  
(No la de Empresa B)

---

# 🟦 **BLOQUE 6 — RF009 (Propagación de campos personalizados A→B)**

## 🎯 Objetivo

Comprobar que tus campos custom viajan a través del circuito IC.

---

## **6.1 Modificar campos custom en Empresa A**

En las líneas del pedido original, ajustar:

- APQ Warehouse
- ADR
- Tipo de paletización
- Nº butlos
- Tipo embalaje
- Fechas planificadas

---

## **6.2 Enviar IC y recibir en Empresa B**

El pedido debe crearse en B.

**Comprobar en Sales Lines de B:** ✔ Todos los campos deben aparecer con su valor  
✔ Método de copia = RF009 (OnAfterInsert de Sales Line)

---

# 🟧 **BLOQUE 7 — RF008 (Recepción automática del PO en empresa A)**

_(El bloque más técnico y más crítico)_

## 🎯 Objetivo

Al registrar una factura de venta en Empresa B:

1. Encontrar el PO original en A
2. Ajustar Qty. to Receive según la cantidad realmente enviada
3. Registrar automáticamente la recepción del PO en A

---

## **7.1 Preparar pedido de venta en Empresa B**

Debe tener:

- IC Document Reference No (procedente del proceso anterior)
- Líneas con cantidades

---

## **7.2 Registrar la factura de venta**

Empresa B → Acciones → Registrar → Factura

**Qué debe ocurrir internamente:**

### ✔ RF008 Step 1

El evento OnAfterPostSalesDoc se ejecuta

### ✔ RF008 Step 2

Busca dinámicamente la empresa correcta según IC Partner:

- Inbox Type = Database
- Company Name ≠ vacío
- Debe encontrar un Purchase Order con nº = ICRefNo

### ✔ RF008 Step 3

Recorrer todas las Sales Invoice Lines  
Tomar `"Quantity (Base)"`

### ✔ RF008 Step 4

Ir Purchase Line por Purchase Line en Empresa A  
Ajustar:

`Qty. to Receive = min(Quantity Base enviada, Outstanding Qty. Base)`

### ✔ RF008 Step 5

Llamar Purch.-Post en modo Receive only  
Genera un **Posted Purchase Receipt** en Empresa A

---

## **7.3 Comprobaciones en Empresa A**

### **A) Purchase Header**

- Receive = TRUE
- Invoice = FALSE
- Qty. to Receive = 0 en líneas afectadas
- Qty. Received actualizado correctamente
- Si el envío era parcial → Outstanding Qty. disminuido pero no cero

### **B) Posted Purchase Receipt**

Debe existir un documento:

- Tipo: Albarán de compra
- Misma numeración del PO transformado
- Líneas con cantidades recibidas correctas

---

## **7.4 Prueba de envíos parciales**

Prueba imprescindible:

1. Desde B → enviar solo parte (p.ej. 10 de 25)
2. Registrar factura parcial
3. A debe recibir solo 10
4. Outstanding Qty. → 15
5. Repetir operación
6. Ver que al final llega a 0 y el pedido queda totalmente recibido

---

# 🟩 **BLOQUE FINAL — LIMPIEZA Y REGRESIÓN**

## ✔ Eliminar mensajes de debug de RF006

## ✔ Verificar que ningún RF interfiere con otro

## ✔ Probar circuito A → B y B → A (dos direcciones)

## ✔ Probar con 2 empresas y con 3 empresas

## ✔ Probar artículos sin Replenishment Purchase (para ver que RF005 no actúa)

## ✔ Probar que no se rompen documentos locales (no IC)

Producto a probar --> FP000802121 

### Prompt para nuevo chat o modo Plan en proceso de pruebas
- **TITLE: Revisar y corregir errores del desarrollo Intercompany UNNOX (runtime 15.1)**

**CONTEXT:**  
Estoy desarrollando para el cliente UNNOX un flujo Intercompany completo entre empresas A → B, definido en el documento funcional **UNNOX-F005E Intercompany (Tercer Formato)**.  
El proyecto ya está enteramente desarrollado y ahora estoy en **fase de pruebas**.  
Todo el desarrollo está concentrado en la **codeunit 50008 Intercompany_LDR**, más extensiones de tabla y página ya finalizadas.

El **runtime ES OBLIGATORIAMENTE 15.1** y **no puedo crear nuevos objetos** ni añadir dependencias.  
Todo debe funcionar usando objetos estándar + extensiones ya creadas.

**ORDER OF FEATURES IMPLEMENTED:**

1. RF001 — Campo “Original Document No.” en Sales Header + viajar por IC
2. RF009 — Traspaso de campos de Sales Line A→B
3. RF005 — Auto "Drop Shipment" en Sales Line
4. RF006 — Acción que automatiza Demand Worksheet + messages + creación PO + envío IC
5. RF002 — Mostrar “Original Document No.” en lista pedidos venta de B
6. RF003 — Campos IC en Production Orders + propagación automática
7. RF004 — Listas de Production Orders mostrando esos campos
8. RF007 — Albarán de Venta con cabecera dinámica según empresa vinculada
9. RF008 — Al facturar en B, cerrar automáticamente el PO en A

**CURRENT PHASE:**  
– Todo el código está hecho y compila sin errores.  
– Estoy realizando **pruebas reales dentro de BC**.  
– Se están detectando errores funcionales: flujos incompletos, configuraciones que faltan o validaciones que no detecté.  
– Necesito que revises el flujo completo RF006–RF008 y me indiques **qué está fallando, en qué paso, por qué y cómo corregirlo**, manteniendo siempre compatibilidad con runtime 15.1.

**IMPORTANT RESTRICTIONS**  
– No inventes eventos estándar que no existen.  
– No pidas crear nuevas codeunits.  
– No pidas dependencias.  
– Si es configuración de BC, indícalo expresamente.  
– Si puedes corregir mi código, hazlo de forma incremental y segura.  
– Prioriza estabilidad frente a optimización.

**INPUT TO ANALYZE (VERY IMPORTANT):**

1. Todo el código que te proporcione de la codeunit 50008 Intercompany_LDR
2. Pageextensions del RF006
3. Tableextensions de RF001, RF009 y RF003
4. Report 50003 del RF007
5. El PDF funcional que define el flujo (UNNOX-F005E)

**YOUR GOAL:**

1. Identificar los errores que aparecen en las pruebas reales.
2. Determinar si son de código, de configuración o de flujo funcional.
3. Proponer corrección exacta línea por línea cuando aplique.
4. Asegurar que el flujo final cumpla EXACTAMENTE los RF del documento.
5. Mantener runtime 15.1.
6. No simplificar ni rescribir en exceso: corregir sin romper partes ya validadas.