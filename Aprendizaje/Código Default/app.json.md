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