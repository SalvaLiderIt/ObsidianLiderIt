
```al
report 50101 PropioReport
{
    UsageCategory = ReportsAndAnalysis; // es visible en menús de informes
    ApplicationArea = All;
    // PON ESTO PARA QUE TE LOS GENERE CON ctrl + shift + b = package
    // Tipo de Layout
    DefaultLayout = RDLC; //formato del layout
    RDLCLayout = 'src/report/layout/PropioReport.rdl'; //el archivo .rdl se encuentra en esa ruta. Al compilar se convierte en .rdlc
    Permissions = tabledata "Company Information" = r,
    tabledata Customer = r,
    tabledata "Sales Invoice Header" = r,
    tabledata "Sales Invoice Line" = r,
    tabledata "VAT Amount Line" = r,
    tabledata "VAT Product Posting Group" = r,
    tabledata "Cust. Ledger Entry" = r,
    tabledata "Payment Terms" = r,
    tabledata "Payment Method" = r;

    dataset //conjunto de datos que va a utilizar el informe, los datos que se envian al diseño
    {

        // HEADER ------------------------------------------------------------------------------------------------------------------------------------------
        dataitem(DataSet_Result; "Sales Invoice Header")
        {
            /*             PrintOnlyIfDetail = true;
                        RequestFilterFields = "No."; */





            // BODY ------------------------------------------------------------------------------------------------------------------------------------------
            dataitem(SalesLine; "Sales Invoice Line")
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


                trigger OnAfterGetRecord()
                var
                    BlobInStream: InStream;//variable usada para leer el blob
                begin
                    if not Customer.Get("Sell-to Customer No.") then
                        Customer.Init();
                    CompanyInformation.Get();
                    CompanyInformation.CalcFields(Picture);//muy importante para que se cargue la imagen
                    if (SalesLine.Description = '') then
                        CurrReport.Skip();//si no hay comentario no se muestra

                    if VATProdPostingGroup.Get(SalesLine."VAT Prod. Posting Group") then
                        VATClause := VATProdPostingGroup."VAT Clause";

                    //lógica que afecta al tercer dataitem correspondiente con la forma de pago y vencimientos    
                    CustLedgerEntry.Reset();
                    CustLedgerEntry.SetRange("Document No.", "No.");
                    if CustLedgerEntry.FindFirst() then
                        Message('Vencimiento encontrado: ' + Format(CustLedgerEntry."Due Date"));

                    //hace falta una conversión de blob a texto para que aparezca la work description
                    WorkDescriptionAsText := '';
                    if DataSet_Result."Work Description".HasValue() then begin
                        DataSet_Result.CalcFields("Work Description");
                        DataSet_Result."Work Description".CreateInStream(BlobInStream);
                        BlobInStream.ReadText(WorkDescriptionAsText, 65001);

                        // Reemplazar caracteres corruptos comunes utilizando nuestra propia codificación, lo sorprendente es que funciona
                        WorkDescriptionAsText := DelChr(WorkDescriptionAsText, '<>'); // elimina saltos de línea
                        WorkDescriptionAsText := ConvertStr(WorkDescriptionAsText, 'Š', '¿'); // solo caracteres individuales
                        WorkDescriptionAsText := ConvertStr(WorkDescriptionAsText, '‡', 'ú'); // solo caracteres individuales
                        WorkDescriptionAsText := ConvertStr(WorkDescriptionAsText, 'Ž', 'í'); // solo caracteres individuales
                        WorkDescriptionAsText := WorkDescriptionAsText.Replace('í®', 'é');
                        WorkDescriptionAsText := WorkDescriptionAsText.Replace('í„', 'ó');
                        WorkDescriptionAsText := WorkDescriptionAsText.Replace('í‚', 'á');
                        WorkDescriptionAsText := WorkDescriptionAsText.Replace('íú', 'ú');



                    end;

                end;
            }
            // dataitem para mostrar fecha vencimientos y forma de pago --------------------------------------------------------------------------------------------------------------
            dataitem(CustLedgerEntry; "Cust. Ledger Entry")
            {
                DataItemLink = "Document No." = field("No.");
                column(DueDate; "Due Date") { }
                column(DueAmount; "Original Amount") { }
                column(Bill_No_; "Bill No.") { }
            }

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