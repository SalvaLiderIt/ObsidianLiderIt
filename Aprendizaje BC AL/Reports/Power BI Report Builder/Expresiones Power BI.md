
- Para "hardcodear" y concatenarla con una expresión, simplemente agregar por ejemplo al inicio, detrás del =, el texto entre comilllas seguido de & antes de First tal que así
- `="Cliente: " & First(Fields!CustomerNo.Value, "DataSet_Result")
` 
	- Otro ejemplo de dos campos combinados en una expresión y que además tiene modificaciones de caracteres:
	- ````=Parameters!CIFlbl.Value & Left(Fields!CIF.Value, 1) & "-" & Mid(Fields!CIF.Value, 2, Len(Fields!CIF.Value) - 1)````
	  ```
- Otra forma de mostrar un campo en Power BI es  poniendo entre corchetes el nombre del campo, por ejemplo [CustomerVAT] (que mostraría el CIF)
- En medida de lo posible hacer lo máximo desde report.al en lugar de hacerlo con expresiones, ya que optimiza el rendimiento y no tarda tanto tiempo compilando.
- Se pueden cambiar los formatos, para fecha, moneda hora etc simplemente desde las propiedades de cuadro de texto
  ![[Pasted image 20251028095103.png]]
- Dato muy importante y útil. En caso de que por cualquier motivo en el informe se esté mostrando líneas vacías en alguna posición de cualquier tabla. Si la tabla tiene filas que tengan expresiones de agrupación. Simplemente abrir las propiedades de grupo --> visibilidad --> Mostrar u ocultar en función de una expresión --> y agrego una expresión que sea igual a la que tengo en general, pero agregándole "IsNothing()" tal que así
	- ![[Pasted image 20251030121515.png]]
	- 





## Mayúsculas, UCase, Caps
Para que todo el contenido de una celda aparezca en mayúsculas, agregar UCase al comienzo de la expresión
```Report Builder
=UCase(Fields!Nombre.Value)
```

## Combinar labels (parametros), campos (dataset) y texto hardcoded en una expresión
```ReportBuilder
=Parameters!VATlbl.Value & " " & Fields!VAT_Percent.Value & "%"
```
## Mostrar Línea distinta de 0
Expresión para que una fila se muestre solo si el resultado es distinto de 0, ya sea positivo o negativo. La expresión se tiene que poner en propiedades de grupo --> visibilidad --> mostrar u ocultar según expresión, y ahí dentro.
```ReportBuilder
=Fields!VAT_Percent.Value = 0
```
## CIF con Guión detrás de la letra
```ReportBuilder
=Left(Fields!CIF.Value, 1) & "-" & Mid(Fields!CIF.Value, 2, Len(Fields!CIF.Value) - 1)
```
##  Mostrar el porcentaje de IVA en el label dinámicamente

En lugar de usar un label fijo como `VATlvl = 'VAT'`, puedes **mostrar el porcentaje directamente en el textbox** del RDLC usando esta expresión:

rdlc

```
="IVA " & Fields!VAT_Percent.Value & "%"
```

Esto mostrará, por ejemplo: `IVA 21%`, `IVA 7%`, etc., según el valor de cada línea.

> Si estás agrupando por `VAT_Percent`, puedes poner esta expresión en la celda del grupo para que se muestre una vez por tipo.


##  Mostrar el porcentaje de IVA en el label dinámicamente

En lugar de usar un label fijo como `VATlvl = 'VAT'`, puedes **mostrar el porcentaje directamente en el textbox** del RDLC usando esta expresión:

rdlc

```
="IVA " & Fields!VAT_Percent.Value & "%"
```

Esto mostrará, por ejemplo: `IVA 21%`, `IVA 7%`, etc., según el valor de cada línea.

> Si estás agrupando por `VAT_Percent`, puedes poner esta expresión en la celda del grupo para que se muestre una vez por tipo.

##  Calcular el importe de IVA por línea

Para calcular el IVA de cada línea, usa esta expresión en el RDLC:

rdlc

```
=-(Fields!Line_Amount.Value * Fields!VAT_Percent.Value / 100)
```

Esto calcula el impuesto correspondiente y lo muestra en negativo como necesitas.

> Si estás agrupando por tipo de IVA, puedes usar:

rdlc

```
=-Sum(Fields!Line_Amount.Value * Fields!VAT_Percent.Value / 100)
```



