# Diseño de base de datos (relacional, 4NF)

Este documento define las entidades, atributos, claves primarias (PK), claves foráneas (FK) y relaciones del sistema. Se prioriza la no redundancia y la flexibilidad para futuras iteraciones.

Referencias de módulos y campos: ver `context/campos.md`.

## Principios y alcance

- Modelo relacional en PostgreSQL.
- Normalización hasta 4ta forma normal (4NF):
  - Atributos multivaluados representados mediante tablas de relación (por ejemplo, alergias, enfermedades, medicamentos autorizados).
  - Datos temporales/recurrentes (inscripción por año) separados en su propia entidad.
- Índices de apoyo para búsqueda por nombre.

## Entidades y tablas

### 1) alumnos

- alumno_id (PK) — entero/autoincremental
- full_name — texto (Nombre completo)
- fecha_nacimiento — date
- lugar_nacimiento — texto
- sexo — enum('M','F','X')
- nacionalidad — texto
- direccion — texto
- creado_en — timestamp
- actualizado_en — timestamp

Notas:

- Índice sugerido: `idx_alumnos_full_name` sobre `full_name` para mejorar la búsqueda por prefijo.

Relaciones:

- 1:N con `inscripciones` (un alumno puede tener múltiples inscripciones a lo largo de los años).
- 1:1 o 1:N con `datos_embarazo` (generalmente 1:1, pero se permite N para auditoría futura si fuera necesario; por ahora 1:1).
- 1:N con `representantes_autorizados`.
- 1:N con `familiares` (rol PADRE/MADRE, y eventualmente REPRESENTANTE si se prefiere unificar).
- 1:1 con `alumno_medico` (atributos simples), más N:M con catálogos de alergias/enfermedades/medicamentos.

### 2) inscripciones

- inscripcion_id (PK)
- alumno_id (FK -> alumnos.id)
- grado — texto/varchar (ej.: "Inicial", "1ero", etc.)
- seccion — texto/varchar (ej.: "A", "B")
- fecha_ingreso — date
- anio_escolar — texto/varchar (ej.: "2025-2026")
- activo — boolean

Notas:

- Una fila por ciclo escolar. `activo=true` indica la inscripción vigente.

### 3) alumno_medico

- alumno_medico_id (PK)
- alumno_id (FK -> alumnos.id) [único]
- problemas_audicion — texto/varchar
- problemas_lenguaje — texto/varchar
- grupo_sanguineo — texto/varchar (ej.: "O+", "A-")
- alimentacion — texto (notas/restricciones)

Notas:

- Campos multivaluados (alergias, enfermedades y medicamentos) se modelan aparte.

### 4) alergias (catálogo)

- alergia_id (PK)
- nombre — texto único

### 5) alumno_alergia (N:M)

- alumno_id (FK -> alumnos.id)
- alergia_id (FK -> alergias.id)
- (PK compuesta: alumno_id, alergia_id)

### 6) enfermedades (catálogo)

- enfermedad_id (PK)
- nombre — texto único

### 7) alumno_enfermedad (N:M)

- alumno_id (FK -> alumnos.id)
- enfermedad_id (FK -> enfermedades.id)
- (PK compuesta: alumno_id, enfermedad_id)

### 8) medicamentos (catálogo)

- medicamento_id (PK)
- nombre — texto único

### 9) alumno_medicamento_autorizado (N:M)

- alumno_id (FK -> alumnos.id)
- medicamento_id (FK -> medicamentos.id)
- (PK compuesta: alumno_id, medicamento_id)

### 10) familiares

Representa PADRE, MADRE (y opcionalmente otros familiares con ese rol). Se separa de representantes autorizados para diferenciar responsabilidades.

- padres_id (PK)
- alumno_id (FK -> alumnos.id)
- rol — enum('PADRE','MADRE')
- full_name — texto
- vive_con_alumno — boolean
- fecha_nacimiento — date
- lugar_nacimiento — texto
- cedula — texto/varchar
- estado_civil — enum('SOLTERO','CASADO','DIVORCIADO','VIUDO','UNIÓN_ESTABLE')
- informacion_laboral — texto
- telefono_contacto — texto/varchar

### 11) datos_embarazo

- embarazo_id (PK)
- alumno_id (FK -> alumnos.id) [único]
- complicaciones — texto
- tipo_parto — enum('NATURAL','CESAREA')
- fumo - boolean
- bebio — boolean
- peso_al_nacer_kg — numeric(5,2)
- demas_informacion — texto

### 12) representantes_autorizados

Permite múltiples personas autorizadas para cada alumno.

- representante_id (PK)
- alumno_id (FK -> alumnos.id)
- full_name — texto
- edad — integer
- cedula — texto/varchar
- parentesco — texto/varchar (o enum, p. ej.: 'TIO','ABUELO','AMIGO_FAMILIA')
- telefono_contacto — texto/varchar

## Relaciones (resumen)

- alumnos (1) — (N) inscripciones
- alumnos (1) — (1) alumno_medico
- alumnos (1) — (N) alumno_alergia — (N) alergias
- alumnos (1) — (N) alumno_enfermedad — (N) enfermedades
- alumnos (1) — (N) alumno_medicamento_autorizado — (N) medicamentos
- alumnos (1) — (N) familiares (rol PADRE/MADRE)
- alumnos (1) — (1) datos_embarazo
- alumnos (1) — (N) representantes_autorizados

## Índices recomendados

- `alumnos(full_name)` — para acelerar búsqueda por nombre/prefijo.
- `inscripciones(alumno_id, activo)` — para ubicar inscripción vigente.
- `familiares(alumno_id, rol)` — para recuperar padre/madre rápidamente.
- `representantes_autorizados(alumno_id)`.
- En tablas N:M, índices por ambas columnas.

## Notas de normalización (4NF)

- Alergias, enfermedades y medicamentos autorizados son conjuntos multivaluados; se modelan como N:M para eliminar redundancia (4NF).
- Inscripciones separadas evitan duplicar grado/sección/año en `alumnos` (3NF/BCNF) y permiten histórico.
- `familiares` unifica PADRE/MADRE bajo un rol para evitar duplicación de estructura; mantiene atributos atómicos (1NF).
- Atributos opcionales de catálogos pueden crecer sin alterar `alumnos`.

## Posibles ajustes futuros

- Convertir `parentesco` de `representantes_autorizados` a enum si se requiere consistencia.
- Añadir `edad` derivada (vista o columna calculada) para alumnos.
- Indexar por trigramas (pg_trgm) si la búsqueda por fragmentos de nombre lo requiere (fuera del alcance actual).
