```al

permissionset 50100 "MarcasYsabores"
{
    Assignable = true;
    Caption = 'Permissions', MaxLength = 30;

    Permissions =
        // tables
        table Taste = X,
        table Item = X,
        table Brand = X,
        tabledata Taste = RIMD,
        tabledata Item = RIMD,
        tabledata Brand = RIMD,
        // pages
        page "Taste List" = X,
        page "Brand List" = X,
        page "Taste Card" = X,
        page "Brand Card" = X;
    // pages
    /*         page BrandCard = X,
            page BrandList = X, */
    /*  page "Taste Card" = X; */


    // Para reports (cuando los crees):
    // report "Nombre Report" = X;

    // Para codeunits (cuando las crees):
    // codeunit "Nombre Codeunit" = X;
}
```