
- Tipo paletización --> es el último campo. Este campo no está mostrando la información en el pedido de compra de la empresa A, al pulsar el botón de intercompany y crear ese pedido de compra, este campo también tiene que mostrarse con el valor que tiene en el pedido de venta. A nivel de línea, en la primera parte del proceso de Intercompany.

- Falta traducciones al Francés




pedidos fantasma, cada vez que le doy al botón de Intercompany en la empresa A, no se muy bien por qué pero se crea otro pedido de venta con otro cliente distinto y vacío. Esto evidentemente no puede ocurrir ya que llena de basura la lista de pedidos de venta de mi cliente. IMPOSIBLE, 2 HORAS INTENTANDO SOLUCIONARLO Y NO HAY MANERA, el problema viene del desarrollo Intercompany, activando y desactivando cosas he conseguido que no se genere, pero una de dos. Si no se hace pedido fantasma, tampoco se hacer el flow de intercompany, y viceversa






<mark style="background: #FFF3A3A6;">Evolutivos</mark>

RF008, En Pedido de compra de la empresa A, BC estandard lanza un mensaje azul en la parte superior, se crean facturas automáticas al facturar en la empresa C, ésto no quiero que suceda, no quiero que se creen facturas basura fantasma.

Dirección cliente, en el informe,  Dirección del cliente tiene que tener los datos del cliente final, los datos de dirección del cliente mismos que tiene en dirección envío. RF007 Ojo también con el número de albarán


RF008, NO se ha generado el albarán de compra ni el de venta en la empresa A. Lo último que se hizo fue crear la factura desde la empresa C. Según el RF008 ya se tendría que haber creado el albarán de compra y venta.  Último commit con debug para ver que info llega.  la información correspondiente a los campos que están en la página Lins seguim prod.