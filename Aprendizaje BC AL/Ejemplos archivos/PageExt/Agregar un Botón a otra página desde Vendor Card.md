

Ejemplo para agregar un botón en el menu de navegación dentro del apartado Vendor (PromotedCategory, Category 9)

```al
pageextension 50101 VendorCard extends "Vendor Card"
{

    layout
    {
        addlast(General)
        {
            field(Homologated; Rec.Homologated)
            {
                ApplicationArea = All;
                Caption = 'Homologated';
            }
        }
    }
    //solo hace falta desde aqui hacia abajo, pero era por poner el ejemplo completo
    actions
    {
        addlast("Ven&dor")
        {
            action("VendorCertificates")
            {
                Caption = 'Certificates';
                ApplicationArea = All;
                Image = Certificate;
                Promoted = true;
                PromotedCategory = Category9;//categoria para que aparezca el boton dentro del apartado de vendor
                PromotedIsBig = true;
                ToolTip = 'Manage vendor certificates.';
                trigger OnAction()
                begin
                    PAGE.Run(PAGE::VendorCertificatesCard);
                end;

            }
        }
    }
}
```

Otro ejemplo de lo mismo sin layout y aplicando un filtro antes de abrir la página a la que lanza el botón

```al
pageextension 50100 VendorCardExt extends "Vendor Card"

{
    actions
    {
        addlast("Ven&dor")
        {
            action("OfferedPurchaseLines")
            {
                Caption = 'Offered Purchase Lines';
                ApplicationArea = All;
                Image = Purchase;
                Promoted = true;
                PromotedCategory = Category9;//categoria para que aparezca el boton dentro del apartado de vendor
                PromotedIsBig = true;
                ToolTip = 'Manage offered purchase lines.';
                trigger OnAction()
                // Declaras una variable de tipo Record "Purchase Line"
                var
                    PurchaseLine: Record "Purchase Line";
                begin
                    // Filtras las líneas por el proveedor actual y tipo Quote
                    PurchaseLine.SetRange("Buy-from Vendor No.", Rec."No.");
                    PurchaseLine.SetRange("Document Type", PurchaseLine."Document Type"::Quote);

                    // Abres la página CON el filtro aplicado
                    PAGE.Run(PAGE::OfferedPurchaseLinesPage, PurchaseLine);
                end;
            }
        }
    }
}
```

![[Pasted image 20251114095255.png]]

**Desde Barra Creation
Si quiero poner el botón directamente en la barra de arriba (Creation) utilizar Promoted atributos Ej.

```al
actions
    {
        area(Creation)
        {
            action("CreatePurchaseOrder")
            {
                Caption = 'Create Purchase Order';
                ApplicationArea = All;
                Image = NewOrder;
                Promoted = true;
                PromotedCategory = Process;// esta linea me permite poner el botón a la derecha de "Delete"
                PromotedOnly = true;

                ToolTip = 'Creates a single purchase order from the selected quote lines.';
                trigger OnAction()
                var

                   CreateOrderMgt: Codeunit "CreateMultipleOffers";//puede que el nombre no sea 100% correcto pero es como lo entiendo de momento
                begin
                    CreateOrderMgt.CreateOorderFromSelectedLines();//este es mi procedimiento
                end;
            }
        }
    }
```

![[Pasted image 20251114115917.png]]

