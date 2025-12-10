"Aka, trozos de código pre-programados"


- ==Ttableext== --> Para extender una tabla 

- ==tfield== --> crear un campo en una tabla
```al
tableextension 50100 "Comp. Bank Account - Customer" extends Customer

{

    fields

    {

       field(50100; "Company Bank Account";Code[20])

       {

            CaptionML = ENU='Company Bank Account', ESP='Cuenta Bancaria de la Empresa';

            TableRelation = "Bank Account";

            Description = 'CLIP001';

       }

    }

}
```

- ==tpageext== --> para extender una página
```al
pageextension 50100 "CompBankAccountCustomerCard" extends "Customer Card"

{
    layout

    {

        addlast(Payments)

        {

            field("Company Bank Account";Rec."Company Bank Account")

            {

                Caption = 'Company Bank Account'; //es el nombre que ve el usuario final en la pantalla

                ApplicationArea = Basic,Suite;

                ToolTipML = ENU='Select a bank to be used in Cash Flow',ESP='Seleccione un banco para utilizar en las previsiones de tesorería';//ToolTipML me da la "explicacion" que aparece con el mouseover, ENU es la explicacion si la aplicación es en ingles

            }

        }

    }

}
```

- 