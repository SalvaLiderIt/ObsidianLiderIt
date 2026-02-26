
Importante asignar boolean True: El almacén de consumo debe estar marcado en la tabla **Location** con la propiedad `"Almacen CONSUMO_LDR" = true`.

## 📋 Resumen de lo intentado hoy

### 🎯 Objetivo

Generar **movimientos de tipo "Consumo"** en Item Ledger Entries al ejecutar el botón "Recepción P.T" en la página Diario de Producción.

---
## 🔍 Lo que descubrimos

✅ **El código existente YA tiene lógica de consumo** en [SubComponentesSumatorioLDR.Page.al](vscode-file://vscode-app/c:/Program%20Files/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) (procedimiento `Registro()`, líneas 630-866)

✅ **El report `Calc. Consumption` SÍ crea líneas de diario**, pero las validaciones de BC las rechazan al registrar

✅ **Los movimientos de "Salida" SÍ se generan correctamente** - el problema es exclusivo del consumo

En qué momento se agregan en este código las líneas correspondientes con tipo movimiento salida en la pag item ledger entries? quiero ir a la raiz del asunto, es decir, si con el código actual tenemos ya en algún sitio fragmentos que actualizan esta página e incluyen líneas que tienen tipo movimiento Salida, por lógica ahí mismo tendrían que estar las líneas de código que agreguen las líneas correspondientes con tipo movimiento "consumo". Quizás es algo relacionado con la palabra "consumo" en el código?, alguna lógica que afecte a lo relacionado con esta palabra en cualquier otra página ya sea ficha producto por ejemplo?


Entry Type (4, Option) --> consumo --> Base Application (Pag, Item Ledger Entries....table, Item Ledger Entry)








**Resumen breve (qué se hizo y por qué funciona ahora)**

- **Se dejó de depender del report “Calc. Consumption”** y se **crean líneas de consumo manualmente**, igual que se hace con las de _Salida_.  
    _Motivo:_ el report no estaba generando líneas válidas y nunca llegaban al posting.
    
- **Se aseguró un `Line No.` único por línea** (10000, 20000, 30000…).  
    _Motivo:_ evitó errores de duplicado en el diario.
    
- **Se configuró correctamente el tipo de movimiento** (`Entry Type = Consumption`) y **la relación con la orden** (`Order Type`, `Order No.`).  
    _Motivo:_ sin esto, BC no reconoce el consumo como parte de la OPL.
    
- **Se usó `Location Code` y `Bin Code` coherentes con la orden** (mismo patrón que “Salida”).  
    _Motivo:_ BC valida bins/ubicaciones y bloquea si faltan.
    
- **Se registró el diario con `Item Jnl.-Post` filtrando por el documento**.  
    _Motivo:_ garantizó que solo se contabilicen las líneas del consumo de esa OPL.


Documentación para conseguir asignar símbolo negativo en movimientos de producto

El problema estaba en que **BC requiere que el componente tenga `Qty. Picked (Base)` actualizado** antes de registrar el consumo. Sin este campo, el sistema de warehouse management (WMS) bloqueaba el registro.

Cambios realizados:
Actualización de Qty. Picked en el componente (Líneas 71-73)
ProdOrderComp.Validate("Qty. Picked", ProdOrderComp."Cantidad Real Consumida_LDR");
ProdOrderComp.Validate("Qty. Picked (Base)", ProdOrderComp."Cantidad Real Consumida_LDR");
ProdOrderComp.Modify(true);

Asignación directa de cantidad positiva (Líneas 84-87)
ItemJnllineC."Quantity" := ProdOrderComp."Cantidad Real Consumida_LDR";
ItemJnllineC."Quantity (Base)" := ItemJnllineC."Quantity";
ItemJnllineC."Invoiced Quantity" := ItemJnllineC."Quantity";
ItemJnllineC."Invoiced Qty. (Base)" := ItemJnllineC."Quantity";

Flujo actual
    A[Componente con Cantidad Real Consumida] --> B[Actualizar Qty. Picked]
    B --> C[Crear línea de diario CONSUMO]
    C --> D[Validate Entry Type = Consumption]
    D --> E[Validate Item No.]
    E --> F[Validate Order references]
    F --> G[Validate Location/Bin]
    G --> H[Asignar Quantity POSITIVA]
    H --> I[Modify línea de diario]
    I --> J[Item Jnl.-Post invierte signo]
    J --> K[Item Ledger Entry con valores NEGATIVOS]


## Unnox Incidencia Reports 

pedido venta 
factura proforma
y factura de venta
revisar la extensión de los campos a nivel de línea y que se lleve el campo PRECIO correctamente Unnox. 
El que mejor está de los tres reports es pedido de venta

UX-F004-03-A Informe Factura Proforma y Factura Venta
UX-F004-04-A Informe Factura Venta

factura proforma venta