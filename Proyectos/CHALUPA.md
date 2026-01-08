Link Chalupa BC-->  [Dynamics 365 Business Central (PR)](https://businesscentral.dynamics.com/b4be8f5e-c104-4fa9-a40f-f8256b71056d/Desarrollo?company=Andaluzas%20de%20Sales%203&dc=0) 
Link BC CDX Chalupa --> https://businesscentral.dynamics.com/1854fb16-e91a-410f-bb19-e2fb2e1aa10c/DesarrolloVS

Todo viene de un pedido de venta, Ficha preparación, albaranes venta, expedición, listado de transportes 
relacionar linea de transportes con pedido de compras, cuando se vaya a registrar un pedido de compra que tenga transporte, y que este transporte tenga la carta de porte firmada, deja facturar, si no no
==Número de lote que funciona 121125==
Incidencia transporte carta portes y facturación


carta de portes desde expedicion


CH 21
Página
Expedicion_LDR (50022, Card)
Preparaciones SGA
Tabla
Cabecera Preparacion_SGA_LDR (51305)
Pedido compra --> Inicio --> preparar (ELIMINAR BOTÓN PREPARAR)
Botón Preparar dentro de preparar no tiene tooltip



==**CH - A -007** - AgregarCampoCod==
Cuando marco en un pedido compra el check "no conformidad" se abre la venta para poner la descripción y elegir que tipo de incidencia es. Mostrar en la cabecera del pedido de compra, debajo del campo descripción, el campo descripción de la incidencia (en el caso que se la haya abierto una)  

Agregar el campo del COD debajo de la descripción


==CH-A-028 CH-A-029== Hecho merge a desarrollo el 12/12, tarea completa
Son Pedidos de Compra





### CH-A-035.Pedido venta-Albarán venta (kg/merma)**==

- Tanto en el pedido de venta como en el albarán de venta, necesitamos crear un campo que sea "kg/merma", actualmente existe uno que es "% descuento merma", pero se necesita registrar esa merma tanto en % como en kg. El cálculo de este nuevo campo sigue la misma lógica que el otro pero sin aplicarle el porcentaje. Además, una vez creado, también se requiere sacar ambos campos ("kg/merma" y "% descuento merma") en la página "Posted Sales Shipment Lines (525, List)" y que sean filtrables.
- 
	- Pedido de venta Tabla "Sales Line" 37 y pag "Sales Order Subform" el campo se llama DtoMerma_LDR 50002 Decimal 
	- De albaranes venta. "Posted sales Shpt. Subform" y tabla "Sales Shiptment Line"
	- Posted Sales Shipment Lines (525, List)
	- He modificado también EventosCodeunitLDR.Codeunit.al en concreto el último eventsubscriber "AfterSalesShptLineInsert"
- En realidad el valor del campo KG/Merma tiene que ser = (Cantidad - Cantidad Recibida)

%Descuento merma, actualizar información al pedido cuando se modifica desde el albaran de venta

Durante el proceso hemos visto que en el pedido no se actualiza %descuentoMerma, hay que arreglarlo. De momento en Albarán sí que está todo realizado y en la rama Git CH-A-035

Solucionado e incorporado a desarrollo


==**CH-A-036.Factura**==

Hist fact venta


==CH - 025 - 025==

==CH - A - 039 ImporteCantidadRecibida==

El código a modificar es en el archivo SalesLineLDR.TableExt.al (CantidadRecibida_LDR en el OnValidate) y EventosCodeunitLDR.Codeunit.al. 
No funciona el campo Importe según cantidad recibida, de momento en la línea del pedido de venta no sale actualizado el valor si cantidad recibida tiene algo distinto de 0 no lo está usando, sigue usando cantidad.. Ejemplo que funciona las dos siguientes capturas.

43000184 ∙ DISTAC, S. A.