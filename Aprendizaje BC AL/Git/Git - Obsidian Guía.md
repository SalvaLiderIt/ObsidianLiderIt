# 🧠 Guía de sincronización Obsidian + GitHub (Windows)

Esta nota explica cómo instalar Git, configurar Obsidian Git y vincular tu bóveda con el repositorio `SalvaLiderIt/ObsidianLiderIt`.

---

## 🛠 1. Instalar Git en Windows

1. Ve a [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Ejecuta el instalador.
3. En el paso **“Adjusting your PATH environment”**, selecciona:
   - ✅ *Git from the command line and also from 3rd-party software*
4. Finaliza la instalación.

Verifica que Git está instalado:
```powershell
git --version
```


## 📁 2. Clonar el repositorio en otro PC

1. Abre PowerShell o Terminal.
    
2. Navega a la carpeta donde quieres tu bóveda:
    

powershell

```
cd "C:\Users\TU_USUARIO\Documents"
```

3. Clona el repositorio:
    

bash

```
git clone https://github.com/SalvaLiderIt/ObsidianLiderIt.git
```

4. Abre esa carpeta como bóveda en Obsidian.
    

## 🔌 3. Configurar el plugin Obsidian Git

### A. Instalar el plugin

- Obsidian → Settings → Community Plugins → Browse → **Obsidian Git**
    

### B. Configurar ruta de Git

- Ve a: Settings → Obsidian Git → Options
    
- En **Custom Git binary path**, pon:
    

Código

```
C:\Users\TU_USUARIO\AppData\Local\Programs\Git\cmd\git.exe
```

Recordar, abrir la bóveda de Obsidian abajo a la izquierda 
- ![[Pasted image 20251127133452.png]] 
### C. Activar automatización

- Vault backup interval: `10`
    
- Auto pull interval: `15`
    
- Auto pull on startup: ✅
    
- Show status bar: ✅
    

## 🔐 4. Autenticación con GitHub

### Opción A: HTTPS + Token

1. Ve a GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
    
2. Crea un token con permisos `repo`
    
3. Cuando Obsidian Git te pida credenciales:
    
    - Usuario: tu nombre de GitHub
        
    - Contraseña: el token
        

### Opción B: SSH (más cómodo)

1. Genera una clave SSH:
    

bash

```
ssh-keygen -t ed25519 -C "tu@email.com"
```

2. Añade la clave pública a GitHub → Settings → SSH keys
    
3. Cambia el remoto:
    

bash

```
git remote set-url origin git@github.com:SalvaLiderIt/ObsidianLiderIt.git
```

## 📦 5. Comandos básicos

bash

```
git status           # Ver cambios
git add .            # Añadir todos los archivos
git commit -m "Mensaje"  # Crear commit
git push             # Subir al repositorio
git pull             # Traer cambios
```

## 🧪 6. Probar desde Obsidian

- Abre la Command Palette (`Ctrl+P`)
    
- Ejecuta:
    
    - `Obsidian Git: Commit all changes`
        
    - `Obsidian Git: Push`
        
    - `Obsidian Git: Pull`
        

## ✅ Resultado esperado

- Tu bóveda se sincroniza con GitHub.
    
- Puedes trabajar desde varios PCs sin perder cambios.
    
- Tienes historial de versiones y control total.