## 1. 🔍 Obtener los Archivos de Dependencia (`.app`)

Este es el primer y más importante paso. Necesitas las versiones exactas de los archivos binarios compilados de las extensiones que has listado en tu `app.json`.

|**Dependencia**|**Editor**|**Versión Requerida (según tu app.json)**|
|---|---|---|
|**SGA_v2**|LiderIT|1.21.0.1|
|**IEPNR**|LiderIT|20.2.32.0|
|**LiderGeneral**|LiderIT|1.0.0.0|
|**RLDR**|Alejandro J de Tena|1.0.0.0|
|**Incidencias**|LiderIT|1.0.0.2|

### 🚨 Acción Requerida: Preguntar a tus Compañeros

Debes contactar a tus compañeros o al equipo de administración de AL/DevOps y pedirles explícitamente los **archivos `.app`** (los paquetes de la extensión) para las versiones exactas listadas arriba.

- **Petición clave:** "Necesito las extensiones compiladas (archivos `.app`) de las siguientes dependencias para publicarlas en mi instancia de Service Tier local (`BC260`)."
    

Una vez que tengas esos archivos (ej. `SGA_v2_1.21.0.1.app`, `IEPNR_20.2.32.0.app`, etc.), guárdalos en una carpeta fácil de acceder en tu VM, por ejemplo: `C:\AL_Dependencies`.

---

## 2. 🚀 Publicación de las Extensiones en el Service Tier Local (Método recomendado: PowerShell)

El método más robusto y rápido para publicar las dependencias es usando el **Business Central Administration Shell (BCAS)**.

### A. Preparación del Shell

1. Abre el menú de Windows de tu VM.
    
2. Busca y ejecuta el **"Dynamics 365 Business Central Administration Shell"** como **Administrador**.
    

### B. Publicar la Aplicación (Extensión)

Para cada uno de los archivos `.app` que has obtenido, ejecutarás el siguiente comando.

> **¡Importante!** El comando debe apuntar a la versión de tu servidor (`BC260`) y a la ruta donde guardaste el archivo `.app`.

#### 📝 Sintaxis del Comando

PowerShell

```
Publish-NAVApp -ServerInstance <NombreInstancia> -Path "<RutaCompletaAlArchivo.app>" -SkipVerification -Force
```

#### 💡 Ejemplo Práctico (para `SGA_v2`):

Si el archivo `SGA_v2_1.21.0.1.app` está en `C:\AL_Dependencies`, y tu instancia es `BC260`, el comando será:

PowerShell

```
Publish-NAVApp -ServerInstance BC260 -Path "C:\AL_Dependencies\SGA_v2_1.21.0.1.app" -SkipVerification -Force
```

**Repite este comando para cada uno de los 5 archivos `.app` de tus dependencias.**

### C. Sincronizar (Suele ser automático, pero verifica)

Generalmente, el comando `Publish-NAVApp` también sincroniza la extensión con la base de datos.

### D. Instalar la Aplicación (Extensión)

Después de publicar, debes asegurarte de que la extensión esté **instalada** en tu _Tenant_ (la base de datos de tu Service Tier).

#### 📝 Sintaxis del Comando

PowerShell

```
Install-NAVApp -ServerInstance <NombreInstancia> -Name <NombreExtension> -Version <VersionExtension>
```

#### 💡 Ejemplo Práctico (para `SGA_v2`):

Necesitas usar el nombre y la versión exacta de tu `app.json`:

PowerShell

```
Install-NAVApp -ServerInstance BC260 -Name "SGA_v2" -Version "1.21.0.1"
```

**Repite este comando para cada una de las 5 extensiones.**

---

## 3. ✅ Descargar los Símbolos en VS Code

Una vez que todas las dependencias estén publicadas e instaladas en tu Service Tier local:

1. Abre VS Code y asegúrate de que la configuración de **`localhost`** esté activa en tu `launch.json`.
    
2. Abre la Paleta de Comandos (**Ctrl + Shift + P**).
    
3. Ejecuta **`AL: Descargar símbolos de la aplicación`**.
    

Esta vez, el proceso debería completarse con éxito, y verás los archivos de símbolos de todas tus dependencias aparecer en la carpeta **`.alpackages`** de tu proyecto.

---

### Resumen de tu Lista de Tareas:

|**Paso**|**Tarea**|**Responsable**|
|---|---|---|
|**1. Obtener Archivos**|Solicitar a tus compañeros los **5 archivos `.app`** de las versiones exactas requeridas.|Tú / Equipo de Desarrollo|
|**2. Publicar**|Ejecutar el comando `Publish-NAVApp` en el BCAS para los 5 archivos.|Tú|
|**3. Instalar**|Ejecutar el comando `Install-NAVApp` en el BCAS para las 5 extensiones.|Tú|
|**4. Descargar Símbolos**|Ejecutar **`AL: Descargar símbolos de la aplicación`** en VS Code.|Tú|

Una vez hecho esto, deberías poder presionar F5 y publicar tu código en tu entorno local sin problemas. ¿Quieres que te ayude a redactar la solicitud a tus compañeros?