
### HA022
- 
### HA009


No disponemos de un mismo artículo con diferentes Lotes para hacer pruebas

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
- Imitar comportamiento de HA019 Pedido Compra, ScanProduct y la codeunit GS1BarcodeParser. Ya está operativa a falta de SerialNumber a día de 28-01-2026

### HA019 

En este desarrollo falta por implementar lo correspondiente a SerialNumber que no se ha nombrado en el DDT

