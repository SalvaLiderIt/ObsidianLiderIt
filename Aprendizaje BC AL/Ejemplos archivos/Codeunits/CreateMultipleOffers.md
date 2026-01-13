
## 🧩 Codeunit 50100 `CreateMultipleOffers` – Generación de Pedido de Compra desde múltiples líneas seleccionadas

### 🎯 Propósito
Este codeunit permite crear un **Purchase Order** automáticamente a partir de varias líneas seleccionadas de **Purchase Quotes**. Es útil cuando se han recibido múltiples ofertas del mismo proveedor y se desea consolidarlas en un único pedido.

---

### ⚙️ Procedimientos clave

#### 🔹 `ValidateSelectedLines`
- Filtra las líneas marcadas como seleccionadas (`SelectLines = true`) y del tipo `Quote`.
- Verifica que:
  - Hay al menos una línea seleccionada.
  - Todas las líneas pertenecen al mismo proveedor.
- Devuelve el número de líneas válidas y extrae el `VendorNo` y `DocumentNo` de referencia.

#### 🔹 `CreatePurchaseOrderHeader`
- Crea una nueva cabecera de pedido (`Purchase Header`) de tipo `Order`.
- Copia datos relevantes desde la primera oferta (`Quote`) como fechas y proveedor.
- Inserta y modifica el nuevo registro.

#### 🔹 `CopySelectedLinesToOrder`
- Recorre las líneas seleccionadas de la oferta.
- Crea nuevas líneas en el pedido (`Purchase Line`) copiando:
  - Tipo, número, cantidad, coste unitario.
  - Descripciones, unidad de medida, ubicación.
- Asigna `Line No.` incrementando por 10000.

#### 🔹 `CreateOorderFromSelectedLines`
- Orquesta el proceso completo:
  1. Valida las líneas seleccionadas.
  2. Crea la cabecera del pedido.
  3. Copia las líneas.
  4. (Opcional) Elimina las líneas originales.
  5. Muestra mensaje de éxito y abre el pedido.

---

### 🛠️ Permisos declarados

```al
Permissions =
    tabledata "Purchase Header" = RIMD,
    tabledata "Purchase Line" = RIMD;
```


### 📌 Usos prácticos

- Consolidar múltiples líneas de oferta en un solo pedido.
- Automatizar la creación de pedidos desde una selección manual.
- Evitar errores humanos al copiar datos entre documentos.
- Integrar esta lógica con un botón en la página de líneas de oferta (`Offered Purchase Lines`).


### 💡 Casos genéricos de aplicación

- Gestión de compras en empresas con múltiples cotizaciones por proveedor.
- Procesos de aprobación donde se seleccionan líneas antes de generar el pedido.
- Extensiones personalizadas que simplifican el flujo de compras en Business Central.


**Ejemplo Completo de la Codeunit

```al
codeunit 50100 CreateMultipleOffers
{
    Permissions =
        tabledata "Purchase Header" = RIMD,
        tabledata "Purchase Line" = RIMD;

    local procedure ValidateSelectedLines(var PurchaseLine: Record "Purchase Line"; var VendorNo: Code[20]; var FirstDocumentNo: Code[20]): Integer
    var
        ErrorMsg: Label 'No lines selected. Please select at least one line.';
        ErrortwoMsg: Label 'All selected lines must be from the same vendor.';
        LineCount: Integer;
    begin
        // Filtrar solo las líneas seleccionadas
        PurchaseLine.SetRange(SelectLines, true);
        PurchaseLine.SetRange("Document Type", PurchaseLine."Document Type"::Quote);

        // Validar que hay líneas seleccionadas
        if not PurchaseLine.FindSet() then
            Error(ErrorMsg);

        // Contar líneas y obtener el proveedor
        LineCount := PurchaseLine.Count();
        VendorNo := PurchaseLine."Buy-from Vendor No.";
        FirstDocumentNo := PurchaseLine."Document No.";
  
        // Validar que todas las líneas seleccionadas sean del mismo proveedor
        PurchaseLine.Reset();
        PurchaseLine.SetRange(SelectLines, true);
        PurchaseLine.SetFilter("Buy-from Vendor No.", '<>%1', VendorNo);
        if not PurchaseLine.IsEmpty() then
            Error(ErrortwoMsg);
        exit(LineCount);
    end;
  
    local procedure CreatePurchaseOrderHeader(VendorNo: Code[20]; FirstDocumentNo: Code[20]): Record "Purchase Header"
    var
        PurchaseHeader: Record "Purchase Header";
        NewPurchaseHeader: Record "Purchase Header";
    begin
        // Obtener la cabecera de la primera oferta para copiar datos
        if PurchaseHeader.Get(PurchaseHeader."Document Type"::Quote, FirstDocumentNo) then begin

            // Crear la nueva cabecera del pedido
            NewPurchaseHeader.Init();
            NewPurchaseHeader."Document Type" := NewPurchaseHeader."Document Type"::Order;
            NewPurchaseHeader.Insert(true);
  
            // Validar el proveedor
            NewPurchaseHeader.Validate("Buy-from Vendor No.", VendorNo);
            // Copiar otros campos relevantes
            NewPurchaseHeader."Posting Date" := WorkDate();
            NewPurchaseHeader."Order Date" := WorkDate();
            NewPurchaseHeader.Modify(true);
        end;
        exit(NewPurchaseHeader);
    end;
  
    local procedure CopySelectedLinesToOrder(var NewPurchaseHeader: Record "Purchase Header")
    var
        PurchaseLine: Record "Purchase Line";
        NewPurchaseLine: Record "Purchase Line";
    begin

        // Resetear filtros y obtener líneas seleccionadas
        PurchaseLine.Reset();
        PurchaseLine.SetRange(SelectLines, true);
        PurchaseLine.SetRange("Document Type", PurchaseLine."Document Type"::Quote);

        if PurchaseLine.FindSet() then
            repeat
                // Crear nueva línea en el pedido
                NewPurchaseLine.Init();
                NewPurchaseLine."Document Type" := NewPurchaseHeader."Document Type";
                NewPurchaseLine."Document No." := NewPurchaseHeader."No.";
                NewPurchaseLine."Line No." := NewPurchaseLine."Line No." + 10000;
                NewPurchaseLine.Insert(true);

                // Copiar campos principales
                NewPurchaseLine.Validate(Type, PurchaseLine.Type);
                NewPurchaseLine.Validate("No.", PurchaseLine."No.");
                NewPurchaseLine.Validate(Quantity, PurchaseLine.Quantity);
                NewPurchaseLine.Validate("Direct Unit Cost", PurchaseLine."Direct Unit Cost");

                // Copiar otros campos relevantes
                NewPurchaseLine.Description := PurchaseLine.Description;
                NewPurchaseLine."Description 2" := PurchaseLine."Description 2";
                NewPurchaseLine."Unit of Measure Code" := PurchaseLine."Unit of Measure Code";
                NewPurchaseLine."Location Code" := PurchaseLine."Location Code";

                NewPurchaseLine.Modify(true);
            until PurchaseLine.Next() = 0;
    end;

  
    procedure CreateOorderFromSelectedLines()
    var
        PurchaseLine: Record "Purchase Line";
        NewPurchaseHeader: Record "Purchase Header";
        PageManagement: Codeunit "Page Management";
        VendorNo: Code[20];
        LineCount: Integer;
        FirstDocumentNo: Code[20];

    begin

        // 1. Validar líneas seleccionadas
        LineCount := ValidateSelectedLines(PurchaseLine, VendorNo, FirstDocumentNo);

        // 2. Crear cabecera del pedido
        NewPurchaseHeader := CreatePurchaseOrderHeader(VendorNo, FirstDocumentNo);

        // 3. Copiar líneas al pedido
        CopySelectedLinesToOrder(NewPurchaseHeader);

        // 4. (Opcional) Eliminar líneas originales - comentado para pruebas
        // DeleteOriginalLines();

        // 5. Mostrar mensaje y abrir pedido
        Message('Purchase Order %1 created successfully with %2 lines.', NewPurchaseHeader."No.", LineCount);
        PageManagement.PageRun(NewPurchaseHeader.RecordId());
    end;

}
```

