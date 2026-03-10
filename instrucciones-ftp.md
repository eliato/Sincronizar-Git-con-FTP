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