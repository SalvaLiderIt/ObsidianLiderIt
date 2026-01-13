
Para que los archivos creen los nombres automáticamente hay que irse a F1 --> Settings UI --> Workspace --> Extensions --> Papelito --> poner el siguiente código dentro del archivo 

```al
{

    "folders": [

        {

            "name": "MainApp",

            "path": "MainApp"

        },

        {

            "name": "MainAppTest",

            "path": "MainAppTest"

        }

    ],

    "settings": {

        "CRS.OnSaveAlFileAction": "Rename",

        "CRS.FileNamePattern": "<ObjectNameShort>.<ObjectTypeShortPascalCase>.al",

        "CRS.FileNamePatternExtensions": "<ObjectNameShort>.<ObjectTypeShortPascalCase>.al",

        "CRS.FileNamePatternPageCustomizations": "<ObjectNameShort>.<ObjectTypeShortPascalCase>.al"

    }

}
```

![[Pasted image 20251014124418.png]]


el ejemplo de código anterior era el primero en el que aprendí, el siguiente código es el que verdaderamente me va a servir para nuevos workspaces y proyectos

```al
{
    "CRS.OnSaveAlFileAction": "Rename",
    "CRS.FileNamePattern": "<ObjectNameShort>.<ObjectTypeShortPascalCase>.al",
    "CRS.FileNamePatternExtensions": "<ObjectNameShort>.<ObjectTypeShortPascalCase>.al",
    "CRS.FileNamePatternPageCustomizations": "<ObjectNameShort>.<ObjectTypeShortPascalCase>.al"
}
```

