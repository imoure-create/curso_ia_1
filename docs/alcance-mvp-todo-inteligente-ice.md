# Alcance funcional MVP - Gestor de Tareas Inteligente con ICE

## 1. Objetivo del MVP

Construir una aplicacion web sencilla en React que permita gestionar tareas tipo ToDo y priorizarlas mediante el modelo ICE. El usuario podra crear tareas, describirlas y solicitar a una API de IA gratuita una estimacion de Impacto, Confianza y Facilidad a partir de la descripcion.

El MVP esta pensado para un curso de React, por lo que debe priorizar claridad, componentes simples, estado local y una experiencia facil de entender.

## 2. Contexto del producto

El Gestor de Tareas Inteligente ayuda a una persona a decidir que tareas abordar primero. En lugar de ordenar solo por fecha o por estado, cada tarea tendra una puntuacion ICE:

- Impacto: valor esperado de completar la tarea.
- Confianza: seguridad de que la estimacion es correcta o de que la tarea aportara valor.
- Facilidad: sencillez para completar la tarea.

La puntuacion ICE permitira ordenar las tareas por prioridad.

## 3. Alcance incluido

### 3.1 Gestion basica de tareas

El usuario podra:

- Crear una tarea con titulo y descripcion.
- Ver un listado de tareas.
- Editar una tarea existente.
- Eliminar una tarea.
- Marcar una tarea como completada o pendiente.
- Ver la puntuacion ICE de cada tarea.
- Ordenar las tareas por puntuacion ICE de mayor a menor.

### 3.2 Campos de una tarea

Cada tarea tendra los siguientes datos minimos:

| Campo | Tipo | Descripcion |
| --- | --- | --- |
| id | string | Identificador generado en frontend. |
| titulo | string | Nombre corto de la tarea. |
| descripcion | string | Texto usado para explicar la tarea y calcular el ICE. |
| impacto | number | Valor de 1 a 10. |
| confianza | number | Valor de 1 a 10. |
| facilidad | number | Valor de 1 a 10. |
| iceScore | number | Resultado del calculo ICE. |
| estado | string | pendiente o completada. |
| explicacionIA | string | Breve justificacion generada por la IA. |
| fechaCreacion | string | Fecha generada en frontend. |

### 3.3 Calculo ICE

El modelo ICE se calculara con la siguiente formula:

```text
ICE = Impacto x Confianza x Facilidad
```

Cada valor ira de 1 a 10, por lo que la puntuacion final estara entre 1 y 1000.

Reglas:

- Si falta algun valor, la puntuacion ICE no se muestra o aparece como pendiente.
- Si la IA devuelve valores fuera del rango 1-10, la aplicacion debe ajustarlos al rango valido.
- El usuario puede modificar manualmente los valores sugeridos por la IA.
- Al cambiar impacto, confianza o facilidad, el ICE se recalcula automaticamente.

### 3.4 Estimacion ICE con IA

El usuario podra escribir una descripcion y pulsar un boton como "Calcular ICE con IA".

La aplicacion enviara la descripcion a una API de IA gratuita desde el frontend usando `fetch`. La API debera devolver o permitir extraer:

- Impacto, numero de 1 a 10.
- Confianza, numero de 1 a 10.
- Facilidad, numero de 1 a 10.
- Breve explicacion de la estimacion.

Para mantener el proyecto simple:

- La clave de API se configurara en una variable de entorno de Vite, por ejemplo `VITE_AI_API_KEY`.
- La llamada se hara desde React, sin servidor intermedio.
- Este enfoque se considera valido solo para practica academica, ya que una clave usada en frontend puede quedar expuesta.
- El proveedor concreto de IA debe ser configurable, siempre que tenga una modalidad gratuita apta para el curso.

### 3.5 Validacion y estados de interfaz

La interfaz debera contemplar:

- Formulario con titulo obligatorio.
- Descripcion recomendada para poder calcular ICE con IA.
- Estado de carga mientras se espera la respuesta de la IA.
- Mensaje de error si falla la llamada a la API.
- Mensaje de error si la respuesta de la IA no se puede interpretar.
- Boton deshabilitado mientras se esta calculando el ICE.

### 3.6 Ordenacion y filtros simples

El MVP incluira:

- Ordenar por ICE descendente.
- Alternar vista entre todas, pendientes y completadas.

No se incluye paginacion ni filtros avanzados.

## 4. Alcance excluido

El MVP no incluye:

- Backend.
- Autenticacion.
- Persistencia real.
- Paginacion.
- Multiusuario.
- Tags o etiquetas.
- Roles o permisos.
- Sincronizacion entre dispositivos.
- Adjuntos o imagenes.
- Notificaciones.
- Fechas limite avanzadas.
- Busqueda avanzada.
- Analiticas.

## 5. Persistencia y datos

La aplicacion funcionara con estado local en memoria mediante React.

Comportamiento esperado:

- Las tareas se mantienen mientras la pagina este abierta.
- Al recargar el navegador, los datos pueden perderse.
- Se pueden incluir tareas de ejemplo iniciales para facilitar la demostracion.

No se usara base de datos ni API propia.

## 6. Vistas principales

### 6.1 Vista principal

Contendra:

- Encabezado sencillo con el nombre de la aplicacion.
- Formulario para crear o editar tareas.
- Boton para calcular ICE con IA.
- Listado de tareas.
- Control simple para ordenar o filtrar.

### 6.2 Tarjeta o fila de tarea

Cada tarea mostrara:

- Titulo.
- Descripcion resumida.
- Estado.
- Valores de impacto, confianza y facilidad.
- Puntuacion ICE destacada.
- Explicacion generada por IA, si existe.
- Acciones: editar, eliminar, completar o reabrir.

## 7. Flujo principal de uso

1. El usuario abre la aplicacion.
2. Escribe el titulo de una tarea.
3. Escribe una descripcion.
4. Pulsa "Calcular ICE con IA".
5. La aplicacion muestra un estado de carga.
6. La IA devuelve impacto, confianza, facilidad y explicacion.
7. El usuario revisa o ajusta los valores.
8. El usuario guarda la tarea.
9. La tarea aparece en el listado ordenable por ICE.
10. El usuario puede completarla, editarla o eliminarla.

## 8. Requisitos funcionales

| ID | Requisito |
| --- | --- |
| RF-01 | El usuario puede crear tareas con titulo y descripcion. |
| RF-02 | El usuario puede editar tareas existentes. |
| RF-03 | El usuario puede eliminar tareas. |
| RF-04 | El usuario puede marcar tareas como pendientes o completadas. |
| RF-05 | El usuario puede solicitar una estimacion ICE usando IA. |
| RF-06 | La aplicacion calcula ICE con la formula Impacto x Confianza x Facilidad. |
| RF-07 | El usuario puede modificar manualmente impacto, confianza y facilidad. |
| RF-08 | El listado puede ordenarse por ICE descendente. |
| RF-09 | El usuario puede filtrar entre todas, pendientes y completadas. |
| RF-10 | La aplicacion muestra errores cuando falla la estimacion con IA. |

## 9. Requisitos no funcionales

| ID | Requisito |
| --- | --- |
| RNF-01 | La aplicacion debe estar construida con React. |
| RNF-02 | No debe usar backend propio. |
| RNF-03 | Debe ser facil de entender para estudiantes de React. |
| RNF-04 | Debe usar componentes pequenos y responsabilidades claras. |
| RNF-05 | Debe funcionar como Single Page Application. |
| RNF-06 | Debe evitar dependencias innecesarias. |
| RNF-07 | Debe manejar errores de la API sin bloquear la aplicacion. |

## 10. Prompt sugerido para la IA

```text
Analiza la siguiente tarea y estima su prioridad usando el modelo ICE.

Devuelve exclusivamente un JSON valido con esta estructura:
{
  "impacto": 1,
  "confianza": 1,
  "facilidad": 1,
  "explicacion": "Texto breve"
}

Reglas:
- impacto, confianza y facilidad deben ser numeros enteros entre 1 y 10.
- La explicacion debe ser breve y clara.
- No incluyas texto fuera del JSON.

Tarea:
{{descripcion}}
```

## 11. Criterios de aceptacion

El MVP se considera completado cuando:

- Se puede crear una tarea desde la interfaz.
- Se puede calcular una puntuacion ICE usando una descripcion y una API de IA gratuita.
- Se puede revisar y editar manualmente la puntuacion sugerida.
- Se puede listar, completar, editar y eliminar tareas.
- Se puede ordenar el listado por ICE.
- No existe backend, autenticacion ni persistencia real.
- El codigo es comprensible para un curso basico o intermedio de React.

## 12. Riesgos y consideraciones

- Algunas APIs gratuitas pueden tener limites de uso, cambios de disponibilidad o restricciones de CORS.
- Al no existir backend, la clave de API puede quedar visible en el navegador.
- La IA puede devolver respuestas no validas, por lo que el frontend debe validar y mostrar errores.
- La estimacion ICE debe entenderse como una ayuda, no como una decision automatica definitiva.

## 13. Entregable esperado

Una aplicacion React sencilla que funcione en local y permita demostrar:

- Gestion basica de tareas.
- Priorizacion con ICE.
- Integracion directa con una API de IA gratuita.
- Manejo de estados de carga y error.
- Codigo simple, didactico y facil de explicar en clase.
