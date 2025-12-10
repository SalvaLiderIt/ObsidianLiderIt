
Va a servir para cuando tengo una rama del proyecto terminada pero aún no se puede incorporar a la rama principal de producción por cualquier motivo, de alguna manera "pausar" esta actualización de Git, hasta el momento en el que se vuelve a desactivar el símbolo de preprocesado para que así se incorpore en la rama.

Para crear estos símbolos simplemente vamos a "aislar" el código que no queremos que se actualice en la rama principal de la siguiente manera (poniendo al comienzo #if nombre --> código --> #endif)

```al
#if ReleaseAfterCreation (nombre que yo quiera)
	ReleaseSalesDocument.PerformManualRelease(SalesHeader); (código)
#endif
```

De esta manera ni si quiera el compilador está ejecutándose en lo que respecta a las líneas de código que se encuentran dentro de estos símbolos de preprocesado.

En caso de tener varios símbolos de preprocesado y querer que sí que se ejecute uno de ellos en concreto, habría que incluir dentro del app.json, detrás de "runtime": "1.0",
```json
"runtime": "8.0",
"preprocessorSymbols": ["ReleaseAfterCreation","Symbol12","OtroSimbolo"]
```


Para eliminar los símbolos de preprocesado se utiliza el buscador en visual studio code (la lupa de la derecha), activamos el icono de un cuadrado y asterísco que equivale a las expresiones regulares y poniendo algo así como #(.*)ReleaseAfterCreation 
Donde (.*) equivale a cualquier cosa que haya en mitad entre el # y el nombre del símbolo
Básicamente se borran manualmente las líneas de código "comentadas", simplemente el utilizar la lupa es para poder seleccionar esas líneas de entre todos los archivos.