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


PV400019 Albanebrix

  
Do you want to change Sell-to Contact No.?

==Mensaje a claudia==:
Claudia muy buenas, llevo toda la mañana intentando localizar el código que tienen subido a producción pero no lo han subido a ningún lado. No se quien hizo este desarrollo, pero te puedo confirmar al 100% que tienen el código correspondiente a estos desarrollos en uno de sus PC, publicaron con esa información y por eso se ve en producción, pero no podemos hacer las pruebas con el campo Contacto con respecto a los informes, porque no tengo acceso a los códigos. Yo lo voy a dejar subido a GitHub (nube de programacion de lider), y te lo dejo publicado en Pruebas. Hasta que no venga el programador que hizo el desarrollo no se pueden unificar ambas cosas porque repito, está en su ordenador y no tenemos acceso. Para que cuando se retome esta tarea no sea tan tedioso, te dejo anotado por aquí la Rama de Github (esto solo le sirve al programador que lo retome), donde sí que está hecha la tarea pero hace falta hacer un "merge" a la rama principal combinándola con el código del desarrollo al que no tenemos acceso ahora.
La rama que tiene este cambio con respecto al report se llama "ReportContactoAlbaranes".

No se ha subido la tarea a rama main, está en la subrama. Que está actualizada según origin/desarrollo 

