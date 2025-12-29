Enlace BC DesarrolloVS --> https://businesscentral.dynamics.com/c9666b57-edd4-450d-b07c-a7140590aaca/DesarrolloVS 

Leer documentación --> [coc/.AL-Go at main · clinica-oftalmologica-cordoba-3562/coc](https://github.com/clinica-oftalmologica-cordoba-3562/coc/tree/main/.AL-Go) 

Link BC CDX Desarrollo Arruzafa (MI CDX) --> https://businesscentral.dynamics.com/CRMbc727119.onmicrosoft.com/DesarrolloArruzafa?company=CRONUS%20ES&dc=0 




### HA022
Consta de 3 archivos, EscanearProductosLDR.Page.al,  TransferOrderSubformLDR.PageExt y TransferLinerLDR.TableExt.al

Para comprobar el desarrollo es en la página Pedidos de Transferencia (Transfer Orders), tabla Transfer Header. --> Crear un n uevo pedido de transferencia --> hacer clic a nivel de linea y ya aparece la página "Transfer Order Subform" y su tabla "Transfer Line"

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
