El "codex" que se utiliza en el desarrollo de Getronix de Fármacos es GS1 DataMatrix, específicamente el **FNC1 (Function Code 1)**
Proceso para obtener la información del escáner/datamatrix de un código QR sin perder información y conseguir los caracteres invisibles.

1. Descargar la imagen adjunta. (Indicándome cuál de los tres códigos se va a escanear De arriba abajo 1-2-3)
2. Abrir un archivo nuevo .txt con el bloc de notas
3. Colocar cursor en la primera línea del txt, teniendo abierta la imagen del código QR en una pantalla, escanear código con la pistola.
4. Si se ha realizado correctamente, ha generado un código como si lo hubiera hecho un teclado. Visualmente son letras y números pero hay más información “invisible”.
5. Guardar el archivo .txt, comprimir como .ZIP
6. Enviar esta carpeta comprimida .ZIP por e-mail.

Este proceso es necesario para obtener el formato utilizado para escanear por el modelo/marca del escáner sin tener acceso físico a él.


Previo a usar desarrollo de Diaro de productos, hace falta establecer una relación GTIN con cada producto, esto se establece en Ficha producto --> Relacionado --> Producto --> Referencias artículo --> Establecer el primer campo "Tipo referencia" con el valor Cód. Barras, y el campo Nº Referencia es el que tiene que tener el valor del GTIN. 
Para obtener el GTIN de un código de un producto nuevo 



### **Sobre la configuración del escáner (Zebra DS22 y Honeywell)**

El comportamiento que describes (GS aparece solo cuando escaneas en BC y no en Notepad) indica que el escáner está en modo **Keyboard Wedge** y probablemente **no está transmitiendo FNC1 como GS (ASCII 29)** en todos los contextos. Esto depende de la configuración.

#### **Qué debes hacer en Zebra DS22**

1. **Activar GS1 DataMatrix y FNC1**
    - Escanea el código de configuración: _“Enable GS1 DataMatrix”_.
2. **Transmitir FNC1 como GS (ASCII 29)**
    - Busca en el manual el código: _“Transmit FNC1 as GS”_ (Group Separator).
3. **Desactivar sustituciones**
    - Asegúrate de que no esté configurado para convertir GS en TAB o eliminarlo.
4. **Verificar sufijos**
    - Mantén solo `CRLF` como terminador, sin filtros adicionales.

#### **Para Honeywell (clientes)**

- Igual: activar GS1 DataMatrix y FNC1.
- Escanear el código de configuración: _“Send FNC1 as ”_.
- Desactivar reglas ADF que puedan eliminar caracteres de control.

## 1. ¿Qué es el GTIN y cómo se obtiene al escanear?

- El **GTIN** (Global Trade Item Number) es un identificador internacional único para productos, normalmente de 13 o 14 dígitos, que viene codificado en los códigos de barras (EAN/UPC) y en los DataMatrix/QR de los envases.
- Cuando escaneas un DataMatrix/QR, el primer bloque de datos con el AI (Application Identifier) **01** es el GTIN.  
    Ejemplo:  
    Si el código escaneado es `01084700066159801728033110BX593221VG4R5TWP1`,  
    el GTIN es **08470006615980** (los 14 dígitos después de "01").