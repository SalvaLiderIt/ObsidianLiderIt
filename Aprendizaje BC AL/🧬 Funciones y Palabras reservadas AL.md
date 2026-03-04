- **SetRange**: 
	- aplica filtros exactos (acumulativos), sirve para Filtrar una tabla
	```java
	SalesLine.SetRange("Document No.", 'PED00045');
	```
	- Es igual que decir: Muéstrame solo los registros donde Document No = PED00045

- **SetCurrentKey**: 
	- fija el **orden** de iteración.
	```java
	SalesLine.SetCurrentKey("Document No.", "Line No.");
	```
	- Cuando iteres esta tabla, ordena por Document No y Line No.

- **FindSet / FindFirst / FindLast / Find('-')**
	- Cómo se quieren recorrer los datos
	- Es un Boolean, devuelve True si ha encontrado registros o False si no los ha encontrado.
	- FindSet() - para bucles normales
		- Recupera un conjunto de registros, optimizado para iterar
		- ```java
		if SalesLine.FindSet() then
	    repeat
        // código
	    until SalesLine.Next() = 0;
		  ```
	- FindSet (TRUE) - si vas a modificar registros dentro del bucle
	- FindFirst() - si solo necesitas el primero

- FieldCLass = FlowField;
	- Es un campo calculado, no almacenado físicamente en la tabla, No ocupa espacio en SQL, y su valor se calcula "al vuelo" cuando lo lees, este cálculo se hace gracias a:
	- CalcFormula
	- ```java
	  CalcFormula = lookup("Payment Method".Description where(Code =               field("Payment Method Code")
	  ```
	-   lookup --> "Busca un registro relacionado y devuélveme un único valor"
	- "Payment Method".Description --> Usa la tabla estándar Payment Method, y devuélveme el campo Description.
	- 




- **repeat…until / Next()**: patrón de recorrido; `Next() = 0` ⇒ fin.
- **Message('%1')**: placeholders; evita spamear dentro del bucle en productivo.
- **Reset()**: limpia filtros/clave si vas a reutilizar el record.

- **FindSet(true)**: si vas a **modificar** registros dentro del bucle.