Para utilizar mensajes de error y evitar los warning del IDE, es necesario incluir una variable de tipo Label en la sección de variables, y además, agregar un "Comment" que **INCLUYA los mismos tags dentro del comentario tal que así 

```al
ErrorMsg: Label 'Cannot archive: The vendor %1 already has an active evaluation (Nº %2).',Comment= '%1 si no incluyo esto me da warning y %2 tantos tags como tenga ';
```

Ejemplo completo del trigger que contiene este ErrorMsg

```al
trigger OnAction()
                var
                    ActiveEvaluation: Record VendorEvaluationHeader;
                    ErrorMsg: Label 'Cannot archive: The vendor %1 already has an active evaluation (Nº %2).',Comment= '%1 si no incluyo esto me da warning y %2 tantos tags como tenga ';
                    SuccessMsg: Label 'Evaluation has been archived successfully.';
                begin
                    // Verificar que no haya otra evaluación activa del mismo proveedor
                    ActiveEvaluation.Reset();
                    ActiveEvaluation.SetRange(VendorNo, Rec.VendorNo);
                    ActiveEvaluation.SetRange(Blocked, false);
                    ActiveEvaluation.SetFilter(EvaluationNo, '<>%1', Rec.EvaluationNo);
  
                    if ActiveEvaluation.FindFirst() then
                        Error(ErrorMsg, Rec.VendorNo, ActiveEvaluation.EvaluationNo);

                    // Archivar la evaluación
                    Rec.Blocked := true;
                    Rec.ArchiveDate := Today();
                    Rec.Modify(true);

                    Message(SuccessMsg);
                    CurrPage.Close();  // Cerrar la ficha
                end;
```

