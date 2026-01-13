

```al
field(5; VendorNo; Code[20])

        {
            Caption = 'Vendor No.';
            ToolTip = 'Specifies the vendor number.';
            TableRelation = Vendor;
            NotBlank = true;
  
            trigger OnValidate()
            var
                Vendor: Record Vendor;
            begin
                if VendorNo <> '' then begin
                    if Vendor.Get(VendorNo) then
                        VendorName := Vendor.Name
                    else
                        VendorName := '';
                end else
                    VendorName := '';
            end;
        }
```