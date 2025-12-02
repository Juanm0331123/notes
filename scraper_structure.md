# Estructura y Notas de Scrapers

## Estructura de directorio Tree para un Scraper (puede variar):
```bash
nombre_proyecto/
├── .gitignore               # CRÍTICO: Ignora credenciales y entornos virtuales
├── .dockerignore            # Optimiza la construcción de la imagen Docker
├── Dockerfile               # Instrucciones para crear el contenedor
├── README.md
├── requirements.txt         # Incluye: selenium, google-api-python-client, pandas...
├── dags/                    # Carpeta para Apache Airflow
│   └── scraper_dag.py       # Define la frecuencia y tareas del DAG
├── config/
│   ├── __init__.py
│   └── settings.py          # Variables de entorno y configuración global
├── credentials/             # ¡NUEVO! Para llaves de Google
│   └── service_account.json # Llave de Google Cloud (¡NUNCA SUBIR A GIT!)
├── src/                     # Código fuente del scraper
│   ├── __init__.py
│   ├── main.py              # Punto de entrada que el DAG o Docker ejecutará
│   ├── scraper.py           # Lógica de Selenium
│   └── drive_manager.py     # Lógica para subir/bajar archivos de Drive
└── data/                    # Volúmenes de Docker se montarán aquí
    ├── raw/
    └── processed/
```

### Ejemplo "real"
```bash
WebScrapingEmssanarBoxalud/
├── .dockerignore            # Configurado para ignorar data/ y venv/
├── .env                     # Variables de entorno (NO subir a Git)
├── .gitignore               # Ignora venv, credentials, data, .env
├── Dockerfile               # Configuración de la imagen con Chrome
├── README.md
├── requirements.txt
├── run.py                   # Entrypoint local (ej: python run.py)
├── config/
│   ├── __init__.py
│   └── settings.py          # Rutas absolutas, config de Drive, constantes
├── credentials/             # Mapeado via Secret en Docker/Airflow
│   └── service_account.json
├── dags/
│   └── scraper_dag.py       # DAG de Airflow que ejecuta el Docker
├── data/                    # ¡IMPORTANTE! Esta carpeta se monta como VOLUMEN
│   ├── downloads/           # AQUÍ se guardan los certificados PDF/ZIP
│   ├── input/               # Aquí cae el Excel descargado de Drive
│   └── state/               # Aquí vive 'progreso_boxsalud.json'
├── logs/
│   └── app.log              # Logs técnicos (errores de sistema/selenium)
└── src/
    ├── __init__.py
    ├── main.py              # Lógica principal (Orquestador)
    ├── scraper.py           # Clase BoxSaludScraper (Selenium puro)
    ├── drive_manager.py     # Clase GoogleDriveManager (API Drive)
    ├── data_manager.py      # Lógica de Excel <-> JSON <-> CSV
    └── utils/
        ├── __init__.py
        ├── logger.py        # Tu clase ScraperLogger (Visualización)
        └── system.py        # Funciones extras (limpieza de archivos, zipear)
```
