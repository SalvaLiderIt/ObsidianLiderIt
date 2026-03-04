
Ejemplo para traer campos de otra página a la actual

```java
// 1. Filtra las líneas de factura que pertenezcan a esta factura
SalesInvLine.SetRange("Document No.", Rec."No.");

// 2. Busca esas líneas
if SalesInvLine.FindSet() then
    // 3. Recorre todas las líneas encontradas
    repeat
        // Aquí procesas cada línea de factura
    until SalesInvLine.Next() = 0;   // Cuando no queden más, sale

```
