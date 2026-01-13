```al
table 50100 Taste
{
    DataClassification = ToBeClassified;
    Caption = 'Taste';
    LookupPageId = "Taste List";
    DrillDownPageId = "Taste List";
	Permissions = tabledata "Purchases & Payables Setup" = RIMD,
	tabledate "Vendor" = RIMD;//para agregar permisos dentro del propio archivo

    fields
    {
        field(1; Code; Code[20])
        {
            DataClassification = CustomerContent;
            Caption = 'Code';
            ToolTip = 'Specifies Code of the taste-flavour.';
            NotBlank = true;
            // add a trigger to convert to uppercase when the value is validated
            trigger OnValidate()
            begin
                Code := UpperCase(Code);  // Convierte automáticamente a mayúsculas
            end;

        }
        field(2; Description; Text[100])
        {
            DataClassification = CustomerContent;
            Caption = 'Description';
            ToolTip = 'Description of the taste-flavour.';
        }
        field(3; "Product Count"; Integer)
        {
            Caption = 'Taste Products Count';
            ToolTip = 'Specifies Number of products with this taste.';
            FieldClass = FlowField;
            AllowInCustomizations = Always;
            CalcFormula = count(Item where("Taste.Code" = field(Code)));
            Editable = false;
        }
    }
    keys
    {
        key(PK; Code)
        {
            Clustered = true;
        }
    }
    //visual effects
    fieldgroups
    {
        fieldgroup(DropDown; Code, Description) { }
        fieldgroup(Brick; Code, Description) { }
    }

    procedure GetProductCountDisplay(): Text[10]
    begin
        CalcFields("Product Count");
        if "Product Count" = 0 then
            exit('-')
        else
            exit(Format("Product Count"));
    end;
}
```

