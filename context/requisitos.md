# Requisitos y dependencias

Este documento reúne los prerequisitos, dependencias y archivos necesarios para instalar y ejecutar el proyecto.

## Prerequisitos (software)

- Python 3.11 o superior

# Requisitos y dependencias (simplificado)

## Prerequisitos

- Python 3.11 o superior
- PostgreSQL 15 o superior (14+ compatible)
- (Opcional) Docker/Docker Compose para levantar PostgreSQL localmente

## Dependencias del proyecto

- flet ~= 0.24.0
- psycopg[binary] ~= 3.1
- python-dotenv ~= 1.0

> Son las mínimas necesarias para una app Flet con PostgreSQL y configuración por `.env`.

## Archivos necesarios

- requirements.txt — dependencias del proyecto
- .env.example — ejemplo de variables de entorno
- docker-compose.yml — (opcional) servicio de PostgreSQL en desarrollo

Sugerencia de `requirements.txt` (actual):

```
flet~=0.24.0
psycopg[binary]~=3.1
python-dotenv~=1.0
```

## Variables de entorno

Crear un archivo `.env` en la raíz del proyecto (basado en `.env.example`) con, al menos:

```
DATABASE_URL=postgresql://usuario:password@localhost:5432/servicio_db
APP_ENV=development
FLET_PORT=8550
```

Observaciones:

- Ajustar usuario, contraseña y puerto según tu entorno.

## Instalación rápida (Linux)

1. Crear y activar entorno virtual:
   - `python3 -m venv .venv`
   - `source .venv/bin/activate`
2. Instalar dependencias:
   - `pip install -r requirements.txt`
3. Configurar PostgreSQL y `DATABASE_URL`.
4. (Opcional) Levantar BD con Docker: `docker compose up -d db`.

## Compatibilidad

- Sistemas operativos: Linux, Windows, macOS.

## Notas

- Los campos definitivos provendrán del PDF; se generará un script SQL inicial (CREATE TABLE) cuando se confirme el desglose de campos.
  > Son las mínimas necesarias para una app Flet con PostgreSQL y configuración por `.env`.
