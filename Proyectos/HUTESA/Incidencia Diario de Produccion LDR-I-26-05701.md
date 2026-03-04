- Diario producción, probar con la fecha 30/01/2026, hay que quitar el "filtro" listado componentes para que agrupe todas las líneas de la orden del día filtrado
- Recepción P.T no se restan los componentes de la pag movs.productos. Programa Producción


Linea 261 DiariodeproduccionLDR

Pag Diario de Produccion_LDR(50003)
Tabla Production Order (5405) --> Standard

Pag a nivel de línea: Sub Componentes Sumatorio_LDR 50016
Tabla Prod. Order Component Agrupado 50022



## Workflow para crear un <mark style="background: #BBFABBA6;">diario de producción.</mark>
Para crear un diario de producción ir a <mark style="background: #FFB86CA6;">Programa de Producción</mark>, seleccionar semana/año/lina de producción. 
Del 23 al 25 de Febrero 2026, semana 9

<mark style="background: #FF5582A6;">Productos movs</mark> para ver los registros negativos al restar cantidad de componentes. Página Item Ledger Entries, tabla Item Ledger Entry




## Incidencia Diario de Produccion

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


