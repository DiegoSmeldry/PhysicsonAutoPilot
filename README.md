## ✍️ Autores

Este proyecto fue desarrollado con dedicación por:

*   **Neptali Ramirez** - *Desarrollo del backend y análisis de datos* - [GitHub](https://github.com/Alessandro-45)
*   **Diego Guevara** - *Desarrollo del frontend* - [GitHub](https://github.com/DiegoSmeldry)
*   **Cesar Gutierrez** - *Desarrollo del frontend* - [GitHub](https://github.com/CaesarAlejandro)
*   **Video** - https://www.youtube.com/watch?v=P-LzUlc2kDc


  # Physics on Autopilot: Visualizador de Datos del Bosón de Higgs

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)

Este proyecto es una aplicación web completa que descarga, procesa y visualiza datos abiertos del experimento ATLAS del CERN. El objetivo es reproducir el famoso gráfico de la masa invariante de 4 leptones que muestra la evidencia del bosón de Higgs.

La aplicación está completamente "dockerizada" y diseñada para un despliegue automático en la nube a través de Render.

## 🚀 Tecnologías Utilizadas

*   **Backend:** Python 3.10, FastAPI, Uvicorn
*   **Análisis de Datos:** Uproot, Awkward Array, Vector, AtlasOpenMagic
*   **Frontend:** HTML, CSS, JavaScript (¡listo para que lo implementes!)
*   **Contenerización:** Docker
*   **Despliegue:** Render

## ⚙️ ¿Cómo Funciona?

El proyecto utiliza una arquitectura de microservicios contenida en una única imagen de Docker, construida en dos etapas para optimizar el rendimiento y el tamaño final:

1.  **Etapa de Construcción (`builder`):**
    *   Se instala un entorno de Python completo con todas las librerías de análisis.
    *   Se ejecuta el script `ArchivosPython/analysis.py`. Este script descarga los datos del CERN, los procesa, aplica los cortes físicos necesarios y calcula los histogramas.
    *   El resultado del análisis se guarda en varios archivos `.json`, uno por cada período de datos (2015-2016, 2017, 2018, Total).

2.  **Etapa Final:**
    *   Se utiliza una imagen ligera de Python.
    *   Se instalan únicamente las dependencias del servidor web (`fastapi`, `uvicorn`).
    *   Se copian los archivos del frontend (`Front/`), el servidor (`server.py`) y los archivos `.json` generados en la etapa anterior.
    *   Se inicia un servidor FastAPI que sirve tanto la página web como los datos de los gráficos a través de una API REST.

## 🛠️ Configuración para Desarrollo Local

Para ejecutar el proyecto en tu máquina local, necesitas tener Git y Docker Desktop instalados.

1.  **Clona el repositorio:**
    ```bash
    git clone https://github.com/Alessandro-45/PhysicsonAutoPilot.git
    cd PhysicsonAutoPilot
    ```

2.  **Construye la imagen de Docker:**
    Este comando ejecuta el `Dockerfile`, incluyendo el script de análisis que descarga los datos.
    **Nota:** La opción `--network=host` es **crucial** para permitir que el contenedor acceda a internet durante el `build` y descargue los datos del CERN.

    ```bash
    docker build --no-cache --network=host -t physics-app-local .
    ```
    *Este primer `build` tardará varios minutos.*

3.  **Ejecuta el contenedor:**
    Este comando inicia la aplicación y mapea el puerto 8080 de tu máquina al puerto 8000 del contenedor.

    ```bash
    docker run -p 8080:8000 physics-app-local
    ```

4.  **Accede a la aplicación:**
    Abre tu navegador y ve a **http://localhost:8080**.

## ☁️ Despliegue en Render

Este proyecto está listo para Despliegue Continuo en Render.

1.  **Crea una cuenta en Render** y conéctala a tu cuenta de GitHub.
2.  **Crea un nuevo "Web Service"** y selecciona este repositorio.
3.  **Configuración:**
    *   **Environment:** `Docker` (Render lo detectará automáticamente).
    *   **Start Command:** Déjalo en blanco. Render usará el `CMD` de tu `Dockerfile`.
    *   **Health Check Path:** Puedes usar `/`.
4.  Haz clic en **"Create Web Service"**.

Render construirá y desplegará tu aplicación. El primer despliegue será lento debido al análisis de datos.

**¡Importante!** Cada vez que hagas `git push` a la rama `main`, Render detectará los cambios y desplegará automáticamente una nueva versión de la aplicación.

## 📁 Estructura del Proyecto

```
.
├── ArchivosPython/
│   ├── analysis.py       # Script principal de análisis de datos.
│   └── analysis.ipynb    # Notebook original (referencia).
├── Front/
│   ├── index.html        # Archivo principal del frontend.
│   ├── style.css         # Estilos.
│   └── app.js            # Lógica del cliente.
├── .dockerignore         # Archivos a ignorar por Docker.
├── Dockerfile            # Define el entorno y la construcción de la app.
├── README.md             # Este archivo.
├── requirements.txt      # Dependencias de Python para el análisis.
└── server.py             # Servidor web FastAPI que sirve la API y el frontend.
```
