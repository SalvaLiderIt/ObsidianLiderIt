archivo de permisos necesario para que funcionen los distintos objetos

Este archivo con su contenido correspondiente se puede generar automáticamente gracias al comando AL: Generate permission set as AL object containing current....

```al
{

  "id": "CompBankAccountMgtPermSet",

  "name": "CompBankAccountMgt Permission Set",

  "description": "Permission set for CompBankAccountMgt codeunit.",

  "permissions": [

    {

      "objectType": "Codeunit",

      "objectId": 50100,

      "actions": ["Execute"]

    },

    {

      "objectType": "TableData",

      "objectId": 18,

      "actions": ["Read", "Modify"]

    },

    {

      "objectType": "TableData",

      "objectId": 36,

      "actions": ["Read", "Modify"]

    }

  ]

}
```