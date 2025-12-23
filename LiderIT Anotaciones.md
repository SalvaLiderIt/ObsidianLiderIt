
- [Control IT](https://controlit.es/#remotes) : gestión de fichajes, ausencias, bajas médicas y vacaciones.
- [Portal Empleados](https://portalempleado.liderit.es/empleados/default.aspx?ReturnUrl=%2fempleados%2fMemberPages%2fconfiguser.aspx) /a: acceso centralizado a documentación corporativa y personal.
- [- ERP](https://erp.liderit.es/) : registro y gestión de partes de trabajo.
- [GLPI - Autenticación](http://soporteinterno.liderit.es/glpi/index.php?redirect=%2Ffront%2Fcentral.php&error=3) : sistema interno de ticketing para la comunicación de incidencias y solicitudes de informática.
- DDT: Documento Desarrollo Técnico
- DSN: Desarrollo solución de negocio
- Ciclo de Desarrollo Lider [Entornos en el cliente - Documentación LiderIT](http://sdesgh01:8080/desarrollo/Entornos/EntornosCliente/) 
- Documentación Lider [Documentación LiderIT](http://sdesgh01:8080/) 
- [[Conexión vpn desde fuera oficina a red interna]] poner VPN Lider
- Para incidencias de acceso, hablar con cargos HelpDesk en Teams como por ejemplo Carlos Alfonso Navarro o Juan José Menendez


### Buenas Prácticas Lider
- Namespaces vamos a utilizar dos tipos de espacios de nombres según sean para extensiones "genéricas" o para las de los clientes:
	- Genéricas: LiderIT.NombreExtensionGenerica.NombreModulo
	- Clientes: LiderIT.NombreCliente.NombreModulo
	- La nomenclatura para usar en código y declarar un namespace es simplemente en una línea nueva 
	- ```al
	  namespace MyNamespace;
	  ```
- Usar #region y #endregion con el nombre de tarea, por ejemplo:  #region ch012 





