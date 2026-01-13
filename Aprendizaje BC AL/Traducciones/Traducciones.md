
Una forma de evitar que ciertos archivos se traduzcan a través de XLIFF es cambiando la propiedad  Comment = 'Ayuda para el traductor', locked = false, MaxLength = 999; por el valor true
[Traducciones - Documentación LiderIT](http://sdesgh01:8080/desarrollo/Directrices%20de%20desarrollo%20AL/Traducciones/?h=trad) 

```al
pageextension 50108 CustomerListExt extends "Customer List"

{

    trigger OnOpenPage()

    var

        MyMessageMsg: Label 'App published: Hello world',

        Comment = 'Ayuda para el traductor', locked = false, MaxLength = 999;// si pongo locked = true, evito que este archivo se traduzca

    begin

        Message(MyMessageMsg);

    end;

}
```

Para que se genere el archivo de traducciones primero hay que crear el paquete, (o publicar pero tardar más). Para crear el paquete darle a f1 -->AL:Package o ctrl + shift + b

Ahora hay que crear los archivos correspondientes a cada idioma distinto, simplemente utilizo el archivo que se acaba de generar automáticamente que es en inglés, guardar cómo, cambio el nombre y le pongo por ejemplo es-ES. Otra opción es directamente darle a Synchronize to Single File --> New File y listo.

Para actualizar los cambios en los paquetes de idioma hay que darle a la paleta de comandos (f1), -->XLIFF: Synchronize to Single File

Códigos de idiomas
target-language="es-ES"    <!-- Español (España) -->
target-language="fr-FR"    <!-- Francés (Francia) -->
target-language="de-DE"    <!-- Alemán (Alemania) -->
target-language="it-IT"    <!-- Italiano (Italia) -->
target-language="pt-PT"    <!-- Portugués (Portugal) -->
target-language="nl-NL"    <!-- Holandés (Países Bajos) -->
target-language="sv-SE"    <!-- Sueco (Suecia) -->
target-language="da-DK"    <!-- Danés (Dinamarca) -->
target-language="no-NO"    <!-- Noruego (Noruega) -->
target-language="pl-PL"    <!-- Polaco (Polonia) -->
target-language="en-GB"    <!-- Inglés (Reino Unido) -->
target-language="en-CA"    <!-- Inglés (Canadá) -->
target-language="es-MX"    <!-- Español (México) -->
target-language="pt-BR"    <!-- Portugués (Brasil) -->
target-language="fr-CA"    <!-- Francés (Canadá) -->
target-language="de-AT"    <!-- Alemán (Austria) -->
target-language="de-CH"    <!-- Alemán (Suiza) -->
target-language="it-CH"    <!-- Italiano (Suiza) -->
target-language="ru-RU"    <!-- Ruso (Rusia) -->
target-language="zh-CN"    <!-- Chino Simplificado -->

Una vez se ha generado el archivo, utilizando el comando de xliff syncronice to file --> new --> es-ES. Falta incluir la traducción propia. 
![[Pasted image 20251118122151.png]]
Hay que reemplazar las líneas de target por la traducción, es decir
```al
		  <source>Invoice Nº:</source>
          <target state="needs-translation"/>
```
Se tiene que transformar en 
```al
<source>Invoice Nº:</source>
<target state="translated">Nº Factura:</target>
```