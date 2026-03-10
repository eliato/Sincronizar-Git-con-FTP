# 🚀 Guía: Despliegue Automático de Git a FTP (GitHub Actions)

Esta guía te permitirá sincronizar tu código automáticamente con tu servidor cada vez que hagas un `push` a la rama principal de GitHub.

---

## 🛠️ Paso 1: Configurar Secretos en GitHub

Para no exponer tus contraseñas en el código, configuraremos variables de entorno seguras.

1. Ve a tu repositorio en **GitHub**.
2. Entra en **Settings** > **Secrets and variables** > **Actions**.
3. Crea los siguientes **Repository secrets**:
   * `FTP_SERVER`: Dirección del servidor (ej: `ftp.tusitio.com`).
   * `FTP_USERNAME`: Tu usuario de FTP.
   * `FTP_PASSWORD`: Tu contraseña de FTP.

---

## 📂 Paso 2: Crear el Archivo de Configuración

Ejecuta estos comandos en la terminal de tu proyecto para crear la estructura necesaria:

```bash
# Crea las carpetas del flujo de trabajo
mkdir -p .github/workflows

# Crea el archivo de configuración
touch .github/workflows/main.yml

```
---
## Pega el siguiente contenido exacto (ajustando la carpeta de destino):

```yml
name: 🚀 Deploy Website via FTP

on:
  push:
    branches:
      - main  # Asegúrate de que este sea el nombre de tu rama principal

jobs:
  web-deploy:
    name: 🎉 Deploying...
    runs-on: ubuntu-latest
    steps:
    - name: 🚚 Get latest code
      uses: actions/checkout@v4

    - name: 📂 Sync files
      uses: SamKirkland/FTP-Deploy-Action@v4.3.5
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        # IMPORTANTE: La ruta DEBE terminar con una barra diagonal (/)
        server-dir: ./public_html/
```
---
## 🚀 Paso 4: Guardar y Subir (Commit & Push)