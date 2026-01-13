
## VISIÓN GENERAL DEL WORKFLOW DE REPORTS EN BUSINESS CENTRAL
Véase también [[Expresiones Power BI]] 
### 🔁 Flujo completo

1. **Diseño visual del informe (PDF de ejemplo)**
    
2. **Creación del objeto** `report` **en AL (Visual Studio Code)**
    
3. **Compilación y publicación del proyecto (Ctrl+Shift+B)**
    
4. **Apertura del archivo** `.rdl` **en Power BI Report Builder**
    
5. **Diseño visual: arrastrar campos desde el dataset**
    
6. **Guardar el** `.rdl` **(se convierte en** `.rdlc`**)**
    
7. **Publicar la extensión en BC y probar el informe**

Ejemplo de Expresión con varios campos dentro de Power BI
- =Fields!CompanyName.Value & " : " & Fields!CompanyAddress.Value & " | " & Fields!CompanyPostCode.Value & " " & Fields!CompanyCity.Value & " | " & Fields!CompanyPhone.Value


==Ejemplo Report Curso Hermanas Nicolás==

```al
report 50100 "Mi Report en VSCode"


{
    CaptionML = ENU = 'My Report in VSCode', ESP = 'Mi Report en VSCode';
    UsageCategory = ReportsAndAnalysis;
    ApplicationArea = All;
    DefaultRenderingLayout = LayoutName;

    //  note esto evita el warning de que faltan permisos explícitos
    //primera opción de hacerlo más límpio y moderno, aunque de momento no veo que corrija el warning
    InherentPermissions = X;
    InherentEntitlements = X;
  
    //segunda opción de hacerlo más explícito
    Permissions =
        tabledata Customer = R,
        tabledata Item = R;
    dataset
    {
        dataitem(Customer; Customer)
        {
            column(No_; "No.")
            {
            }
            column(Name; Name)
            {
            }
            column(Search_Name; "Search Name")
            {
            }
  
            trigger OnAfterGetRecord()
            begin
                Name := CopyStr(MoveNameToUpperCase(Name), 1, 100);
            end;
        }
    }
  
    requestpage
    {
        AboutTitle = 'Teaching tip title';
        AboutText = 'Teaching tip content';
        layout
        {
            area(Content)
            {
                group(GroupName)
                {
                    Caption = 'Group Name';
                    field(ShowSearchName; ShowSearchNameVisible)
                    {
                        CaptionML = ENU = 'Show Search Name', ESP = 'Mostrar Alias';
                        ToolTip = 'Specifies whether to show the Search Name (Alias) for the customer.';
                        ApplicationArea = All;
                    }
                }
            }
        }
    }

    rendering
    {
        layout(LayoutName)
        {
            Type = Excel;
            LayoutFile = 'mySpreadsheet.xlsx';
        }
    }
    var
        ShowSearchNameVisible: Boolean;
  
    local procedure MoveNameToUpperCase(TheName: Text): Text

    begin
        exit(UpperCase(TheName));
    end;
}
```

El te buscar información y exportarla a excel entre otros formatos.

Para mostrar imágenes en la cabecera, es necesario agregar un "Calcfield de la imagen" en el trigger OnAfterGetRecord
```al
trigger OnAfterGetRecord()//en lugar de hardcoderar valores, se pueden asignar en estos triggers para conseguir informacion de la empresa

            begin

                CompanyInformation.Get();

                Customer.Get("Sell-to Customer No.");

                CompanyInformation.CalcFields(Picture);

  

            end;
```

- Para evitar hardcodear nada en Power BI, ya que posiblemente necesitemos que en algún momento se traduzca todo el contenido de la factura, en caso de que el campo que queramos mostrar ya exista en las tablas base de Dynamics hacerlo agregando "IncluyeCaption" tal que así: De esta manera al compilar aparece este elemento "CustomerNameCaption" en la sección de parámetros, como si fuera un label
```al
column(CustomerName; "Sell-to Customer Name")
            {
                IncludeCaption = true;
            }
```
- Para seleccionar la plantilla/archivo que vamos a utilizar (rdl), hay que irse (en español) desde el cliente web de BC a buscar --> selección de informes: Ventas --> el tipo de informe, si es abono (rectificativa..etc), poner la Id. informe que es la ID de mi proyecto .al 
- ![[Pasted image 20251031133447.png]]

103039