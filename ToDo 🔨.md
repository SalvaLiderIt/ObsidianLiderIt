

### Datamatrix

Probar desarrollos con escáner 
No disponemos de un mismo artículo con diferentes Lotes para hacer pruebas
TODOS los Datamatrix se reconocen a la perfección, son el resto de formatos los que tienen errores
- HA009 Diario de Productos ==STANDBY==
	- Productos con "errores" Mantener código al escanear
		- 1002 Arceole --> No es formato reconocido e imagen borrosa
		- 1009 Vabysmo (num lote mal, fecha caducidad vacía)
		- 1011 Davinefrina (la fecha la reconoce mal y de cantidad mete 516)
		- 1012 Lidocaina (minidosis) hay que introducir manualmente lote y caducidad
		- 1013 Fortecortín. Num Lote erróneo, lo mezcla con primer campo
		- 1014 Paracetamol ERROR
			- El valor del parámetro 2 de DMY2Date está fuera del intervalo permitido. El valor actual es: 60. El intervalo permitido es: de 1 a 12.
			- 01040305391285131727093010254354517126057797
		- 1015 Tecnis Odissey (la fecha no es correcta)
- HA019 Pedido Compra
	- Productos con errores NO EDITABLES CAMPOS, hacer editables
		- 1002 No se lee con el escáner
		- ==1009 Vabysmo== (num lote mal, fecha caducidad vacía)
		- 1010 Tobrex (no tiene lote, es serial number)
		- ==1011 Davinefrina== (lote mal, la fecha no la reconoce  y de cantidad mete 516)
		- 1012 NO EDITABLE lote y vencimiento???
		- ==1013 Fortecortín.== Num Lote erróneo, lo mezcla con primer campo
		- ==1014 Paracetamol== ERROR
			- El valor del parámetro 2 de DMY2Date está fuera del intervalo permitido. El valor actual es: 60. El intervalo permitido es: de 1 a 12.
			- 01040305391285131727093010254354517126057797
		- 1015 Tecnis Odissey ERROR. Sí que se puede leer este formato desde desarrollo HA009 diario de producto
			- DRN00VI25580838725290728 
			- The length of the string is 24, but it must be less than or equal to 20 characters. Value: DRN00VI25580838725290728
- HA022



Volver a importar el código del desarrollo de chalupa para importar nóminas HA005 importar desde CH-004

HA009 --> StandBy
### HA019 


arreglar el 1009

sigo haciendo pruebas con respecto al escaneo de distintos productos en la página de Pedido Compra --> ScanProductLDR.Page.al  
El formato del código que estoy escaneando, no está parseando bien los campos de Lote y fecha de vencimiento. En lote muestra 100004727359091727063010B1570, fecha de vencimiento vacía. El código del producto al completo es 010847000758340021100004727359091727063010B1570B09  
El lote correcto tendría que ser B1570B09, la fecha de expiración 06-2027  
Hay que agregar este formato de código para completar los campos de este desarrollo. Importante, de una manera aditiva, no destructiva. Tienes que agregar código que permita hacerlo, no puedes modificar la manera de interpretar los otros formatos de código que ya podemos leer correctamente.  
Un ejemplo que por ejemplo sí que se lee correctamente es 01103806575117681017LT211730073111250819306 soy consciente de que el código no es igual, pero era por si te sirve la información

sigue mostrando esto en el lote 100004727359091727063010B1570  
que te parece si agregas un mensaje debug para saber lo que recibe y así ir a tiro cierto
por favor hazlo sin generar warnings ni errores de compilación

DEBUG: Barcode recibido en ParseGS1Elements: 010847000758340021100004727359091727063010B1570B09

En número de lote ha puesto 100004727359091727063010B1570, y la fecha de vencimiento sigue vacia



sigo haciendo pruebas con respecto al escaneo de distintos productos en la página de Pedido Compra --> ScanProductLDR.Page.al ok  
DEBUG: Barcode recibido en ParseGS1Elements: 010560085599002721SKTRFRE7HP6P172705311025FQ1527149937516

SKTRFRE7HP6P172705311025F es lo que aparece en lote, el formato del código que estoy escaneando, no parsesa bien los campos de lote y fecha de vencimiento. Tendría que mostrar en fecha de validez 05 2027 y en lote 25FQ152, agrega este case o lo que sea de manera que si se escanea un código con este formato, se ubiquen correctamente los campos. Pero que no sea destructivo. Tampoco quiero que generes warnings ni errores de compilación

Un ejemplo que por ejemplo sí que se lee correctamente es 01103806575117681017LT211730073111250819306 soy consciente de que el código no es igual, pero era por si te sirve la información


1013 fortecortin
otro caso que no funciona  
DEBUG: Barcode recibido en ParseGS1Elements: 010847000759423917270430102421114B21BH22P403TR  
el lote tendría que ser 2421114B y la fecha de caducidad es 04 2027 el código escaneado es 010847000759423917270430102421114B21BH22P403TR  
este tipo de código también tiene que leerse sin destruir los anteriores
Solo falta por arreglar la fecha de vencimiento


APARCADO hasta tener la versión premium de copilot 