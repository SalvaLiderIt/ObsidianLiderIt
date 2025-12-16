```al
{

    "version": "0.2.0",

    "configurations": [

        {

            "name": "localhost",

            "request": "launch",

            "type": "al",

            "environmentType": "Sandbox",

            "server": "http://localhost:8080/BC260/",

            "serverInstance": "BC260",

            "authentication": "Windows",

            //"startupObjectId": 22,

            //"startupObjectType": "Table",

            "breakOnError": "All",

            // "launchBrowser": true,

            "enableLongRunningSqlStatements": true,

            "enableSqlInformationDebugger": true,

            "tenant": "default",

            "usePublicURLFromServer": true
            // "schemaUpdateMode": "ForceSync"//probar esta línea para forzar algo si no funciona, NO USAR EN DESARROLLO, BORRA COSAS

        }

    ]

}
```

Launch de proyectos de Lider reales

```json
{

    "version": "0.2.0",

    "configurations": [

        {

            "name": "localhost",

            "request": "launch",

            "type": "al",

            "environmentType": "Sandbox",

            "server": "http://localhost:8080/BC260/",

            "serverInstance": "BC260",

            "authentication": "Windows",

        },

        {

            "name": "Desarrollo",

            "type": "al",

            "request": "launch",

            "environmentName": "Desarrollo",

            "startupObjectId": 9307,

            "breakOnError": "All",

            "breakOnRecordWrite": "None",

            "launchBrowser": true,

            "enableSqlInformationDebugger": true,

            "tenant": "b4be8f5e-c104-4fa9-a40f-f8256b71056d",

            "enableLongRunningSqlStatements": true,

            "longRunningSqlStatementsThreshold": 500,

            "numberOfSqlStatements": 10,

        },

    ]

}
```

launch.json chalupa
```json
{

    "version": "0.2.0",

    "configurations": [

        {

            "name": "localhost",

            "request": "launch",

            "type": "al",

            "environmentType": "Sandbox",

            "server": "http://localhost:8080/BC260/",

            "serverInstance": "BC260",

            "authentication": "Windows",

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