Mi tenant del CDX  --> CRMbc727119

Tutorial Lider --> 
http://sdesgh01:8080/desarrollo/Entornos/EntornoCDX/?h=cdx 

Credenciales CDX Tenant https://cdx.transform.microsoft.com/dashboard/tenant-details/e9b6c7f4-0953-485d-b98f-a47b810b30e2
- TenantSalva  --> ==CRMbc727119.onmicrosoft.com== 
	- Admin Name admin@CRMbc727119.onmicrosoft.com  
	- email admin@CRMbc727119.onmicrosoft.com 
		- Password:            ==YUV][bD=~38b71d:QaNrQcP#o872#{3k==
	
	- User Password:    c}10xPD5%af;Na6:DT60=~r8~BLFbn9% 

Link BC, TenantSalva --> https://businesscentral.dynamics.com/?redirectedFromSignup=1&ScenarioId=Signup 

Entorno Produccion TenantSalva --> https://businesscentral.dynamics.com/1854fb16-e91a-410f-bb19-e2fb2e1aa10c/Production 


## 🧩 Cómo extraer los datos del `launch.json` desde la página de CDX

| Campo `launch.json`   | Dónde encontrarlo en la página CDX                                                                               |
| --------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `"server"`            | Siempre es `https://businesscentral.dynamics.com` para entornos CDX SaaS.                                        |
| `"serverInstance"`    | Normalmente `"sandbox"` para CDX, salvo que se indique otro nombre en el entorno.                                |
| `"tenant"`            | Lo ves en el correo del admin: `admin@CRMbc727119.onmicrosoft.com` → el tenant es `CRMbc727119.onmicrosoft.com`. |
| `"authentication"`    | `"AAD"` porque CDX usa Azure Active Directory.                                                                   |
| `"startupObjectId"`   | Puedes usar `22` para abrir la lista de clientes (Customer List) como punto de partida.                          |
| `"startupObjectType"` | `"Page"` si usas una página como objeto inicial.                                                                 |
| `"name"`              | Puedes poner `"CDX"` o `"CDX TenantSalva"` para identificarlo.                                                   |


## 🧪 Ejemplo completo de `launch.json` para tu CDX

json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "CDX TenantSalva",
      "type": "al",
      "request": "launch",
      "server": "https://businesscentral.dynamics.com",
      "serverInstance": "sandbox",
      "tenant": "CRMbc727119.onmicrosoft.com",
      "authentication": "AAD",
      "startupObjectId": 22,
      "startupObjectType": "Page",
      "breakOnError": true,
      "schemaUpdateMode": "Recreate"
    }
  ]
}
```


La prueba primera de proyecto vacío que me ha permitido descargar símbolos y publicar en mi CDX tiene lo siguiente tanto en launch.json como en app.json

Launch.json
```json
{

    "version": "0.2.0",

    "configurations": [

        {

            "name": "CDX TenantSalva",

            "type": "al",

            "request": "launch",

            "environmentName": "UTY",

            "server": "https://businesscentral.dynamics.com",

            //"serverInstance": "sandbox",

            "tenant": "CRMbc727119.onmicrosoft.com",

            "launchBrowser": true,

            "authentication": "AAD",

            "startupObjectId": 22,

            "startupObjectType": "Page",

            "breakOnError": true,

            "schemaUpdateMode": "ForceSync"

        },

        /* {

            "name": "Desarrollo",

            "type": "al",

            "request": "launch",

            "environmentName": "Desarrollo",

            "startupObjectId": 27,

            "breakOnError": "All",

            "breakOnRecordWrite": "None",

            "launchBrowser": true,

            "enableSqlInformationDebugger": true,

            "tenant": "b4be8f5e-c104-4fa9-a40f-f8256b71056d",

            "enableLongRunningSqlStatements": true,

            "longRunningSqlStatementsThreshold": 500,

            "numberOfSqlStatements": 10,

            //"schemaUpdateMode": "ForceSync"

        }, */

    ]

}
```

app.json
```json
{

  "id": "b246773f-e2be-4f43-a4fe-c4be003284eb",

  "name": "TestLocal",

  "publisher": "Default Publisher",

  "version": "1.0.0.0",

  "runtime": "14.0",

    "screenshots": [],

    "platform": "1.0.0.0",

    "application": "27.0.0.0",

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

Importante:
**DEPENDENCIAS** de Chalupa, apps descarga de la tienda LINKs (Cambiar TENANTID y NOMBRE_ENTORNO por el correspondiente), el primer enlace tiene a modo de ejemplo mi tenant y el entorno "UTY".

CRMbc727119.onmicrosoft.com 

https://businesscentral.dynamics.com/CRMbc727119.onmicrosoft.com/DesarrolloVS?aid= FIN&page=2503&filter=%27ID%27%20IS%20%27fb9f1c25-c1a1-4524-859a-26157abd2e58%27&signInRedirected=1


TC-Core
https://businesscentral.dynamics.com/CRMbc727119.onmicrosoft.com/UTY?aid= FIN&page=2503&filter=%27ID%27%20IS%20%27bc804f5a-e849-4369-aaa7-bb44ce5ea2af%27&signInRedirected=1

TC-File
https://businesscentral.dynamics.com/[TENANTID]/[NOMBRE_ENTORNO]?aid= FIN&page=2503&filter=%27ID%27%20IS%20%2756db1649-b574-40a6-8288-079e4628bfc7%27&signInRedirected=1

TC-FTP
https://businesscentral.dynamics.com/[TENANTID]/[NOMBRE_ENTORNO]?aid= FIN&page=2503&filter=%27ID%27%20IS%20%27fb9f1c25-c1a1-4524-859a-26157abd2e58%27&signInRedirected=1




==Ya tengo prácticamente todo lo del CDX operativo, falta cuadrar las versiones de las extensiones, instalando los .app directamente en el cliente web, una vez tenga eso ya podré usarlo. Esto de Chalupa==


### CDX Dependencias* Arruzafa
Link Instalación desde la microsoft store de extension lider SII. Recordar modificar la URL con respecto a las URL anteriores con el tenant y el entorno para poder instalar https://statics.teams.cdn.office.net/evergreen-assets/safelinks/2/atp-safelinks.html