Link Alquisur BC --> [Dynamics 365 Business Central (ALQ)](https://businesscentral.dynamics.com/fcb375b9-e92f-465e-aff0-77121953cd43/Pruebas?company=ALQUISUR%20S.L) 


hace un par de semanas hicimos un desarrollo que lo que hacía era: si se modificaba el campo contacto (sell to contact) en la cabecera del pedido de venta, se debía actualizar él o los albaranes que tuviera asociados ese pedido. Esto es lo que está hecho y funciona correctamente.  sin embargo, Teresa, necesita que al imprimir esos albaranes se actualice también el cmapo de contacto

Claudia Consultora Valladolid --> Tarea campo Contacto actualizar reports informes

hist albaranes venta

albaran de venta sin valorar --> 50003
albaran de ventas valorado -->50004

pedido de venta --> campo contacto --> se genera albarán que va a historico albaranes 
hacer que se actualice de manera retroactiva en los informes el campo contacto
sell to contact 84

pruebas/pruebas/producción
Estoy publicando en pruebas, Entorno "Alquiler y Química del Sur SL"

PV501841 API MOVILIDAD SA

He probado y en entorno pruebas no se actualiza el campo que sí que se actualiza en producción, si no me equivoco mi incidencia está solventada pero no puedo probarlo, hay que o bien agregar mi .rdl a producción o activar las extensiones en pruebas para comprobarlo. Tengo que mergear mi rama con main para probar el cambio.



¿Confirma que desea cambiar Fact. a-Nº contacto?
¿Confirma que desea cambiar Venta a-Nº contacto?
