# Sistema de Gestión Digital — Preescolar Patriotas de Venezuela

> "ELABORACIÓN DE UN SISTEMA DE GESTIÓN DIGITAL PARA EL REGISTRO DE DATOS PERSONALES, MÉDICOS Y DE DESARROLLO INFANTIL DE LOS NIÑOS DEL PREESCOLAR PATRIOTAS DE VENEZUELA"

Este documento describe el contexto y la motivación del proyecto de servicio comunitario para el desarrollo de un sistema de gestión digital orientado al registro de datos estudiantiles.

## Tabla de contenido

- [Introducción](#introducción)
- [Justificación](#justificación)
- [Alcance y funcionalidades](#alcance-y-funcionalidades)
- [Secciones de datos (plegables)](#secciones-de-datos-plegables)
- [Módulos y campos (detalle)](#módulos-y-campos-detalle)
- [Resumen de UI (desde el PDF)](#resumen-de-ui-desde-el-pdf)

## Introducción

En vista de las dificultades que enfrentan múltiples instituciones educativas en el manejo de la información de sus estudiantes, donde poseen información incompleta, o directamente no poseen registros que contengan datos de estos, el preescolar Patriotas de Venezuela ha identificado la necesidad de contar con un sistema organizado para el registro de los datos personales, médicos y de desarrollo infantil de los niños que asisten a la institución. Actualmente, los únicos datos que poseen de los alumnos que se encuentran en sus aulas, se encuentran en formato físico, debido que son proporcionados por los padres mediante un formulario con preguntas sobre información genérica del niño, como pueden ser: datos morfológicos, información médica relevante como por ejemplo saber si este tiene alergias, enfermedades o condiciones especiales, entre otros.

Esta información que maneja la guardería se gestiona de manera manual, lo que dificulta una revisión del expediente de un alumno, además del peligro siempre presente sobre un posible extravío o daño, bien sea parcial o completo sobre este expediente, ocasionando inconvenientes en la localización de datos relevantes, duplicidad de registros e incluso pérdida de información importante para el seguimiento del bienestar de los estudiantes, además de reducir la calidad de atención de parte del docente al alumnado por la falta de dicha información. Esta carencia genera incertidumbre, complica la toma de decisiones informadas y entorpece la labor del profesorado.

Ante esta realidad, se hace evidente la necesidad de desarrollar un proyecto que brinde soluciones efectivas. Un sistema enfocado en una aplicación de escritorio, que sea intuitiva y accesible, diseñada específicamente para la institución educativa, puede marcar una diferencia notable en la mejoría de sus procesos actuales. De esta manera, se busca contribuir al mejoramiento de la atención integral de los estudiantes y al funcionamiento general de la institución. Este proyecto beneficiaría directamente a la institución educativa. A su vez le beneficiaría al desarrollador del proyecto debido a que le presenta una oportunidad para aplicar los conocimientos aprendidos durante el estudio de la carrera de ingeniería en sistemas en la Universidad Santa María.

## Justificación

La digitalización de los registros estudiantiles ofrece múltiples beneficios a las instituciones educativas, especialmente en términos de organización, seguridad y calidad en la atención pedagógica. Uno de los aportes más relevantes es la posibilidad de contar con información completa y accesible sobre cada niño, lo cual permite a los docentes tomar decisiones más acertadas y brindar un acompañamiento educativo y formativo más personalizado, dedicado a las necesidades particulares de cada niño.

Asimismo, la gestión digital de los expedientes reduce la dependencia del formato físico, en donde siempre se encuentra presente la vulnerabilidad física inherente a la documentación en papel, evitando pérdidas de información y facilitando un acceso rápido a datos relevantes como antecedentes médicos, condiciones especiales o aspectos del desarrollo infantil. Esto contribuye a mejorar la calidad del cuidado y de la enseñanza, ya que el personal educativo dispone de información confiable y actualizada en todo momento.

La creación de un sistema de gestión digital que contenga la información de cada expediente estudiantil representa un avance en la modernización administrativa de la guardería, al optimizar los procesos de registro y consulta de información. Además de beneficiar directamente a la institución y al personal docente, la investigación resulta valiosa para el desarrollador del proyecto debido a que le presenta una oportunidad para aplicar los conocimientos aprendidos durante el estudio de la carrera de ingeniería en sistemas en la Universidad Santa María.

## Alcance y funcionalidades

- Listado de alumnos con:
  - Filtro por grado/sección.
  - Barra de búsqueda por nombre con filtrado en vivo (tras cada carácter). Los alumnos que coinciden permanecen en pantalla; el resto se ocultan.
- Botón para agregar alumno (desde la vista principal).
- Por cada alumno en la lista: botones de Editar y Eliminar.
- Vista de detalle del alumno con edición por secciones (plegables).
- Login simple con un único usuario para la directiva.

## Secciones de datos (plegables)

En la vista de detalle del alumno, la información se organiza en listas plegables para facilitar su llenado y consulta. Secciones previstas:

- Datos personales del alumno.
- Datos médicos del alumno.
- Datos del padre.
- Datos de la madre.
- Datos del embarazo.
- Datos del representante autorizado.

Los campos exactos de cada sección se ajustarán conforme al PDF que se incluirá en el model context del proyecto.

## Resumen de UI (desde el PDF)

- Página inicial:
  - Barra de búsqueda centrada en nombre del alumno con filtrado en vivo.
  - Botón “Agregar alumno” junto a la barra.
  - Listado de alumnos mostrando campos principales para consulta rápida, por ejemplo: Nombre y Apellido, Grado/Sección, Fecha de nacimiento (o Edad).
  - Por cada alumno: botones “Editar” y “Eliminar”.
- Página de detalle de alumno:
  - Secciones plegables por categoría (ver lista anterior) para facilitar el llenado y la lectura.
  - Los campos internos exactos se tomarán del PDF; si es necesario, se añadirá el desglose en este documento.

## Módulos y campos (detalle)

La lista completa y actualizada de campos por módulo está en `context/campos.md`.
