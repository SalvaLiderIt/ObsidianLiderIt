
# Patrón genérico: Copiar descripción desde otra tabla en AL

## 📌 Idea principal

Cuando tenemos un **código** en una tabla (ej. `ReturnReason.Code`) y queremos mostrar también su **descripción** en otra tabla/página (ej. `Purchase Header`), usamos la lógica:


```
if OtherTableRec.Get(SomeCode) then
    Rec.Validate(TargetField, OtherTableRec.Description);
```

Esto permite:

- **Sincronizar campos** entre tablas relacionadas.
- **Mostrar información no editable** en páginas (`Page` o `PageExtension`).
- **Evitar duplicar lógica** en el cliente: se resuelve en el servidor AL.

## 🛠️ Contextos de uso

- **Dentro de triggers**:
    - `OnValidate` → cuando el usuario selecciona un código, se rellena automáticamente la descripción.
    - `OnAfterGetRecord` → al abrir la página, se asegura que la descripción esté sincronizada.
- **En PageExtension** → para mostrar campos calculados o no editables.
- **En TableExtension** → para guardar la descripción en la tabla base y tener trazabilidad.
    

## 🧩 Ejemplo 1: En un `OnValidate` de PageExtension

```
trigger OnValidate()
var
    ReasonRec: Record "Return Reason";
begin
    if ReasonRec.Get(pTipoIncidencia) then
        Rec.Validate(DescripcionIncidencia_LDR, ReasonRec.Description);

    Rec.Modify(true);
end;
```

👉 Uso: Cuando el usuario selecciona un código de incidencia, se guarda automáticamente la descripción en el pedido.

## 🧩 Ejemplo 2: En `OnAfterGetRecord` de PageExtension


```
trigger OnAfterGetRecord()
var
    ReasonRec: Record "Return Reason";
begin
    Clear(Rec.DescripcionIncidencia_LDR);
    if Rec.TipoIncidencia_LDR <> '' then
        if ReasonRec.Get(Rec.TipoIncidencia_LDR) then
            Rec.DescripcionIncidencia_LDR := ReasonRec.Description;
end;
```

👉 Uso: Al abrir la página de pedido, se asegura que el campo de descripción esté sincronizado con la tabla de razones.

## 🧩 Ejemplo 3: Guardar código + descripción en la tabla


```
tableextension 50100 PurchaseHeaderExt extends "Purchase Header"
{
    fields
    {
        field(50100; TipoIncidencia_LDR; Code[10])
        {
            Caption = 'Código Incidencia';
            TableRelation = "Return Reason".Code;
        }
        field(50101; DescripcionIncidencia_LDR; Text[100])
        {
            Caption = 'Descripción Incidencia';
            Editable = false;
        }
    }
}
```

👉 Uso: Se guarda tanto el código como la descripción en la tabla `Purchase Header`, lo que facilita reportes y auditoría.

## 📚 Conclusión

Este patrón es la manera estándar en AL de **mostrar en un campo/variable la información que está en otra tabla**.

- Se usa mucho en **documentos de ventas/compras** (mostrar nombre del cliente, descripción de razón, etc.).
- Es útil para **campos no editables** que dependen de un código seleccionado.
- Se puede aplicar tanto en **PageExtension** como en **TableExtension**.