[UNNOX-F005E Intercompany (Tercer formato).docx](https://lideritc.sharepoint.com/:w:/s/GRUPOUNNOX/IQCoOuj90xpIQIXVDO5CjDe0AZm6my_eis_25HBd-IoZrbA?e=q3v2a5) Teams desarrollo DDT

Horas 39 desarrollo y 7 análisis 

Prompt a agregar después de cada Prompt de las tareas
-  Varias cosas a tener en cuenta, no puedo generar dependencias en mi .app, el "runtime": "15.1", es ese y no se puede modificar. Intento tener la mayor cantidad posible de código dentro de los mismos archivos para reducir la cantidad de IDs que el cliente necesitará. Adjunto el PDF del DDT y esto es lo que llevo hecho ahora mismo: 

### InterCompany UX-F005E AUTOMATIZAR

![[Pasted image 20260129111927.png]] 

### Resumen otorgado a la IA en Prompt para generar una guía para el desarrollo:
- Te acabo de adjuntar el PDF en el que está el DDT de mi siguiente desarrollo. Es el más grande y complejo que he hecho hasta la época y quiero hacerlo lo mejor posible para salir victorioso en este gran desafío que me ha otorgado mi empresa.  
En ese archivo están básicamente los puntos que tengo que hacer hasta terminar el desarrollo, en cuanto al workflow correspondiente dentro de BC ya profundizaré a futuro, incluye muchos pasos. Así a groso modo es un desarrollo que va a comunicar 3 empresas que son de una misma compañía, a las que se puede acceder desde dentro de BC en un mismo Tenant. El desarrollo trata de crear 2 botones (en un principio, si se complica quizás sea en 3 para refactorizar un poco el desarrollo), que permitan AUTOMATIZAR distintos procesos dentro de BC, que son los implicados desde el punto de vista de lógica de negocio en los que (ejemplo práctico:  
Unnox es el cliente nuestro, éste tiene 3 empresas, A, B y C. Cada una de estas empresas fabrica y distribuye distintos productos. Pero entre ellas están comunicadas como si fueran una.  
Les llega un cliente, éste cliente le pide a la empresa A, un producto que realmente no lo tiene la empresa A, si no que lo tiene la empresa C. Es aquí donde entramos nosotros, nuestro cliente (Unnox) no quiere perder el tiempo en generar todo el flujo de compra/venta dentro de su misma empresa, por lo que tenemos que automatizar todo este proceso de, el cliente de Unnox, llamémoslo Toys, le pide a la A, el producto. Ahora automatizamos para que, desde ese pedido de compra que ha recibido la empresa A, nuestro botón va a, mandar un pedido de compra a la empresa C, que tiene el producto, la C va a generar un pedido de venta, va a fabricar el producto, hacer el proceso de picking, crear albarán de venta, factura de venta, todo eso irá de vuelta a la empresa A, pero el producto va a ser enviado directamente al cliente Toys, COMO si fuera la empresa A la que ha hecho todo este proceso. En el albarán de venta que se envía al cliente Toys, habrá un botón justo antes de crearlo y enviarlo, que permitirá cambiar la información de la cabecera para sustituir los campos respectivos de la empresa C por la A. Todo este flujo estará Interconectado gracias a las páginas del estándard de Empresas vinculadas, transacciones de bandeja de salida/entrada /procesadas etc. Ya que será necesaria tener una traceabilidad de todo este proceso que sucede realmente dentro de la empresa Unnox.  
  
Esto es el resumen a groso modo del workflow que tiene que hacer nuestro desarrollo. Tiene más pasos intermedios, (hoja de demanda, ejecutar distintas acciones, aceptar mensajes etc), que iremos discutiéndolos sobre la marcha.  
Con toda esta información que te he dado, me gustaría que crearas en formato Markdown un esquema, un índice, un resumen, muy bien ordenado de los pasos a implementar que vamos a seguir desde el momento 0 hasta el final pleno del desarrollo. Yo este markdown me lo pondré en mi Obsidian y lo usaré de guía, a partir de esa guía iré preguntándote poco a poco cómo hacer cada paso.
## Anotaciones

Intercompany es un área entera, está relacionado con ventas y compras. Tiene que esta firno fino. Fecha de arranque Unnox 1 Marzo. 
El cliente va a probar en Producción --> Cómo se sube a Pro??
Publico en Desarrollo  la empresa dentro de Pro Unnox Ibérica Pruebas Lider IT. **COLOR VERDE.

**Conf Empresas vinculadas es cuando son varias empresas de la misma.

Intercompany se va a basar en productos que se fabriquen
Dentro de productos, Reposición --> Sistema reposición. Dentro de productos tenemos tanto la materia prima como el producto que se va a vender.

Unnox tiene 4 sedes, Ibérica, Navarra y HT + VanoPlas (Francia sólo distribuidora) 
Las tres empresas españolas fabrican y hacen todo el proceso aunque cada una puede hacer distintos productos.
- Ibérica --> Barcelona en concreto vacarices
- Navarra --> Pamplona
- HT --> Cádiz
Las empresas que avmos a utilizar para probar van a ser
- Unnox Iberica LiderIt
- Unnox Iberia HT LiderIT
- Unnox Navarra LiderIT



El primer desarrollo es trucar los albaranes para que de manera ficticia se manden las cosas desde una empresa.

Dentro de pedido de venta --> crear envío de almacén, esto abre una página con la info de pedido de venta, y después cuando se registra el envío de almacén, se genera automáticamente el albarán de venta. El botón para trucar la info de la company va a ser desde la página Albarán de Vent.

A nivel de línea, en Pedido de Venta, hay un botón estándard que es Envío Directo. Esto va a estar activo tanto en el pedido de compra como en el de venta dentro de las propias empresas de Unnox.

Hay un manual de Intercompany con más información está en Teams- Compartida -Documentos- General -Evolutivo -F005


- Hoja de Demanda
	- Es una página estandard que tienen las empresas en la que al darle al botón de Calcular Plan, se generan todas las líneas de necesidades que tiene la empresa, todas las líneas de compra que necesita la empresa. Se automatiza. 
	- Envío directo, traer pedidos venta, se traen los pedidos que tienen el check de envio directo activo. 
	- Inicio -- ejecutar mensajes accion - cuando se le de a este botón y a aceptar, se va a crear el pedido de compra con la otra empresa de Unnox. Introducimos a nivel de línea el nº proveedor. Le damos a Ejecutar mensajes acción.
	- Al ejecutar ese mensaje de acción, se genera un pedido de compra entre Ibérica y Navarra en este ejemplo. 
	- Ahora se ha generado otro pedido de compra, dentro de pedidos de compra, este que hemos generado, hay un botón que es Env ped compra empresas vinculadas, es un botón estandar de intercompany. Aparece un mensaje 
- El desarrollo básicamente va a tener dos botones, el primero en Pedido Venta Original para que al rellenar la información entera del pedido de venta se automatice todo este flujo hasta que llega a la empres que va a fabricar el producto.
- El otro botón es con el flujo al contrario, desde la empresa que va a fabricar el producto. 
- 

Empresas vinculadas, transacciones de bandeja..... -> estas son las paginas que tiene el cliente para ver los movimientos que ha habido dentro de sus empresas, le permite un seguimiento de los pedidos intercompany agrupados, las procesadas son las que se han enviado, si no tienen procesada es una especie de borrador, por algún motivo no se ha llegado a hacer el proceso al completo.

Hay un workflow de Picking, para el proceso desde que se fabrica, y mueve antes de generar un albaran de venta.  
Desde pedido de venta una vez hecho el proceso de picking, podemos darle a registrar envío --> Enviar (Albarán). Una vez registrado volvemos al pedido de venta para comprobar que se ha generado el pedido de venta, está en Pedido -- Envios. La cantidad enviada que aparece a nivel de línea en pedido de venta es la que se ha creado ya vinculada con un albarán de venta. 
Después de tener el albarán le damos a registrar, desde pedido de venta, y después a facturar. Ahora se va esta factura a Histórico facturas venta.

A partir de este paso el estandard peta, dentro de pedido de compra, aparecerá un mensaje arriba, estándard va a crear una factura nueva sin ningún tipo de vinculación. Ahora estamos en Ibérica, ya es la vuelta del proceso. Esta factura será inutil, una persona la eliminará. Nosotros solo queremos el mensaje que aparece arriba, "la empresa vinculada ha enviado este pedido...". 