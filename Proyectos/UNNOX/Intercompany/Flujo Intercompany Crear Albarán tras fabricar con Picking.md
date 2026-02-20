Proceso para probar Intercompany una vez en la empresa B:
Ya hemos creado el pedido de venta en la empresa A con todos los datos necesarios, se ha pulsado el botón de Intercompany y accedemos a la empresa B:

Pedido venta --> poner almacén (principal) + Cod terminos pago y cod forma pago en detalles factura --> Línea --> Informacion relacionada --> lin seguim product --> asignar un lote a nivel de línea (lotefelipe) --> cerramos esto, --> inicio --> crear envio almacen --> estamos en envío almacen --> crear picking (sin rellenar nada--> aceptar) --> Envío --> Lins Picking --> mostrar documento --> Cod ubicación (PRODE), **registrar picking** (si)  --> vuelvo a envio almacen --> Inicio --> registrar envío --> registrar envío --> enviar (con esto ya se genera el albarán). Se puede ver el albarán (el report) dándole a Pedido --> envíos.

Ahora en el pedido de venta de la empresa B, una vez se ha generado el albarán, ya se puede crear la factura Inicio --> Registrar --> Facturar

Ahora ya se puede generar la factura para seguir con el desarrollo de Intercompany e iniciar el proceso de vuelta desde la empresa A

Empresa A de vuelta: (en caso de que no se haga automático el proceso del RF008)
Pedido de compra último generado. Importante, ir a lin seguim prod, e introducir manualmente LOTEFELIPE y la cantidad. Cerramos --> Sí --> 




Util Cleanup Orphaned --> página creada para limpiar registros de lote que se han quedado huérfanos por algún motivo. Elimina datos a nivel de base de datos con SQL desde código. LIMPIEZA CLEAN BORRAR DESTRUIR