
```al
pageextension 50100 ItemCard extends "Item Card"
{
    layout
    {
        addlast(Item)
        {
            group(MarcasYSabores)
            {
                Caption = 'Marcas y Sabores';

                field("Taste"; Rec."Taste.Code")
                {
                    ApplicationArea = All;
                    Caption = 'Taste';
                    ToolTip = 'Specifies Select the taste of the item.';
                }
                field("Taste Description"; Rec."Taste Description")
                {
                    ApplicationArea = All;
                    Caption = 'Taste Description';
                    Editable = false;  // Confirmamos que no es editable
                }
                field("Brand"; Rec."Brand.Code")
                {
                    ApplicationArea = All;
                    Caption = 'Brand';
                    ToolTip = 'Specifies Select the brand of the item.';
                }
                field("Brand Description"; Rec."Brand Description")
                {
                    ApplicationArea = All;
                    Caption = 'Brand Description';
                    Editable = false;  // Confirmamos que no es editable
                }
                field("Nutriscore"; Rec."Nutriscore")
                {
                    ApplicationArea = All;
                    Caption = 'Nutriscore';
                    ToolTip = 'Specifies Select the nutriscore of the item.';
                }
            }
            // Add changes to page layout here
        }
    }

    actions
    {
        addafter(Functions)
        {
            action(TasteList)
            {
                ApplicationArea = All;
                Caption = 'Ver Taste List';
                Image = List;
                ToolTip = 'Opens the Taste List page.';
                // Estas dos líneas me permiten mostrar el botón en la barra de acciones principal
                Promoted = true;
                PromotedCategory = Process;

                trigger OnAction()
                var
                begin
                    // Abre la página Taste List
                    PAGE.Run(PAGE::"Taste List");
                end;
            }
            action(BrandList)
            {
                ApplicationArea = All;
                Caption = 'Ver Brand List';
                Image = List;
                ToolTip = 'Opens the Brand List page.';
                // Estas dos líneas me permiten mostrar el botón en la barra de acciones principal
                Promoted = true;
                PromotedCategory = Process;

                trigger OnAction()
                var
                begin
                    // Abre la página Brand List
                    PAGE.Run(PAGE::"Brand List");
                end;
            }
        }
    }

}
```