
[Github - Documentación LiderIT](http://sdesgh01:8080/desarrollo/Github/) 

El orden para trabajar sería, clono el repositorio, por ejemplo origin/Desarrollo.
- Create Branch From --> origin/Desarrollo, se le pone el nombre a la rama.
- Descargar símbolos, trabajar, hacer proyecto, una vez esté terminada la rama, hay que hacer commit, sincronizar. Después cambiar de rama a la de Desarrollo que es la rama padre de la que yo he creado. Estoy en Desarrollo, darle a git pull from (la rama que yo he creado), y de esta manera se incluyen los cambios en la rama principal.
- Para que salgan las modificaciones de la rama principal (desarrollo), cuando estoy en mi subrama, directamente poner en el comando Pull From (desarrollo) y ya me aparecerán los cambios más actuales. (No se la utilidad de Fetch)

### Workflow Ramas LiderIT

![[Pasted image 20251127124005.png]]


<mark style="background: #ABF7F7A6;">Flujo</mark> a seguir de <mark style="background: #ABF7F7A6;">Lider</mark> con respecto a las <mark style="background: #ABF7F7A6;">ramas de GitHub</mark>:
- Utilizar MCP para crear y eliminar ramas, se crean desde Main, de esta manera MCP genera una trazabilidad en GitHub, se utilizan, se incorporan a Main, y se elimina la rama. 
	- MCP (Model Context Protocol) es un estándar abierto que permite conectar LLMs (modelos como ChatGPT, Claude o Copilot) con herramientas externas, APIs, datos, repositorios, workflows… de forma estandarizada y segura.
	- Ha publicado su GitHub MCP Server, una integración oficial que permite a los modelos acceder a repositorios, PRs, issues, workflows, commits, etc.
	- Facilita que Copilot y otros agentes puedan entender tu repo, manipularlo, consultarlo o automatizar tareas usando el protocolo estándar MCP.
- Con el CICD (sistema) Es el proceso de automatizar tareas cada vez que haces un commit o un pull request, como: Compilar tu código Ejecutar tests Revisar calidad de código Validar que no rompes nada La idea: detectar errores lo antes posible. Ejemplo típico en GitHub Actions: Cada push → ejecuta pruebas automáticamente.
- 