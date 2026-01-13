
Para seleccionar el modelo que se va a utilizar a la hora de hacer un informe, hay que ir a la página Report Selection, escoger el tipo de report en Usage, y poner la ID del report deseado

Cada vez que se realizan cambios que puedan afectar al archivo RDL, es necesario Compilar (ctrl + shift + b), cerrar el archivo abierto en Power BI, darle clic derecho sobre el archivo desde Visual Studio --> Open externally


El tamaño estándard de folios A4 en pulgadas es
- Ancho 8,27in o 21cm
- Alto 11,69in o 29,7cm
- Hay que calcular el tamaño total sumando Header, body y footer para evitar espacios en blanco indeseados.


### Abrir report/page directamente desde la URL
En el navegador poner la siguiente URL --> http://localhost:8080/BC260/?company=CRONUS%20Espa%C3%B1a%20S.A.&tenant=default&report=50101  Colocar la ID correspondiente al report (el último número)
Para abrir una página por su ID sería con esta URL pero poniendo la ID correspondiente: 
http://localhost:8080/BC260/?company=CRONUS%20Espa%C3%B1a%20S.A.&tenant=default&page=9307

## Tabla "Infinita"

Para generar informes/reports con muchas páginas o líneas de manera dinámica a modo de desarrollo, para evitar crear los ejemplos manualmente, se utiliza una tabla "infinita".
Va a permitir o bien seleccionar desde dynamics la cantidad de líneas que genera tanto de cabecera como de cuerpo (de la propia tabla), o dejar establecido directamente en el código la cantidad de líneas. 
Una vez terminado el desarrollo y conseguido el propósito, se vuelve a cambiar el dataitem, ya que es el que nos permite crear la tabla de tipo integer que es la infinita.

Un ejemplo de este tipo de tabla hecho por Alex
```al
report 50101 MyReport

{

    UsageCategory = ReportsAndAnalysis;
    ApplicationArea = All;
    DefaultLayout = RDLC;
    RDLCLayout = 'src/reports/a.rdl';
    dataset
    {
        dataitem("Integer"; Integer)
        {
            // RequestFilterFields = Number;
            DataItemTableView = where(Number = filter(1..10));
            column(Number; Number) { }
        }
    }
}
```
En dynamics al abrir este report aparecerá la opción Number en el caso de que esa línea no esté comentada ![[Pasted image 20251120124232.png]]

