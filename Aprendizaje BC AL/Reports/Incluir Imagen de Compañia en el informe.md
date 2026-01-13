
En el ejemplo de abajo están incluidas las líneas de código necesarias para que se pueda ver una imagen. Es necesario agregar esa imagen a la company information desde dynamics. 
Tener  column(Picture; CompanyInformation.Picture) { } dentro del dataitem en el que queremos que aparezca, y al final de ese mismo data item, incluir el trigger  OnAfterGetRecord con la línea CompanyInformation.CalcFields(Picture);

```al
dataset
    {
        //HEADER ------------------------------------------------------------------------------------------------------------------------------------------
        dataitem(DataSet_Result; "Purchase Header")
        {
            //  todo EN EL HEADER SE REPITE SOLO IMAGEN Y EL "SELLO"
            column(Name; CompanyInformation.Name)
            {
                IncludeCaption = true;
            }
            column(Picture; CompanyInformation.Picture) { }

            dataitem(PurchaseLine; "Purchase Line")

            {

                // BODY ------------------------------------------------------------------------------------------------------------------------------------------

                DataItemLink = "Document No." = field("No.");//filtro para que solo salgan las lineas del albaran actual

                column(Description; Description) { }
                
            trigger OnAfterGetRecord()
            begin
                CompanyInformation.CalcFields(Picture);//muy importante para que se cargue la imagen
            end;
        }
    }
trigger OnPreReport()
    begin
        CompanyInformation.Get();
    end;
    var
        CompanyInformation: Record "Company Information";
```

Una vez tenemos en el código todo lo necesario para agregar la imagen, desde Report Builder, clic derecho --> insertar imagen y con los siguientes valores
![[Pasted image 20251118134740.png]]
