Enlace BC DesarrolloVS --> https://businesscentral.dynamics.com/c9666b57-edd4-450d-b07c-a7140590aaca/DesarrolloVS 

Leer documentación --> [coc/.AL-Go at main · clinica-oftalmologica-cordoba-3562/coc](https://github.com/clinica-oftalmologica-cordoba-3562/coc/tree/main/.AL-Go) 

Link BC CDX Desarrollo Arruzafa (MI CDX) --> https://businesscentral.dynamics.com/CRMbc727119.onmicrosoft.com/DesarrolloArruzafa?company=CRONUS%20ES&dc=0 

Tenant Arruzafa -> c9666b57-edd4-450d-b07c-a7140590aaca 
### Extensiones Arruzafa instaladas
- SII Entorno Desarrollo https://businesscentral.dynamics.com/c9666b57-edd4-450d-b07c-a7140590aaca/Desarrollo?aid=FIN&page=2503&filter=%27ID%27%20IS%20%272c108d4c-e375-4cd0-b514-45ae9b6f09bc%27&signInRedirected=1 
- Retenciones (.app subido directamente a administrador de extensiones). Esta y la anterior ambas están en entornos producción, desarrollo y desarrolloVS.

### HA010
Report hecho por Jesús, he modificado el código correspondiente con la funcionalidad de Paciente para que aparezca en el report.
### HA001 N43
Conciliacion banco pag Bank acc. reconciliation, botón Importa Norma 43
Tarea equivalente de la que se ha importado el desarrollo -->CH 025-024 Conciliacion bancaria
**Conciliaciones de cuenta bancaria** --> Conciliación Banco
El último commit trae una supuesta mejora de código, más rápido y óptimo, posiblemente se haya modificado para mal el comportamiento final del desarrollo, en caso de que no funcione, el penúltimo commit llamado "translation added some code optimized..." tiene la versión que funciona.

Los archivos a creados/importados/modificados
- Norm43ImportLDR.Codeunit.al (Creado)
- BankAccReconciliationLDR.PageExt.al (Modificado)
- LDRCargaConciliacionNorma43Lsc.XmlPort.al (Importado/Refactorizado)
- CuentaConcepto43LDR.Table.al (Importado)
- 
Rama Git HA001-ImportarNorma43
Publicado en DesarrolloVS
Pendiente de consultor, necesarias configuraciones
Mergeado a rama Desarrollo

En un principio van a utilizarse sólo archivos .txt con los movimientos de los bancos para la prueba de este desarrollo.

Todo funciona a fecha de 21/01/2026

### HA005
- Pag Importar Nóminas
- Importado desarrollo chalupa CH-004
- Rama de Git HA005-ImportacionExcelNomina
- Publicado en Desarrollo VS
- pendiente de pruebas consultor, seguramente hay que modificar cosas, faltan configuraciones por agregar
- Archivos Creados/modificados
	- PayrollImportSetupArruzafaLDR.Table.al
	- PayrollImportLineArruzafa.Table.al
	- PayrollImportHistoryArruzafa.Table.al
	- PayrollImportStatus.Enum.al
	- PayrollImportMgtArruzafaLDR.Codeunit.al
	- PayrollImportArruzafaLDR.Page.al
	- PayrollImportHistoryArruzafa.Page.al
	-
de importar nominas a Diario general --> nóminas (lista libros diario general) --> Diarios generales (general jorunal) --> sección IMPNOM, a nivel de línea es donde tendría que aparecer las nóminas importadas

Ahora mismo se importa el archivo y se crean las líneas, falta poder comprobar si los valores se adjudican correctamente a los campos correspondientes (hacen falta conocimientos contables)

Dato importante, el 27-01-2026 se ha modificado todo el desarrollo para que el archivo de nóminas a importar sea .csv en lugar de excel, debido a una actualización de BC que no permite el uso de Buffer DotNet

### HA009
Este desarrollo está en Stand By por orden de Andrés 23/01/2026 Diario de producto
- Rama Git HA009-MatrixDiarioProducto
- Archivos usados para este desarrollo: 
	- ItemJournalLineLDR.TableExt.al
	- ScanProductItemJournalLDR.Page.al
	- ItemJournalLDR.Pageext.al
	- ItemJournalScanDTOLDR.table.al
	- ItemJournalScan_LDR.Codeunit.al
- Commit HA009 Task Finished
- Mergeado a rama Desarrollo
- Publicado en Desarrollo VS
- Pendiente de Consultor

### HA019

Pedido Compra, primer paso importante tras dar de alta los productos, decimos a qué almacén entra qué producto, con qué cantidad y que número de lote.

NO TENGO IMAGENES DE UN MISMO PRODUCTO CON DISTINTO LOTE para hacer pruebas. Las imágenes que tengo de un mismo producto pero con distinto identificador se corresponden al Número de Serie, que aún no hay nada como tal en el desarrollo ni en los DDT.

Importado desarrollo de Global Rosetta, archivos creados: 
PurchaseLineLDR.TableExt.al 
PurchaseOrderSubformLDR.PageExt.al 
ScanProductLDR.Page.al (archivo compartido con desarrollo HA022) 

	Creada rama git HA019-MatrixRecepcionAlmacen. 


### HA022
Consta de 3 archivos, EscanearProductosLDR.Page.al,  TransferOrderSubformLDR.PageExt y TransferLinerLDR.TableExt.al

Para comprobar el desarrollo es en la página **Pedidos de Transferencia** (Transfer Orders), tabla Transfer Header. --> Crear un n uevo pedido de transferencia --> hacer clic a nivel de linea y ya aparece la página "Transfer Order Subform" y su tabla "Transfer Line"

Tarea finalizada, Publicada en DesarrolloVS, mergeada rama a Desarrollo. La rama en la que se ha realizado se llama HA022-LectorQRTransferOrder, el último commit se llama "All warnings fixed all translation...task finished HA022"

### HA012
- Falta por agregar el campo retencion 19% y meterlo dentro del procedimiento que calcula la sumatoria. (tiene que avisar MariPaz)


### HA021
Subido a DesarrolloVS, falta probar por consultor. En github es la rama HA021.  
Hay dos desarrollos en paralelo, la misma funcionalidad de importar un archivo Excel usando las nuevas características de BC, y el desarrollo "reciclado" de Unnox que está marcado como Deprecated.  
Falta hacer merge a Main  
  
Archivos modificados:  
COCPurchPriceImportMgt.Codeunit.al  
COCPurchPriceListsExtLDR.PageExt.al  
PurchasePricesLDR.PageExt.al  
VendorCardLDRExtLDR.PageExt.al  
COCPurchPriceImportBuffer.Table.al  
PurchasePriceLDR.TableExt.al

Por orden para acceder a este nuevo desarrollo sería, Proveedores --> Seleccionamos un proveedor (importante que el número de proveedor coincida con la primera columna del excel, si no no se va a importar nada)--> Precios y descuentos en el menú --> Lista de precios de compra (se ha activado esta nueva característica y no aparece en rojo en la columna de actualizacion de datos) --> Nuevo botón “Importar Tarifa” --> Seleccionamos o arrastramos el archivo excel, importante que no tenga columna H, es decir hasta ahora el desarrollo soporta desde la A hasta G incluidas --> Clic en la línea creada con las líneas importadas dentro de ella. 

Importante, para que aparezca la nueva característica y el botón “Listas de precios compra” desde Proveedores, hay que activar la nueva característica en el entorno correspondiente “Actualización de características: nueva experiencia de precios de venta”:
