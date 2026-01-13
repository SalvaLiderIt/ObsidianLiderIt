**Ejemplo De botón para importar archivo, exportar o eliminar (Ejercicio 5)**

```al
page 50101 VendorCertificatesList //Ficha Certificado Proveedor

{
    PageType = List;
    ApplicationArea = All;
    UsageCategory = Administration;
    SourceTable = VendorCertificatesHeader;
    Caption = 'Vendor Certificates';
    layout
    {
        area(Content)
        {
            group(GroupName)
            {
                Caption = 'Vendor Certificates Header';
                field(CertifiedCode; Rec.CertifiedCode)
                {
                }
                field(CertifiedType; Rec.CertifiedType)
                {
                }
                field(IssueDate; Rec.IssueDate)
                {
                }
                field(ExpirationDate; Rec.ExpirationDate)
                {
                }
                field(Active; Rec.Active)
                {
                }
                /* field(Attached; Rec.Attached)
                {
                } */
            }
        }
    } 

    actions
    {
        area(Processing)
        {
            action(ImportAttachment)
            {
                Caption = 'Import Document';
                Image = Import;
                ToolTip = 'Import and attach a document to this certificate.';
                ApplicationArea = All;
                PromotedCategory = Process;
                PromotedIsBig = true;
                Promoted = true;
                
                trigger OnAction()
                var
                    InStr: InStream;
                    OutStr: OutStream;
                    FileName: Text;
                begin
                    if UploadIntoStream('Select a file', '', '', FileName, InStr) then begin
                        Clear(Rec.Attached);  // Limpiar el blob primero
                        Rec.Attached.CreateOutStream(OutStr);  // Crear el stream de salida
                        CopyStream(OutStr, InStr);  // Copiar los datos
                        Rec.Modify(true);
                        Message('File %1 uploaded successfully.', FileName);
                    end;
                end;
            }

            action(ExportAttachment)

            {
                Caption = 'Export Document';
                Image = Export;
                ToolTip = 'Download the attached document.';
                ApplicationArea = All;
  
                trigger OnAction()
                var
                    InStr: InStream;
                    FileName: Text;
                begin
                    if not Rec.Attached.HasValue() then
                        Message('No hay ningún documento adjunto.');
  
                    Rec.Attached.CreateInStream(InStr);
                    FileName := 'Certificate_' + Rec.CertifiedCode + '.pdf';  // Puedes mejorar el nombre
                    DownloadFromStream(InStr, 'Export', '', '', FileName);
                end;
            }
  
            action(DeleteAttachment)
            {
                Caption = 'Delete Document';
                Image = Delete;
                ToolTip = 'Remove the attached document.';
                ApplicationArea = All;
  
                trigger OnAction()
                begin
                    if not Rec.Attached.HasValue() then
                        Message('No hay ningún documento adjunto.');
  
                    if Confirm('¿Está seguro de que desea eliminar el documento adjunto?') then begin
                        Clear(Rec.Attached);
                        Rec.Modify(true);
                        Message('Documento eliminado.');
                    end;
                end;
            }
        }
    }
}
```

- **`Promoted = true`**: Hace que la acción sea visible en la barra superior
- **`PromotedCategory = Process`**: La agrupa en la categoría "Process/Procesar"
- **`PromotedIsBig = true`**: Hace que el botón sea más grande y visible

