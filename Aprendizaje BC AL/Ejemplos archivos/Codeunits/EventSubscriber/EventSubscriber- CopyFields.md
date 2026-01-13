Ejemplo de Codeunit para "copiar o arrastrar" información de una tabla a otra y volver a mostrarla

En este caso:  copiar datos desde `Customer` a `Sales Header` cuando se valida el cliente
```al
codeunit 50100 CopyFields
{
	Permissions = tabledata Item = RIMD;//agregar permisos desde la codeunit
	
    [EventSubscriber(ObjectType::Table, Database::"Sales Header", "OnAfterValidateEvent", "Sell-to Customer No.", false, false)]
    local procedure OnAfterValidateSellToCustomerNo(var Rec: Record "Sales Header"; var xRec: Record "Sales Header")
    var
        Customer: Record Customer;
    begin
        if Customer.Get(Rec."Sell-to Customer No.") then begin
            Rec.SalesZone := Customer.SalesZone;
            Rec.Route := Customer.Route;
            Rec.TransportAgency := Customer.TransportAgency;
        end;
    end;
}
```

**¿Para qué se usa?** Permite ejecutar lógica personalizada cuando ocurre un evento en otro objeto (tabla, página, etc.). Ideal para copiar datos, validar condiciones o extender comportamiento sin modificar el objeto original.

**Cómo leerlo**?
- `ObjectType::Table`: tipo de objeto que lanza el evento
- `Database::"Sales Header"`: tabla origen del evento
- `"OnAfterValidateEvent"`: tipo de evento (tras validar un campo)
- `"Sell-to Customer No."`: campo que dispara el evento
- `false, false`: no ignorar por licencia ni permisos faltantes
  
**Parámetros del procedimiento**
- `Rec`: registro actual (ya modificado)
- `xRec`: registro anterior (antes del cambio)
- Se usan para comparar valores y aplicar lógica condicional

