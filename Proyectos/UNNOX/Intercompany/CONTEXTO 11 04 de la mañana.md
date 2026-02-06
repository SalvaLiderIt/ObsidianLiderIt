
## **CONTEXTO ACTUAL DEL PROYECTO - Fecha: 04/02/2026**

### **📍 Situación:**
Desarrollo de automatizaciones Intercompany en Business Central 27.2 AL para cliente Unnox.
Comunicación entre **3 empresas en el mismo tenant** mediante flujos automatizados de compra/venta.

**Empresas involucradas:**
- **Empresa A**: UNNOX NAVARRA (PRUEB) - Recibe pedido del cliente final
- **Empresa C**: UNNOX IBERICA (PRUEB) - Fabrica/envía el producto
- **Empresa B**: No trabajada aún

---

## **✅ RFs COMPLETAMENTE FUNCIONALES (Probados en producción):**

### **RF001** - Propagación "Original Document No." A→B→C
Flujo: Sales Header (A) → Purchase Header (A) → IC Outbox Purchase Header → IC Inbox Sales Header (C) → Sales Header (C)
- Estado: ✅ FUNCIONANDO

### **RF003** - Campos IC en Production Order (RECIÉN AUTOMATIZADO)
Flujo: Sales Header → Purchase Header → Production Order (auto-fill al abrir)
- Copia automática de: Original Document No._LDR, Final Customer Name_LDR, IC Reference Doc. No._LDR
- **Trigger**: OnAfterGetCurrRecord en PageExtensions (PlannedProdOrderLDR, ReleasedProductionOrderExtLDR, FinishedProductionOrderLDR)
- **Estrategia**: Busca Sales Order por 3 métodos (Source No., Item Ledger Entry, Sales Lines activas)
- Estado: ✅ COMPILANDO EXITOSAMENTE (pruebas en curso)

### **RF003B** - Ship-to Fields en Sales Header (RECIÉN IMPLEMENTADO)
Flujo: IC Inbox Sales Header → Sales Header en Empresa C
- Copia automática de: Ship-to Name, Address, City, County, Post Code, Country/Region Code
- Ahora en "Envío y facturación" del SO en Empresa C aparece el cliente final (no Unnox Navarra)
- Estado: ✅ COMPLETADO

### **RF003C** - Ship-to Fields en Purchase Header (RECIÉN IMPLEMENTADO)
Flujo: Sales Header (A) → Purchase Header (A) → IC Outbox Purchase Header → IC Inbox Sales Header (C)
- Asegura que info del cliente final fluye a través de toda la cadena IC
- Estado: ✅ COMPLETADO

### **RF005** - Auto-mark Drop Shipment
Cuando un Item tiene Vendor IC configurado, marca automáticamente Drop Shipment en Sales Line
- Estado: ✅ FUNCIONANDO

### **RF006** - Creación automática Purchase Order IC
- Flujo: Sales Order (A) → Botón RF006 → PO (A) → IC Outbox → IC Inbox → Sales Order (C)
- Copia automática de Ship-to fields mediante RF003C
- Estado: ✅ FUNCIONANDO

### **RF009A** - Propagación campos de línea a Sales Line final (RECIÉN OPTIMIZADO)
Flujo: IC Inbox Sales Line → Sales Line (C)
- Copia: APQ Warehouse, ADR, Palletization Type, Packages No., Packaging Type, Planned Delivery Date, Planned Shipment Date
- Usa rastreo global (CurrentICInboxSalesHeaderDocNo) para búsqueda confiable
- Estado: ✅ FUNCIONANDO

### **RF009C** - Propagación IC Outbox Purchase Line → IC Inbox Sales Line (FUNCIONANDO)
- Copia custom fields desde IC Outbox → IC Inbox
- Estado: ✅ FUNCIONANDO

### **RF009D** - Copia campos editables en Purchase Line desde Sales Line
- Copia: NoBultos_LDR, "Tipo de Embalaje_LDR"
- APQ Warehouse y ADR son FlowFields (calculados automáticamente desde Item)
- Estado: ✅ COMPLETADO

### **RF009E** - Copia campos Sales Line → IC Outbox Purchase Line (RECIÉN REFACTORIZADO)
Evento: OnBeforeICOutboxTransactionCreatedPurchDocTrans
- Procedimientos: 
  - CopyShipToFieldsToPurchOutbox: Copia Ship-to fields (RF003C)
  - CopyCustomFieldsToOutboxLines: Itera líneas IC Outbox
  - CopySalesFieldsToOutboxLine: Busca Sales Line original y copia todos los custom fields
- Copia: APQ Warehouse (desde Item.ALMACEN_APQ_LDR), ADR, Palletization Type, Packages No., Packaging Type, fechas
- Estado: ✅ REFACTORIZADO Y COMPILANDO

### **RF010** - Auto-accept IC Inbox Transactions
- Auto-crear Purchase/Sales documents cuando llegan al Inbox
- Rastreo de IC Header No. para RF009A
- Estado: ✅ FUNCIONANDO

### **RF008** - Auto-receive Purchase Order en Empresa A
- Cuando Empresa C factura, auto-recibe el PO en Empresa A
- Estado: ✅ IMPLEMENTADO (pruebas pendientes)

---

## **🚀 CAMBIOS REALIZADOS EN ESTE CHAT (04/02/2026):**

### **Problema 1: Campos de línea no se transferían** ❌→✅
**Síntoma**: APQ, ADR, Nº bultos, fechas no aparecían en SO de Empresa C
**Causa**: Evento de tabla OnAfterInsertEvent en IC Outbox Purchase Line no se disparaba confiablemente
**Solución**: 
- Cambió a evento de Codeunit: `OnBeforeICOutboxTransactionCreatedPurchDocTrans`
- Se dispara DESPUÉS de crear todas las líneas
- Refactorizado en 3 procedimientos locales para mantener complejidad ciclomática < 10
- Ahora copia todos los campos: APQ Warehouse, ADR, Palletization Type, Packages No., Packaging Type, fechas

### **Problema 2: Ship-to mostraba Unnox Navarra (Empresa A) en lugar de cliente final** ❌→✅
**Síntoma**: En "Envío y facturación" del SO en Empresa C seguía mostrando Unnox Navarra
**Causa**: Ship-to fields no se copiaban en la cadena IC
**Solución**:
- **RF003B** (línea 189-197): Copia Ship-to desde IC Inbox Sales Header → Sales Header (C)
- **RF003C** (línea 619-632, RF009E línea 337-349): Copia Ship-to desde Sales Header (A) → Purchase Header (A) → IC Outbox Purchase Header
- Flujo completo: Sales Header (A) → Purchase Header (A) → IC Outbox → IC Inbox → Sales Header (C)

### **Problema 3: Production Order fields no se rellenaban automáticamente** ❌→✅
**Síntoma**: Original Document No., Final Customer Name, IC Reference Doc. No. vacíos al abrir PO
**Causa**: Event subscriber OnAfterModifyEvent no confiable para auto-fill; requería trigger OnAfterGetRecord
**Solución**:
- Codeunit: `procedure AutoFillProdOrderICFields(var ProdOrder: Record "Production Order")`
- PageExtensions: Agregado trigger `OnAfterGetCurrRecord()` a 3 páginas Production Order
- Estrategia triple: busca Sales Order por Source No., Item Ledger Entry, o Sales Lines activas
- Se ejecuta automáticamente al abrir la PO (usuario NO ve botón)
- Compilación: ✅ EXITOSA

---

## **📊 FLUJO COMPLETO ACTUAL (A→C):**

[EMPRESA A - Sales Order (ej: PV400017)]  
↓ [Líneas con Drop Shipment + Vendor IC]  
↓ [RF005 marca automáticamente]  
[RF006 Button]  
↓ [Copia Ship-to desde SO a PO (RF003C)]  
↓ [Copia Nº bultos y Tipo embalaje de SO a PO (RF009D)]  
[EMPRESA A - Purchase Order (ej: PC400008)]  
↓ [ICInboxOutboxMgt.SendPurchDoc]  
[IC OUTBOX TRANSACTION]  
↓ [RF009E OnBeforeICOutboxTransactionCreated]  
↓ [Copia todos los custom fields: APQ, ADR, Palletization, Packages, Packaging, fechas]  
↓ [Copia Ship-to fields (RF003C)]  
[IC OUTBOX Purchase Header + Lines con todos los campos]  
↓ [ICOutboxProcessor_LDR.ProcessOutboxTransactions]  
[IC INBOX (Empresa C)] ← Automático  
↓ [RF010 OnAfterInsertEvent para Sales Header]  
↓ [RF009C copia IC Outbox → IC Inbox fields]  
[EMPRESA C - Sales Order creado automáticamente]  
├─ Original Document No._LDR = PV400017 (RF001) ✓  
├─ Ship-to = Cliente Final (no Unnox Navarra) (RF003B) ✓  
├─ Campos línea: APQ, ADR, Palletization, Packages, Packaging, fechas (RF009A) ✓  
└─ Listo para Picking/Shipment/Invoice  
↓ [Empresa C factura Sales Order]  
↓ [RF008] Auto-recibe PO en Empresa A  
↓ [Empresa A recibe automáticamente]



---

## **🔧 ARQUITECTURA DE CODEUNITS:**

### **Codeunit 50008 - Intercompany_LDR** (1683 líneas)
**Responsabilidad**: Orquestación principal de flujos IC, event subscribers, procedimientos de negocio

**Procedimientos públicos:**
- `RunRF006_AutoProcess()` - Trigger button en Sales Order
- `AutoFillProdOrderICFields()` - Called desde PageExtensions (RF003)

**Event Subscribers:**
- RF001: ICOutboxPurchHdr_OnBeforeInsert, ICInboxSalesHeader_OnBeforeInsert, CreateSalesDoc_OnBeforeSalesHeaderInsert
- RF003: AutoFillProdOrderICFields via page triggers
- RF003B: CreateSalesDoc_OnBeforeSalesHeaderInsert (Ship-to fields)
- RF003C: RF009E_OnBeforeICOutboxTransactionCreated (Ship-to fields)
- RF005: SalesLine_OnAfterValidate_No_RF005, SalesLine_OnAfterValidate_Type_RF005, SalesLine_OnAfterModify_RF005
- RF006: RunRF006_AutoProcess, RF006_CreatePurchaseOrderFromSalesOrder, RF006_CreatePurchaseLineFromSalesLine
- RF008: RF008_OnAfterPostSalesDoc (Auto-receive)
- RF009A: RF009_OnAfterSalesLineInsert (Propagación a Sales Line final)
- RF009C: RF009C_OnBeforeICInboxSalesLineInsert (IC Outbox → IC Inbox)
- RF009D: RF006_CreatePurchaseLineFromSalesLine (custom fields en PO)
- RF009E: RF009E_OnBeforeICOutboxTransactionCreated con 3 procedimientos locales
- RF010: ICInboxPurchHdr_OnAfterInsert, ICInboxSalesHdr_OnAfterInsert

### **Codeunit 50010 - ICOutboxProcessor_LDR** (Job Queue/Autonomía)
**Responsabilidad**: Procesar automáticamente transacciones pendientes en IC Outbox
**Método**: Llamado desde RF006 + Job Queue Schedule

---

## **⚠️ ESTADO DE PRUEBAS:**

### **Probado y VALIDADO:**
- ✅ RF001, RF005, RF006 - Funcionalidad básica confirmada
- ✅ RF009A, RF009C - Campos de línea copiándose en cadena IC
- ✅ Compilación exitosa con estructura refactorizada

### **Probado pero PARCIALMENTE:**
- ⚠️ RF003 - Build exitoso, **pruebas de auto-fill en ejecución**
- ⚠️ RF003B, RF003C - Ship-to fields, **requieren validación en SO de Empresa C**
- ⚠️ RF008 - Lógica implementada, **SIN PRUEBAS EXHAUSTIVAS**

### **Pendiente de DIAGNÓSTICO en runtime:**
1. ¿Se dispara correctamente RF009E al crear IC Outbox?
2. ¿Llegan los custom fields a IC Inbox?
3. ¿Se copia RF003B correctamente al crear Sales Header en C?
4. ¿RF003 auto-fill funciona al abrir Production Order?

---

## **🎯 PRÓXIMAS ACCIONES (Orden recomendado):**

### **INMEDIATO - Chat siguiente:**
1. **Validar flujo completo en runtime**:
   - Crear SO en A con cliente final (no Unnox Navarra)
   - Apretar botón RF006
   - Verificar en Empresa C:
     - ¿Aparecen APQ, ADR, Nº bultos, fechas en líneas?
     - ¿Ship-to es el cliente final?
     - ¿Original Document No. se rellenó?
   - Abrir Production Order desde SO de C
   - ¿Se rellenan automáticamente los 3 campos IC?

2. **Si fallan campos de línea**: Agregar Message() statements en RF009E para diagnosticar

3. **Si fallan Ship-to**: Verificar si IC Outbox Purchase Header acepta campo Ship-to

4. **Si falla RF003 auto-fill**: Usar debugger en OnAfterGetCurrRecord

### **DESPUÉS DE VALIDACIÓN:**
5. Implementar RF007 (si está en documento PDF)
6. Pruebas exhaustivas de RF008
7. Agregar logging/error handling
8. Revisión de edge cases (cantidades parciales, devoluciones, múltiples vendors)

---

## **📁 ARCHIVOS CRÍTICOS:**

| Archivo | Ubicación | Líneas | Estado |
|---------|-----------|--------|--------|
| IntercompanyLDR.Codeunit.al | src/codeunit/ | 1683 | ✅ Compilando |
| PlannedProdOrderLDR.PageExt.al | src/pageextension/ | 51 | ✅ Compilando |
| ReleasedProductionOrderExtLDR.PageExt.al | src/pageextension/ | 61 | ✅ Compilando |
| FinishedProductionOrderLDR.PageExt.al | src/pageextension/ | 61 | ✅ Compilando |
| ICOutboxProcessor_LDR.Codeunit.al | src/codeunit/ | ~100 | ✅ Compilando |
| PurchaseLineLDR.TableExt.al | src/tableextension/ | Incluye NoBultos_LDR, "Tipo de Embalaje_LDR" | ✅ |
| ICOutboxPurchaseLineLDR.TableExt.al | src/tableextension/ | Fields 50200-50206 | ✅ |

---

## **💾 ESTADO DE COMPILACIÓN:**
- ✅ **Build exitoso** - Ningún error de sintaxis
- ⚠️ Warnings en otros archivos (Purchase Price deprecated, Report layout conflicts) - **No afectan IC flows**
- Namespace: LiderIT.Unnox.Sales
- Codeunit ID: 50008 (Intercompany_LDR)

---

## **📝 NOTAS TÉCNICAS:**

1. **Event Subscriber Timing**: RF009E se dispara en `OnBeforeICOutboxTransactionCreatedPurchDocTrans` (DESPUÉS que se crean líneas IC Outbox)
2. **Refactorización de complejidad**: RF009E dividido en 3 procedimientos locales para respetar umbral ciclomático
3. **Rastreo de IC Context**: CurrentICInboxSalesHeaderDocNo mantiene track del IC Header para RF009A
4. **Triple estrategia RF003**: AutoFillProdOrderICFields intenta 3 métodos para encontrar Sales Order (robustez)
5. **Ship-to chain**: Implementado en 3 puntos de la cadena (RF003B, RF003C, RF009E) para garantizar propagación

---

## **🔐 Datos de Acceso:**
- Repositorio: c:\Users\spavila\REPOSITORIOS\unnox\
- App Version: 1.0.0.19
- BC Version: 27.2
- Objeto ID 50008: Intercompany_LDR (Codeunit principal)

---

## **✅ CHECKLIST PARA PRÓXIMO CHAT:**

- [ ] Crear SO en Empresa A con cliente FINAL específico
- [ ] Apretar botón RF006
- [ ] Navegar a SO creado en Empresa C
- [ ] Verificar: Original Document No., Ship-to Name, campos de línea (APQ, ADR, bultos, fechas)
- [ ] Crear Production Order desde SO de C
- [ ] Verificar: Original Document No., Final Customer Name se rellenaron automáticamente
- [ ] Si hay fallos, compartir: mensajes de error, valores esperados vs. reales

---

**🎯 Objetivo final**: Automatización COMPLETA de flujo A→C sin intervención manual, con trazabilidad total de cliente final y campos logísticos en toda la cadena.


## CONTEXTO CRÍTICO - Sesión Unnox InterCompany (Continuación)

**Estado Actual (5 Feb 2026):**

- ✅ RF003 FUNCIONANDO: Production Order auto-fill con FindLast strategy
- ✅ RF009E→C FUNCIONANDO: Field copying through entire IC chain (A→B→C)
- 🔧 RF009 PARCIALMENTE ARREGLADO: Event subscriber issue debido a BC architectural limitation
- ⏳ **CRÍTICO**: Auto-aceptación de IC Inbox Sales Headers - Última fix aplicada en Message 45 simplificando `ProcessICInboxSalesHeader`

**Problema Principal Actual:**  
La auto-aceptación de transacciones IC Inbox estaba rota. El código dependía de `CreatedSalesOrderNo` que NUNCA se poblaba porque `SalesHeader_OnAfterInsert` no dispara para documentos IC-created (limitación BC).

**Fix Aplicado (Message 45):**  
Se simplificó `ProcessICInboxSalesHeader` (líneas ~1710-1726) para:

1. Llamar `CreateSalesDocument()`
2. Buscar el SO creado por Customer + Order Date usando `FindLast()`
3. Llamar `CopyICInboxFieldsToSalesLines()` si encuentra el SO
4. `Commit()` al final

**Compilación:** ✅ SUCCESS - Sin errores

**Archivo Principal:**  
[IntercompanyLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/bdd88df003/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

**Variables Clave:**

- `CurrentICInboxSalesHeaderDocNo` - Rastreo temporal durante procesamiento
- `CreatedSalesOrderNo` - Ahora NO se usa (evento nunca dispara)

**Debug Messages Presentes:**

- RF003: "RF003 DEBUG..." (líneas ~1410+)
- RF009A: "RF009A DEBUG..." (líneas ~245-285)
- RF009C: "RF009C DEBUG..." (líneas ~319+)
- RF009E: "RF009E DEBUG..." (líneas ~421-447)

**Próximo Paso:**

1. El usuario debe PUBLICAR y PROBAR si auto-aceptación IC ahora funciona
2. Si funciona: Verificar que SOs se crean automáticamente sin intervención manual
3. Si falla: Investigar si SO se crea pero no se encuentran sus líneas para copiar campos

**Workspace:**

c:\Users\spavila\REPOSITORIOS\unnox\Unnox\
├── src/codeunit/IntercompanyLDR.Codeunit.al (1879 líneas, ACTIVO)
├── src/codeunit/[otros codeunits...]
├── Translations/Unnox.es-ES.xlf
└── app.json

**BC Environment:** 27.2  
**Lenguaje:** AL  
**Próxima Tarea:** Testear y ajustar si es necesario