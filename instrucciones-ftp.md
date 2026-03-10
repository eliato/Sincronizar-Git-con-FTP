# 🚀 Guía: Sincronizar GitHub con FTP (Despliegue Continuo)

Esta guía explica cómo automatizar la subida de archivos a un servidor FTP cada vez que se hace un `push` a la rama principal en GitHub, utilizando **GitHub Actions**.

---

## 🛠️ Paso 1: Configurar los Secretos en GitHub

Por seguridad, nunca debes poner las credenciales del FTP directamente en el código. Para eso usamos los "Secrets" de GitHub.

1. Ve a tu repositorio en **GitHub**.
2. Dirígete a **Settings** (Configuración) > **Secrets and variables** > **Actions**.
3. Haz clic en el botón verde **New repository secret** y crea estos tres secretos:
   * `FTP_SERVER`: La dirección de tu servidor (ej. `ftp.tudominio.com`).
   * `FTP_USERNAME`: Tu nombre de usuario del FTP.
   * `FTP_PASSWORD`: Tu contraseña del FTP.

---

## 📂 Paso 2: Crear el Archivo de Flujo de Trabajo (Workflow)

Debemos indicarle a GitHub qué debe hacer cuando subimos código. 

1. En la raíz de tu proyecto local, crea la siguiente estructura de carpetas:
   ```bash
   mkdir -p .github/workflows

## Paso 3: Crea un archivo llamado main.yml dentro de esa carpeta:
con el siguiente comando:
    ```bash
    touch .github/workflows/main.yml
agrega el siguiente contenido:
    ```bash
name: Deploy Website

on:
  push:
    branches:
      - main  # Cambia a 'master' si tu rama principal se llama así

jobs:
  web-deploy:
    name: 🎉 Deploy
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
        # IMPORTANTE: La carpeta destino debe terminar SIEMPRE con una barra diagonal (/)
        server-dir: ./public_html/    

**(Nota: Si quieres subir los archivos a la raíz principal del FTP y no a una subcarpeta, debes poner server-dir: ./)**

## 🚀 Paso 4: Guardar y Subir (Commit & Push)
Ahora solo tienes que registrar los cambios en Git y subirlos a GitHub. En tu terminal ejecuta:
   ```bash
git add .
git commit -m "Configura GitHub Actions para FTP"
git push origin main


**¡Listo! A partir de ahora, ve a la pestaña "Actions" en tu repositorio de GitHub. Verás cómo empieza a ejecutarse el proceso y a subir tus archivos automáticamente.**