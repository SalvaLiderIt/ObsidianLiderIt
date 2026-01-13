
- Auto-asignación de cuenta bancaria desde cliente a documento de venta
- ```al
  codeunit 50100 CompBankAccountMgt
  {
	  trigger OnRun()

    begin

    end;

    [EventSubscriber(ObjectType::Table,Database::"Sales Header", OnAfterValidateEvent, "Bill-to Customer No.", true, true)]

    local procedure SetCompanyBankAccount_SalesHeader(var Rec : Record "Sales Header"; var xRec: Record "Sales Header"; CurrFieldNo : Integer)

    var

        Customer : Record Customer;

    begin

        if Customer.Get(Rec."Bill-to Customer No.") then

            if Customer."Company Bank Account" <> '' then

                Rec.Validate("Company Bank Account", Customer."Company Bank Account");

    end;
  }
  ```
- Para mostrar un mensaje en pantalla por un error hay dos opciones, o utilizando Error(), o utilizando Message() + exit. Con el primero va a dar un error y va a parar toda la ejecución de la extensión, sin embargo utilizando Message, vamos a seguir reproduciendo la iteración, parando justo la que ha dado el error. Ejemplo:
- ![[Pasted image 20251017120841.png]]
