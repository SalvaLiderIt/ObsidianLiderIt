```al
{

  "id": "05da4847-8c7b-4095-a75b-9ab4165a6bd1",
  "name": "MiPrimeraExtension",
  "publisher": "Default Publisher",
  "version": "1.0.0.0",
  "brief": "",
  "description": "",
  "privacyStatement": "",
  "EULA": "",
  "help": "",
  "url": "",
  "logo": "",
  "dependencies": [],
  "screenshots": [],
  "platform": "1.0.0.0",
  "application": "1.0.0.0",
  "idRanges": [
    {
      "from": 50100,
      "to": 50149
    }
  ],
  "resourceExposurePolicy": {
    "allowDebugging": true,
    "allowDownloadingSource": true,
    "includeSourceInSymbolFile": true
  },
 "runtime": "15.2",
  "features": ["TranslationFile"],
  "suppressWarnings": ["AS0051","AS0052","AS0084"]//esta linea para suprimir warnings, aunque mejor hacerlo en el lider.ruleset.json dentro de .codeanalysis
}
```

Puedo agregar una línea que me permita "silenciar" warnings que me da el editor, la agrego al final del anterior fragmento de código y la comento 
Véase archivo [[Dependencias]] 


app.json Chalupa

```json
{

    "id": "6315f7d9-bbba-463e-8e90-4e2b2a96f4b1",

    "name": "Chalupa",

    "publisher": "LiderIT",

    "version": "1.0.0.42",

    "brief": "Funcionalidad base",

    "description": "Funcionalidad base",

    "privacyStatement": "http://www.liderit.es",

    "EULA": "http://www.liderit.es",

    "help": "http://www.liderit.es",

    "url": "http://www.liderit.es",

    "runtime": "14.0",

    "logo": "logo.png",

    "dependencies": [

        {

            "id": "E40D1B38-F48E-4D64-9797-4165784DDF8D",

            "name": "SGA_v2",

            "publisher": "LiderIT",

            "version": "1.21.0.1"

        },

        {

            "id": "1796053f-37a0-46cb-8910-d17d50a840ce",

            "name": "IEPNR",

            "publisher": "LiderIT",

            "version": "20.2.32.0"

        },

        {

            "id": "30c5a3d6-3a0f-49a3-bda0-ab49563e6b47",

            "name": "LiderGeneral",

            "publisher": "LiderIT",

            "version": "1.0.0.0"

        },

        {

            "id": "1129f626-04df-4b2a-a01e-fd8527600230",

            "name": "RLDR",

            "publisher": "Alejandro J de Tena",

            "version": "1.0.0.0"

        },

        {

            "id": "3883e1df-8ebc-477f-afc0-aa65e9e51b29",

            "name": "Incidencias",

            "publisher": "LiderIT",

            "version": "1.0.0.2"

        }

    ],

    "screenshots": [],

    "platform": "1.0.0.0",

    "application": "25.0.0.0",

    "idRanges": [

        {

            "from": 50000,

            "to": 50149

        }

    ],

    "features": [

        "TranslationFile",

        "NoImplicitWith"

    ],

    "target": "Cloud",

    "resourceExposurePolicy": {

        "allowDebugging": true,

        "allowDownloadingSource": false,

        "includeSourceInSymbolFile": false

    }

}
```