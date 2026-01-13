En una pageext si quiero agregar un botón que me lleve a otro sitio, la estructura es así:

Recordar, en un pageext, no tiene por qué haber layout sí o sí, podemos tener directamente actions si queremos poner un botón directamente sin agregar campos.

```al
actions
{
    addafter(SomeGroup)
    {
        action("Nombre del botón")
        {
            trigger OnAction()
            var
            begin
                PAGE.Run(PAGE::"Nombre de la página");
            end;
        }
    }
}

```

Otra manera más rápida para acceder --> Navegar a otra página es usando

```al
actions
{
    addafter(SomeGroup)
    {
        action("Nombre del botón")
        {
            RunObject = page VendorEvaluationArchivedList;
        }
    }
}

```

Un ejemplo con tres botones agregados a la barra de herramientas de arriba

```al
actions

    {
        addlast("F&unctions")
        {
            action("Vendor Types")
            {
                ApplicationArea = All;
                Caption = 'Table Supplier';
                Image = Table;
                ToolTip = 'Opens the Master Table Supplier page.';
                Promoted = true;
                PromotedCategory = Process;
                PromotedOnly = true;

                trigger OnAction()
                begin
                    PAGE.Run(PAGE::"MasterTableSupplier");
                end;
            }
            
            action("Sector Types")
            {
                ApplicationArea = All;
                Caption = 'Table Sector';
                Image = Table;
                ToolTip = 'Opens the Master Table Sector page.';
                Promoted = true;
                PromotedCategory = Process;
                PromotedOnly = true;


                trigger OnAction()
                begin
                    PAGE.Run(PAGE::"MasterTableSupplier");
                end;
            }
            
            action("Activity Types")
            {
                ApplicationArea = All;
                Caption = 'Table Activity';
                Image = Table;
                ToolTip = 'Opens the Master Table Activity page.';
                Promoted = true;
                PromotedCategory = Process;
                PromotedOnly = true;

                trigger OnAction()
                begin
                    PAGE.Run(PAGE::"MasterTableSupplier");
                end;
            }
        }


    }
```

Si quiero aplicar algún filtro a la hora de mostrar la página con respecto a valores de una tabla por ejemplo: Se crea una variable de tipo Record que apunte a la tabla, y dentro del begin hay que establecer el filtro en sí con un .SetRange que tiene dos parámetros, el primero el nombre del campo a filtrar y segundo el valor de ese campo, en este caso apuntan a un enum que tengo aparte. Y por último se ejecuta la página con PAGE.Run que tiene dos parámetros, el primero el nombre de la página a abrir y el segundo parámetro es el filtro que se le aplica a la hora de abrirla que en este caso es la variable que hemos creado previamente.

```al
action("Sector Types")

            {

                ApplicationArea = All;
                Caption = 'Table Sector';
                Image = Table;
                ToolTip = 'Opens the Master Table Sector page.';
                Promoted = true;
                PromotedCategory = Process;
                PromotedOnly = true;

                trigger OnAction()
                var
                    SectorRec: Record "Master Table Supplier";
                begin
                    SectorRec.SetRange(Type, "Master Table Supplier Type"::Sector);
                    PAGE.Run(PAGE::"MasterTableSupplier", SectorRec);
                end;
            }
```