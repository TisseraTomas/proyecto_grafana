# Solución de Observabilidad: Telegraf + InfluxDB + Grafana

Este repositorio contiene la capa de visualización y alertas del Trabajo Práctico Integrador de la materia SOA / Servicios Web y Sistemas Distribuidos.

El proyecto implementa un entorno de Grafana 100% reproducible mediante Docker Compose y aprovisionamiento automático (Provisioning), conectado a una base de datos InfluxDB 2.x remota.

## Arquitectura

La solución se divide en dos componentes principales:
1. **Recolección y Almacenamiento (Remoto):** Agente Telegraf e InfluxDB alojados en una Máquina Virtual.
2. **Visualización y Alertas (Local):** Servidor Grafana desplegado mediante contenedores, consultando la API remota de InfluxDB de forma directa mediante peticiones HTTP.

## Estructura del Proyecto

El proyecto sigue el paradigma de "Infraestructura como Código", automatizando la creación de conexiones, tableros y alertas:

```text
├── .env.example                 # Plantilla de variables de entorno (Credenciales)
├── .gitignore                   # Archivos ignorados por Git
├── docker-compose.yml           # Orquestador de contenedores
├── README.md                    # Este documento
└── grafana/
    └── provisioning/            # Aprovisionamiento automático (Plug & Play)
        ├── alerting/            # Reglas de alertas, contact points y policies
        ├── dashboards/          # Tableros JSON y configuración de carpetas
        └── datasources/         # Conexión declarativa a InfluxDB
```
## Requisitos Previos
Para ejecutar este proyecto, necesitas tener instalado en tu entorno local:

- Docker

- Docker Compose

## Instrucciones de Despliegue (Plug & Play)
Sigue estos pasos para levantar el entorno completo de forma automatizada:

1. Clonar el repositorio:

```Bash
git clone https://github.com/TisseraTomas/proyecto_grafana.git
cd proyecto_grafana
```
2. Configurar credenciales seguras:
Copia el archivo de ejemplo para crear tu entorno local de variables:

```Bash
cp .env.example .env
```
Edita el archivo ```.env``` recién creado con tu editor de texto y completa:

- Tu ```USER``` y ```PASSWORD``` deseados para administrar Grafana.

- El ```INFLUX_TOKEN``` provisto por el equipo administrador de la base de datos.

- El ```GF_SMTP_USER``` y ```GF_SMTP_FROM_ADDRESS``` (tu dirección de correo electrónico) junto con el ```GF_SMTP_PASSWORD``` (tu contraseña de aplicación de Gmail) para habilitar el enrutamiento de las alertas.

Desplegar los contenedores:
3. Ejecuta Docker Compose en modo detached (segundo plano):

```Bash
docker-compose up -d
```
4. Acceder a la plataforma:
Abre tu navegador web e ingresa simplemente a localhost.

Nota de Seguridad: NGINX interceptará la petición en el puerto 80 y te redirigirá automáticamente a la versión segura ```https://localhost``` (puerto 443). Dado que la plataforma ya incluye certificados SSL auto-firmados en la carpeta ```nginx/certs```, el navegador mostrará una advertencia de seguridad inicial. Por favor, selecciona 'Configuración Avanzada' y 'Continuar' para acceder a la interfaz encriptada.

5. Inicia sesión:
Utiliza las credenciales que definiste en el archivo ```.env```. El tablero principal, la conexión a la base de datos y las reglas de notificación por correo ya estarán completamente operativos.