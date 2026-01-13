
```bash
// para crear un nuevo proyecto git ya teniendo creado el repositorio desde github

cd "c:\ProyectosAL\Ejercicios"

git status

git init

git config user.name "SalvaLiderIt"

git config user.email "spavila@liderit.es"

git remote add origin https://github.com/SalvaLiderIt/Exercises.git

git remote -v

git add .

git commit -m "Initial commit: Setup AL exercises repository with MarcasYSabores project"

git push -u origin main

//para hacer las subidas ya simplemente usar los siguientes comandos
git add .

git commit -m "Descripción de tus cambios"

git push


```

Para volver atrás en el tiempo al commit anterior GOD
1. **`git status`** - Para ver qué archivos habían cambiado
2. **`git restore .`** - Para descartar TODOS los cambios y volver al último commit
3. **`git status`** - Para confirmar que el working tree está limpio

 Tipos de Ramas en Git

![[Pasted image 20251022120220.png]]

![[Pasted image 20251022120519.png]]

![[Pasted image 20251022120600.png]]

![[Pasted image 20251022120654.png]]

Cuando estoy trabajando en una rama que no es la principal, a la hora de que hay cambios que no tengo implementados, que se han hecho en la principal. Debo de ir a github --> master --> Merge Branch into Current Branch

![[Pasted image 20251022121136.png]]
Por otro lado está el Pull Request, que es al contrario, es un proceso formal en el que se pide que el desarrollo de una rama alternativa, sea incorporado en la rama master. Se hace desde github, tiene el botón "Create pull request".