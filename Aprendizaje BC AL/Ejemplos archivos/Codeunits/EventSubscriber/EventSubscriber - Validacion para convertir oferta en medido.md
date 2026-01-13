
### 📌 ¿Qué es?

Un `EventSubscriber` permite **interceptar y extender la lógica estándar** de Business Central sin modificar el código base. En este caso, se usa para **validar permisos del usuario antes de convertir una oferta de compra en pedido**.

### 🎯 ¿Para qué sirve?

Este patrón es útil cuando necesitas:

- Añadir **validaciones personalizadas** antes de ejecutar procesos estándar.
- Controlar el comportamiento de procesos sin alterar objetos base.
- Implementar **reglas de negocio específicas** según configuración de usuario, empresa, etc.
- Registrar o auditar acciones antes/después de eventos clave.

### 🛠️ Posibles usos

- Verificar permisos antes de copiar documentos (como en este ejemplo).
- Validar condiciones antes de emitir facturas, registrar pedidos, etc.
- Bloquear acciones si el usuario no cumple ciertos criterios.
- Enviar notificaciones o logs cuando se ejecutan procesos importantes.

### 🧪 Ejemplo práctico

```al
[EventSubscriber(ObjectType::Codeunit, Codeunit::"Purch.-Quote to Order", "OnBeforeRun", '', false, false)]
local procedure CheckUserPermissionOnBeforeConvertQuoteToOrder(var PurchaseHeader: Record "Purchase Header")
var
    UserSetup: Record "User Setup";
    UserNotAllowedErr: Label 'El usuario no tiene permiso para copiar ofertas de compra a pedidos de compra. Por favor revise su configuración de usuario.';
begin
    if not UserSetup.Get(UserId()) then
        Error(UserNotAllowedErr);

    if not UserSetup."Copy Quote to Purchase Order" then
        Error(UserNotAllowedErr);
end;
```