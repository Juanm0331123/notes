# Estructura y Notas de Scrapers

## Estructura de directorio Tree para un Scraper (puede variar):
´´´bash
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
│   └── drive_manager.py     # ¡NUEVO! Lógica para subir/bajar archivos de Drive
└── data/                    # Volúmenes de Docker se montarán aquí
    ├── raw/
    └── processed/
´´´
