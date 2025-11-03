# Arquitectura del Sistema (simplificada)

Este documento resume la arquitectura para un proyecto pequeño, de uso en una única institución (guardería). Se priorizan la simplicidad y la rapidez de implementación.

## Decisiones clave

- Lenguaje: Python 3.11+
- Interfaz: Flet (app de escritorio)
- Base de datos: PostgreSQL 15+
- Acceso a datos: psycopg 3 con SQL simple (sin ORM)
- Configuración: variables de entorno con `python-dotenv`
- Autenticación: login simple con un único usuario (credenciales en `.env`)

## Vista de componentes (alto nivel)

```mermaid
flowchart LR
  subgraph App[Aplicación de Escritorio (Flet)]
    UI[UI Flet]
    Logic[Lógica simple (búsqueda/filtros)]
    Data[Consultas SQL (psycopg)]
  end

  DB[(PostgreSQL)]

  UI <--> Logic
  Logic <--> Data
  Data <--> DB
```

## Pantallas y flujo funcional

1. Login simple (usuario único).
2. Listado de alumnos:

- Filtro por grado/sección.
- Buscador por nombre con filtrado en vivo (tras cada carácter tecleado).
- Botón “Agregar alumno”.
- Por alumno en la lista: botones “Editar” y “Eliminar”.
- La tarjeta/fila del alumno muestra campos principales (p. ej. Nombre y Apellido, Grado/Sección, Fecha de nacimiento/Edad) para consulta rápida.

3. Detalle de alumno:
   - Secciones plegables para completar/editar:
     - Datos personales del alumno.
     - Datos médicos del alumno.
     - Datos del padre.
     - Datos de la madre.
     - Datos del embarazo.
     - Datos del representante autorizado.
   - Ver detalle de campos en `context/campos.md`.

## Persistencia: esquema inicial (borrador y simple)

- alumnos(
  id PK, nombres, apellidos, fecha_nacimiento, grado, seccion,
  direccion, telefono_contacto, creado_en, actualizado_en
  )
- datos_medicos(
  id PK, alumno_id FK, alergias, condiciones, medicamentos, observaciones
  )
- tutores(
  id PK, alumno_id FK, nombre, telefono, email, parentesco -- valores: PADRE/MADRE/REPRESENTANTE
  )
- datos_embarazo(
  id PK, alumno_id FK, semanas_gestacion, complicaciones, observaciones
  )

Notas:

- Esquema mínimo; se ajustará con el PDF de campos definitivos.
- Índices básicos por alumno_id y (opcional) índice por nombre/apellido para la búsqueda.

## Consideraciones

- Sin roles ni permisos avanzados; un único usuario para la directiva.
- Backups de BD recomendados pero fuera del alcance del MVP.

## Referencias

- Definición de módulos y campos: `context/campos.md` (versión inicial, sujeta a cambios).
- Diseño de base de datos: `context/base_de_datos.md` (tablas, PK/FK, relaciones, normalización).
- Esquema visual (DBML): `context/esquema.dbml` (importable en dbdiagram.io).

## Roadmap breve

- v0.1: Login simple, listado con filtro/búsqueda, alta de alumno.
- v0.2: Detalle con secciones plegables y persistencia por secciones.
- v0.3: Exportaciones simples (CSV) opcionales.
