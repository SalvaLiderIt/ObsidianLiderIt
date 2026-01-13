
Para hacer la llamada a una Codeunit, se puede hacer con un trigger, creando una variable de tipo codeunit, que apunta al nombre de la codeunit.

```al
trigger OnAfterValidate()
            var
                VendorCertMgt: Codeunit "Vendor Certificate Mgt.";
                //nombreVariable: tipo "nombreCodeunit"
            begin
                VendorCertMgt.CheckVendorCertification(Rec);
                //nombreVariable.nombreProcedimiento(parametro/rec=registro actual)
            end;
```