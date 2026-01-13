No se debe hardcodear nada, todo tiene que aparecer como labels (buenas prácticas)
OfertaPropioReport (primer ejercicio semi-real)

```al
report 50102 OfertaPropioReport

{

    UsageCategory = ReportsAndAnalysis; // es visible en menús de informes

    ApplicationArea = All;

    // PON ESTO PARA QUE TE LOS GENERE CON ctrl + shift + b = package

    // Tipo de Layout

    DefaultLayout = RDLC; //formato del layout

    RDLCLayout = 'src/report/layout/OfertaPropioReport.rdl'; //el archivo .rdl se encuentra en esa ruta. Al compilar se convierte en .rdlc

    Permissions = tabledata "Company Information" = r,

    tabledata Customer = r,

    tabledata "Sales Header" = r,

    tabledata "Sales Line" = r,

    tabledata "VAT Amount Line" = r,

    tabledata "VAT Product Posting Group" = r,

    tabledata "Cust. Ledger Entry" = r,

    tabledata "Payment Terms" = r,

    tabledata "Payment Method" = r;

  

    dataset //conjunto de datos que va a utilizar el informe, los datos que se envian al diseño

    {

  

        // HEADER ------------------------------------------------------------------------------------------------------------------------------------------

        dataitem(DataSet_Result; "Sales Header")

        {

            /*             PrintOnlyIfDetail = true;

                        RequestFilterFields = "No."; */

  
  

            // BODY ------------------------------------------------------------------------------------------------------------------------------------------

            dataitem(SalesLine; "Sales Line")

            {

                DataItemLink = "Document No." = field("No.");

                column(Picture; CompanyInformation.Picture)//cada column es un campo que se va a mostrar en el informe, en este caso una imagen

                {

                }

                column(Posting_Date; "Posting Date") { }//fecha emisión de factura

                column(No; "No.") { }//número de factura

                column(WorkDescription; WorkDescriptionAsText) { }//descripción del trabajo

                column(CustomerNo; "Sell-to Customer No.") { }//número de cliente

                column(CustomerName; Customer.Name) { }//nombre del cliente

                column(CustomerAddress; Customer.Address) { }//dirección del cliente

                column(CustomerPostCode; Customer."Post Code") { }//código postal del cliente

                column(CustomerCity; Customer.City) { }//ciudad del cliente

                column(CustomerVAT; Customer."VAT Registration No.") { }//nif del cliente

                column(CompanyName; CompanyInformation."Description 2") { }//nombre de la empresa

                column(CompanyAddress; CompanyInformation.Address) { }//dirección de la empresa

                column(CompanyPostCode; CompanyInformation."Post Code") { }//código postal de la empresa

                column(CompanyCity; CompanyInformation.City) { }//ciudad de la empresa

                column(CompanyPhone; CompanyInformation."Phone No.") { }//teléfono de la empresa

                column(CompanyEmail; CompanyInformation."E-Mail") { }//email de la empresa

                column(CompanyWeb; CompanyInformation."Home Page") { }//página web de la empresa

                column(PaymentTermsCode; Pt."Code") { }

                column(PaymentMethodCode; Puti."Code") { }

                column(BankName; CompanyInformation."Bank Name") { }

                column(IBAN; CompanyInformation."IBAN") { }

                column(Description; "Description") { }

                column(Quantity; "Quantity") { }

                column(UnitPrice; "Unit Price") { }

                column(LineAmount; "Line Amount") { }

                column(VATIdentifier; "VAT Identifier") { }

                column(VATCalculationType; "VAT Calculation Type") { }

                column(AmountIncludingVAT; "Amount Including VAT") { }

                column(VATPercent; "VAT %") { }

                column(VATClause; VATClause) { }

                trigger OnAfterGetRecord()

                var


                begin

                    if not Customer.Get("Sell-to Customer No.") then

                        Customer.Init();

  

                    CompanyInformation.Get();

                    CompanyInformation.CalcFields(Picture);//muy importante para que se cargue la imagen

  

                    if Pt.Get(DataSet_Result."Payment Terms Code") then;//necesito precargar la informacion en las variables correspondientes

                    if Puti.Get(DataSet_Result."Payment Method Code") then;

                    if VATProdPostingGroup.Get(SalesLine."VAT Prod. Posting Group") then

                        VATClause := VATProdPostingGroup."VAT Clause";
  

                end;

            }

            // dataitem para mostrar fecha vencimientos y forma de pago --------------------------------------------------------------------------------------------------------------

  

        }

  

    }

    labels //esto se utiliza para evitar hardcodear texto en Power BI, y para facilitar una posible futura traducción

    {

        NFact = 'Nº Factura';

        FEmision = 'Fecha emisión factura:';

    }

    var

        CompanyInformation: Record "Company Information";

        Customer: Record Customer;

        VATProdPostingGroup: Record "VAT Product Posting Group";

        Pt: Record "Payment Terms";

        Puti: Record "Payment Method";


        VATClause: Text[500];

        WorkDescriptionAsText: Text;

}
```


Segundo formato, rectificativa 

```al
report 50103 RectificativaPropioReport

{

    UsageCategory = ReportsAndAnalysis;

    ApplicationArea = All;

    DefaultLayout = RDLC;

    RDLCLayout = 'src/report/layout/RectificativaPropioReport.rdl';

  

    Permissions = tabledata "Company Information" = r,

                  tabledata Customer = r,

                  tabledata "Sales Cr.Memo Header" = r,

                  tabledata "Sales Cr.Memo Line" = r,

                  tabledata "VAT Amount Line" = r,

                  tabledata "VAT Product Posting Group" = r,

                  tabledata "Cust. Ledger Entry" = r,

                  tabledata "Payment Terms" = r,

                  tabledata "Payment Method" = r;

  

    dataset

    {

        //HEADER ------------------------------------------------------------------------------------------------------------------------------------------

        dataitem(DataSet_Result; "Sales Cr.Memo Header")

        {

  

            column(CorrectedInvoiceNo; "Corrected Invoice No.") { }

  
  
  

            dataitem(SalesLine; "Sales Cr.Memo Line")

            {

                // BODY ------------------------------------------------------------------------------------------------------------------------------------------

                DataItemLink = "Document No." = field("No.");

  

                column(Picture; CompanyInformation.Picture) { }

                column(Posting_Date; "Posting Date") { }

                column(No; "No.") { }

                column(CustomerNo; "Sell-to Customer No.") { }

                column(CustomerName; Customer.Name) { }

                column(CustomerAddress; Customer.Address) { }

                column(CustomerPostCode; Customer."Post Code") { }

                column(CustomerCity; Customer.City) { }

                column(CustomerVAT; Customer."VAT Registration No.") { }

                column(CompanyName; CompanyInformation."Description 2") { }

                column(CompanyAddress; CompanyInformation.Address) { }

                column(CompanyPostCode; CompanyInformation."Post Code") { }

                column(CompanyCity; CompanyInformation.City) { }

                column(CompanyPhone; CompanyInformation."Phone No.") { }

                column(CompanyEmail; CompanyInformation."E-Mail") { }

                column(CompanyWeb; CompanyInformation."Home Page") { }

                column(PaymentTermsCode; Pt."Code") { }

                column(PaymentMethodCode; Puti."Code") { }

                column(BankName; CompanyInformation."Bank Name") { }

                column(IBAN; CompanyInformation."IBAN") { }

                column(MercantileText; CompanyInformation."Mercantile Text") { }

                column(NoLinea; "Line No.") { }

                column(Description; "Description") { }

                column(Quantity; "Quantity") { }

                column(UnitPrice; "Unit Price") { }

                column(LineAmount; "Line Amount") { }

                column(VATIdentifier; "VAT Identifier") { }

                column(VATCalculationType; "VAT Calculation Type") { }

                column(AmountIncludingVAT; "Amount Including VAT") { }

                column(VATPercent; "VAT %") { }

                column(VATClause; VATClause) { }

                /* column(CorrectedInvoiceNoBody; CorrectedInvoiceNoText) { } */

  

                trigger OnAfterGetRecord()

                begin

                    if not Customer.Get("Sell-to Customer No.") then

                        Customer.Init();

                    CompanyInformation.Get();

                    CompanyInformation.CalcFields(Picture);

  

                    if Pt.Get(DataSet_Result."Payment Terms Code") then;

                    if Puti.Get(DataSet_Result."Payment Method Code") then;

  

                    if VATProdPostingGroup.Get(SalesLine."VAT Prod. Posting Group") then

                        VATClause := VATProdPostingGroup."VAT Clause";

  

                    CustLedgerEntry.Reset();

                    CustLedgerEntry.SetRange("Document No.", "No.");

                    if CustLedgerEntry.FindFirst() then

                        Message('Vencimiento encontrado: ' + Format(CustLedgerEntry."Due Date"));

  
  
  
  

                end;

            }

  

            dataitem(CustLedgerEntry; "Cust. Ledger Entry")

            {

                DataItemLink = "Document No." = field("No.");

                column(DueDate; "Due Date") { }

                column(DueAmount; "Original Amount") { }

                column(Bill_No_; "Bill No.") { }

            }

        }

    }

  

    labels

    {

        NFact = 'Nº Factura';

        FEmision = 'Fecha emisión factura:';

        FacturaCorregida = 'Factura corregida:';

    }

  

    var

        CompanyInformation: Record "Company Information";

        Customer: Record Customer;

        VATProdPostingGroup: Record "VAT Product Posting Group";

        Pt: Record "Payment Terms";

        Puti: Record "Payment Method";

        VATClause: Text[500];

}
```

Tercer formato Oferta

```al
report 50102 OfertaPropioReport

{

    UsageCategory = ReportsAndAnalysis; // es visible en menús de informes

    ApplicationArea = All;

    // PON ESTO PARA QUE TE LOS GENERE CON ctrl + shift + b = package

    // Tipo de Layout

    DefaultLayout = RDLC; //formato del layout

    RDLCLayout = 'src/report/layout/OfertaPropioReport.rdl'; //el archivo .rdl se encuentra en esa ruta. Al compilar se convierte en .rdlc

    Permissions = tabledata "Company Information" = r,

    tabledata Customer = r,

    tabledata "Sales Header" = r,

    tabledata "Sales Line" = r,

    tabledata "VAT Amount Line" = r,

    tabledata "VAT Product Posting Group" = r,

    tabledata "Cust. Ledger Entry" = r,

    tabledata "Payment Terms" = r,

    tabledata "Payment Method" = r;

  

    dataset //conjunto de datos que va a utilizar el informe, los datos que se envian al diseño

    {

  

        // HEADER ------------------------------------------------------------------------------------------------------------------------------------------

        dataitem(DataSet_Result; "Sales Header")

        {

            /*             PrintOnlyIfDetail = true;

                        RequestFilterFields = "No."; */

  
  

            // BODY ------------------------------------------------------------------------------------------------------------------------------------------

            dataitem(SalesLine; "Sales Line")

            {

                DataItemLink = "Document No." = field("No.");

                column(Picture; CompanyInformation.Picture)//cada column es un campo que se va a mostrar en el informe, en este caso una imagen

                {

                }

                column(Posting_Date; "Posting Date") { }//fecha emisión de factura

                column(No; "No.") { }//número de factura

                column(WorkDescription; WorkDescriptionAsText) { }//descripción del trabajo

                column(CustomerNo; "Sell-to Customer No.") { }//número de cliente

                column(CustomerName; Customer.Name) { }//nombre del cliente

                column(CustomerAddress; Customer.Address) { }//dirección del cliente

                column(CustomerPostCode; Customer."Post Code") { }//código postal del cliente

                column(CustomerCity; Customer.City) { }//ciudad del cliente

                column(CustomerVAT; Customer."VAT Registration No.") { }//nif del cliente

                column(CompanyName; CompanyInformation."Description 2") { }//nombre de la empresa

                column(CompanyAddress; CompanyInformation.Address) { }//dirección de la empresa

                column(CompanyPostCode; CompanyInformation."Post Code") { }//código postal de la empresa

                column(CompanyCity; CompanyInformation.City) { }//ciudad de la empresa

                column(CompanyPhone; CompanyInformation."Phone No.") { }//teléfono de la empresa

                column(CompanyEmail; CompanyInformation."E-Mail") { }//email de la empresa

                column(CompanyWeb; CompanyInformation."Home Page") { }//página web de la empresa

                column(PaymentTermsCode; Pt."Code") { }

                column(PaymentMethodCode; Puti."Code") { }

  

                column(BankName; CompanyInformation."Bank Name") { }

                column(IBAN; CompanyInformation."IBAN") { }

                column(Description; "Description") { }

                column(Quantity; "Quantity") { }

                column(UnitPrice; "Unit Price") { }

                column(LineAmount; "Line Amount") { }

                column(VATIdentifier; "VAT Identifier") { }

                column(VATCalculationType; "VAT Calculation Type") { }

                column(AmountIncludingVAT; "Amount Including VAT") { }

                column(VATPercent; "VAT %") { }

                column(VATClause; VATClause) { }

  
  
  

                trigger OnAfterGetRecord()

                var

  

                begin

                    if not Customer.Get("Sell-to Customer No.") then

                        Customer.Init();

  

                    CompanyInformation.Get();

                    CompanyInformation.CalcFields(Picture);//muy importante para que se cargue la imagen

  

                    if Pt.Get(DataSet_Result."Payment Terms Code") then;//necesito precargar la informacion en las variables correspondientes

                    if Puti.Get(DataSet_Result."Payment Method Code") then;

  

                    if VATProdPostingGroup.Get(SalesLine."VAT Prod. Posting Group") then

                        VATClause := VATProdPostingGroup."VAT Clause";

  
  

                end;

            }

            // dataitem para mostrar fecha vencimientos y forma de pago --------------------------------------------------------------------------------------------------------------

  

        }

  

    }

    labels //esto se utiliza para evitar hardcodear texto en Power BI, y para facilitar una posible futura traducción

    {

        NFact = 'Nº Factura';

        FEmision = 'Fecha emisión factura:';

    }

  

    var

        CompanyInformation: Record "Company Information";

        Customer: Record Customer;

        VATProdPostingGroup: Record "VAT Product Posting Group";

        Pt: Record "Payment Terms";

        Puti: Record "Payment Method";

  

        VATClause: Text[500];

        WorkDescriptionAsText: Text;

  
  
  

}
```