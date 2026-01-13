```al
#pragma warning disable LC0048 //decirle al LinterCop que ignore este warning añadiendo una directiva en el código:

tableextension 50100 ItemTableExt extends Item
{
    fields
    {
        field(50100; "Taste.Code"; Code[20])
        {
            Caption = 'Taste';
            ToolTip = 'Specifies the taste of the item.';
            TableRelation = Taste.Code;
            ValidateTableRelation = true;
            DataClassification = CustomerContent;
        }
        field(50101; "Taste Description"; Text[100])
        {
            Caption = 'Taste Description';
            ToolTip = 'Specifies the description of the taste-flavour.';
            FieldClass = FlowField;
            CalcFormula = lookup(Taste.Description where(Code = field("Taste.Code")));
            Editable = false;
        }
        field(50102; "Brand Description"; Text[100])
        {
            Caption = 'Brand Description';
            ToolTip = 'Specifies the description of the brand.';
            FieldClass = FlowField;
            CalcFormula = lookup(Brand.Description where(Code = field("Brand.Code")));
            Editable = false;
        }
        field(50103; "Brand.Code"; Code[20])
        {
            Caption = 'Brand';
            ToolTip = 'Specifies the brand of the item.';
            TableRelation = Brand.Code;
            ValidateTableRelation = true;
            DataClassification = CustomerContent;
            AllowInCustomizations = Always;
        }
        field(50104; "Nutriscore"; Enum Nutriscore)
        {
            Caption = 'Nutriscore';
            ToolTip = 'Specifies the nutriscore of the item.';
            DataClassification = CustomerContent;
            AllowInCustomizations = Always;
        }
    }
}
```

Para modificar un campo de las tablas base de BC, normalmente no me dejaría, pero utilizando la propiedad "modify" sí que puedo. La propiedad se pone al mismo nivel de field{}, dentro de fields{}

```al
fields
{
	modify("Item Category Code")//when i need to change a field in the base table i can use the modify keyword
	     {
	         ToolTip = 'Specifies the category code of the item.';
	         TableRelation = "Item Category" where("Clasification" = const(Family));
		 }
	field
		{
		}
}
```

 [[Llamada a Codeunit desde Tableext en un trigger]] 
