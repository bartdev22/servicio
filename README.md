# Servicio — Sistema de Gestión Digital

Sistema de escritorio para la gestión del registro de datos personales, médicos y de desarrollo infantil de los niños del Preescolar Patriotas de Venezuela, desarrollado en Python con Flet y PostgreSQL.

## Contenido

- [Descripción](#descripción)
- [Stack y decisiones](#stack-y-decisiones)
- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Configuración](#configuración)
- [Ejecución](#ejecución)
- [Estructura propuesta](#estructura-propuesta)
- [Documentos de contexto](#documentos-de-contexto)

## Descripción

Este proyecto busca modernizar la gestión de expedientes estudiantiles mediante una aplicación sencilla y portable, enfocada en mejorar el acceso, la integridad y la seguridad de la información.

## Stack y decisiones

- Lenguaje: Python 3.11+
- UI: Flet (aplicación de escritorio en Python)
- Base de datos: PostgreSQL 15+
- Acceso a datos: psycopg 3 (binario) con SQL simple
- Configuración: `python-dotenv`

> Ver más detalles en `context/arquitectura.md`.

## Requisitos previos

- Python 3.11 o superior
- PostgreSQL 15 o superior
- Git

Opcional: Docker/Docker Compose para levantar PostgreSQL localmente.

## Instalación

1. Clonar el repositorio y entrar a la carpeta del proyecto.
2. Crear y activar un entorno virtual:
   - Linux/macOS: `python3 -m venv .venv && source .venv/bin/activate`
3. Instalar dependencias:
   - `pip install -r requirements.txt`

> Las versiones sugeridas de dependencias están documentadas en `context/requisitos.md`.

## Configuración

Crear un archivo `.env` en la raíz (basado en `.env.example`) con al menos:

```
DATABASE_URL=postgresql://usuario:password@localhost:5432/servicio_db
APP_ENV=development
FLET_PORT=8550
```

Configurar la base de datos PostgreSQL y crear la base `servicio_db` (o el nombre que se defina).

## Ejecución

Durante la etapa inicial (sin empaquetado):

```
python main.py
```

Opcionalmente, si se configura entrada con Flet:

```
python -m flet run main.py
```

### (Opcional) Base de datos con Docker Compose

1. Copia `.env.example` a `.env` y ajusta las variables si lo deseas.
2. Levanta PostgreSQL:

```
docker compose up -d db
```

3. Verifica la salud del contenedor y prueba la conexión desde la app con el botón "Probar conexión a BD".

## Alcance y funcionalidades (MVP)

- Listado de alumnos con:
  - Filtro por grado/sección.
  - Barra de búsqueda por nombre con filtrado en vivo (tras cada carácter). Los que coinciden permanecen en pantalla; el resto se ocultan.
- Botón para agregar alumno (en la vista principal).
- Por cada alumno: botones de Editar y Eliminar.
- La tarjeta/fila de cada alumno muestra campos principales para consulta rápida (p. ej. Nombre y Apellido, Grado/Sección, Fecha de nacimiento o Edad).
- Vista de detalle del alumno con secciones plegables para completar datos:
  - Datos personales del alumno.
  - Datos médicos del alumno.
  - Datos del padre.
  - Datos de la madre.
  - Datos del embarazo.
  - Datos del representante autorizado.
- Login simple con un único usuario (credenciales básicas definidas en configuración).

## Estructura propuesta

```
servicio/
├─ context/
│  ├─ arquitectura.md
│  ├─ proyect.md
│  └─ requisitos.md
├─ app/                 # Código fuente (simple)
│  ├─ ui/               # Vistas y componentes Flet
│  ├─ core/             # Lógica sencilla (búsqueda, filtros)
│  ├─ infra/            # Acceso a BD con psycopg
│  └─ config/           # Carga de .env, logging
├─ requirements.txt     # Dependencias mínimas
├─ .env.example         # Variables de entorno ejemplo
├─ docker-compose.yml   # Servicio PostgreSQL para desarrollo
└─ main.py              # Punto de entrada Flet
```

## Documentos de contexto

- `context/proyect.md` — Contexto y justificación del proyecto.
- `context/arquitectura.md` — Arquitectura propuesta y decisiones.
- `context/requisitos.md` — Prerequisitos, dependencias y archivos necesarios.
- `context/campos.md` — Módulos y campos del alumno (versión inicial).
- `context/base_de_datos.md` — Diseño relacional (tablas, PK/FK, relaciones, normalización 4NF).
- `context/esquema.dbml` — Esquema en DBML para visualizar en dbdiagram.io.

---

Licencia y lineamientos de contribución se documentarán cuando el proyecto avance a una versión inicial ejecutable.
