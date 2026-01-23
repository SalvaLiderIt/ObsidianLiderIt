


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



Diarios de reclasificación de producto, sirve para mover artículos con un lote de un almacén a otro.

HA009 --> StandBy
HA019 --> Continuar por el código 1008 (debería de estar bien pero comprobarlo por si acaso, he tenido que volver atrás en commit.) el producto 1009 vabysmo ha dado problemas y no he podido solucionarlo, volver a intentarlo en nuevo chat de github