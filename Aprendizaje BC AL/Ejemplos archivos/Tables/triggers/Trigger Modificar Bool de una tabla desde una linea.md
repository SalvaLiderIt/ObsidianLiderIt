
**Trigger que modifica un campo en otra tabla (cabecera) cuando se valida un campo booleano en una línea.**

**Ejemplo de uso:**

- Al marcar el campo `Good` como verdadero en una línea de evaluación, se actualiza el campo `Result` en la cabecera relacionada (`VendorEvaluationHeader`) a `Testing`.
    

**Estructura típica:**


```al
trigger OnValidate()
var
    Cabecera: Record <TablaCabecera>;
begin
    if <CampoBooleano> then
        if Cabecera.Get(<IdCabecera>) then begin
            Cabecera.<CampoDestino> := <NuevoValor>;
            Cabecera.Modify(true);
        end;
end;
```

**Aplicaciones comunes:**

- Sincronizar estados entre líneas y cabeceras.
- Activar lógica condicional al marcar checkboxes.
- Forzar actualizaciones automáticas en registros relacionados.