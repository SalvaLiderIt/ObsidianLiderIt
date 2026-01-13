
# AL - Patrón de acción para archivar registros con validación previa

## 🧠 Resumen
Este patrón define una acción (`action`) en una página que permite archivar un registro (por ejemplo, una evaluación de proveedor), **solo si no existe otro registro activo relacionado**. Se utiliza para garantizar unicidad lógica antes de realizar una acción irreversible.

---

## ⚙️ Explicación del patrón

- Se define un botón en el área `Processing` de la página.
- Al hacer clic, se ejecuta un `trigger OnAction()` que:
  1. Busca si existe otro registro activo del mismo proveedor.
  2. Si lo encuentra, lanza un error con placeholders (`%1`, `%2`).
  3. Si no lo encuentra, marca el registro como archivado (`Blocked := true`) y guarda la fecha.
  4. Muestra un mensaje de éxito y actualiza la página.

---

## 🧪 Ejemplo completo

```al
action(ArchiveEvaluation)
{
    Caption = 'Archive Evaluation';
    ToolTip = 'Archive the selected vendor evaluation.';
    Image = Archive;

    trigger OnAction()
    var
        ActiveEvaluation: Record VendorEvaluationHeader;
    begin
        // Verificar que no haya otra evaluación activa del mismo proveedor
        ActiveEvaluation.Reset();
        ActiveEvaluation.SetRange(VendorNo, Rec.VendorNo);
        ActiveEvaluation.SetRange(Blocked, false);
        ActiveEvaluation.SetFilter(EvaluationNo, '<>%1', Rec.EvaluationNo); // Excluir la actual

        if ActiveEvaluation.FindFirst() then
            Error(ErrorMsg, Rec.VendorNo, ActiveEvaluation.EvaluationNo);

        // Archivar la evaluación
        Rec.Blocked := true;
        Rec.ArchiveDate := Today();
        Rec.Modify(true);

        Message(SuccessMsg);
        CurrPage.Update(false); // Refresca la página
    end;
}
```

## 🧩 Usos comunes

- Archivar documentos únicos por entidad (evaluaciones, contratos, etc.).
- Validar condiciones antes de modificar un estado.
- Evitar duplicados activos en procesos de negocio.

