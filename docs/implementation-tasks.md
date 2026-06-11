# Tareas de implementacion

## 1. Preparar base del proyecto

**Objetivo**

Crear la base React + TypeScript + Material UI para una SPA simple y coherente con la guia tecnica.

**Alcance incluido**

- Configurar Vite, React y TypeScript.
- Instalar Material UI y dependencias aprobadas.
- Crear estructura inicial de carpetas.
- Definir tema base de Material UI.
- Crear `App`, `MainLayout` y `TasksPage` vacios.
- Mostrar encabezado inicial con el nombre de la aplicacion.

**Archivos o carpetas previsibles a modificar**

- `package.json`
- `index.html`
- `src/main.tsx`
- `src/App.tsx`
- `src/app/`
- `src/pages/`
- `src/theme/`

**Criterios de aceptacion**

- La aplicacion arranca en local sin errores.
- La pantalla principal muestra el layout base.
- Material UI esta integrado mediante `ThemeProvider`.
- La estructura respeta `docs/development-guidelines.md`.
- No hay funcionalidades de negocio implementadas todavia.

**Dependencias**

- Ninguna.

## 2. Definir modelo, utilidades ICE y estado local

**Objetivo**

Crear el modelo de tareas y la logica base para gestionar tareas en memoria.

**Alcance incluido**

- Definir tipos `Task`, `TaskStatus` e `IceValues`.
- Crear utilidades para calcular `iceScore`.
- Validar y ajustar valores ICE al rango 1-10.
- Crear hook de feature para tareas.
- Incluir tareas de ejemplo opcionales para demostracion.
- Preparar acciones de crear, editar, eliminar y cambiar estado.

**Archivos o carpetas previsibles a modificar**

- `src/types/`
- `src/utils/iceUtils.ts`
- `src/features/tasks/`
- `src/features/tasks/hooks/useTasks.ts`
- `src/features/tasks/constants/`

**Criterios de aceptacion**

- El estado de tareas vive en React, sin backend ni persistencia real.
- El calculo ICE usa `Impacto x Confianza x Facilidad`.
- Los valores fuera de rango se normalizan.
- Las acciones principales pueden probarse desde el hook o componentes temporales.
- No se introduce libreria global de estado.

**Dependencias**

- Tarea 1.

## 3. Implementar listado, filtros y acciones basicas

**Objetivo**

Construir la vista principal con listado de tareas, filtros simples y acciones directas.

**Alcance incluido**

- Crear `TaskListSection`, `TaskList` y `TaskItem`.
- Crear `TaskToolbar` con filtro por estado.
- Permitir ordenar por ICE descendente.
- Mostrar estado vacio cuando no haya tareas.
- Mostrar titulo, descripcion resumida, estado, valores ICE e `iceScore`.
- Permitir eliminar, completar y reabrir tareas.

**Archivos o carpetas previsibles a modificar**

- `src/pages/TasksPage.tsx`
- `src/features/tasks/components/TaskToolbar.tsx`
- `src/features/tasks/components/TaskListSection.tsx`
- `src/features/tasks/components/TaskList.tsx`
- `src/features/tasks/components/TaskItem.tsx`
- `src/features/tasks/components/IceScoreSummary.tsx`

**Criterios de aceptacion**

- El usuario ve el listado de tareas en la pantalla principal.
- El usuario puede filtrar entre todas, pendientes y completadas.
- El usuario puede ordenar por ICE descendente.
- El usuario puede completar, reabrir y eliminar una tarea.
- La UI usa componentes Material UI.

**Dependencias**

- Tarea 2.

## 4. Implementar formulario de creacion y edicion

**Objetivo**

Permitir crear y editar tareas con validacion local y edicion manual de valores ICE.

**Alcance incluido**

- Crear `TaskFormDialog`.
- Crear `TaskForm` con titulo y descripcion.
- Validar titulo obligatorio.
- Crear `IceManualEditor` para impacto, confianza y facilidad.
- Recalcular ICE automaticamente al cambiar valores.
- Guardar tareas nuevas en estado pendiente.
- Reutilizar el formulario para editar tareas existentes.
- Mostrar confirmacion breve al guardar.

**Archivos o carpetas previsibles a modificar**

- `src/features/tasks/components/TaskFormDialog.tsx`
- `src/features/tasks/components/TaskForm.tsx`
- `src/features/tasks/components/TaskBasicFields.tsx`
- `src/features/tasks/components/IceManualEditor.tsx`
- `src/features/tasks/components/TaskFormActions.tsx`
- `src/pages/TasksPage.tsx`

**Criterios de aceptacion**

- El usuario puede abrir el formulario desde la pantalla principal.
- El usuario puede crear una tarea con titulo y descripcion.
- El usuario puede editar una tarea existente.
- El usuario puede ajustar manualmente impacto, confianza y facilidad.
- El ICE se recalcula antes de guardar.
- Los errores de validacion se muestran junto al campo afectado.

**Dependencias**

- Tarea 3.

## 5. Integrar sugerencia ICE por IA y cerrar flujo principal

**Objetivo**

Completar el flujo principal: solicitar sugerencia ICE por IA, revisarla, ajustarla y confirmar la tarea.

**Alcance incluido**

- Crear `aiIceService` usando `fetch`.
- Leer configuracion desde variables `VITE_`.
- Enviar descripcion de tarea al proveedor configurado.
- Validar respuesta de IA antes de aplicarla.
- Crear `IceSuggestionPanel` con carga, error y resultado.
- Permitir aceptar sugerencia o editar valores manualmente.
- Mostrar mensaje de error si falla la llamada o la respuesta no es valida.
- Dejar la tarea creada visible en el listado tras confirmar.

**Archivos o carpetas previsibles a modificar**

- `src/services/aiIceService.ts`
- `src/features/tasks/components/IceSuggestionPanel.tsx`
- `src/features/tasks/components/TaskForm.tsx`
- `src/features/tasks/components/IceManualEditor.tsx`
- `src/utils/iceUtils.ts`
- `.env.example`

**Criterios de aceptacion**

- El usuario puede solicitar una estimacion ICE desde el formulario.
- La UI muestra estado de carga mientras espera la IA.
- El boton de calculo se deshabilita durante la llamada.
- La respuesta valida rellena impacto, confianza, facilidad y explicacion.
- El usuario puede aceptar o modificar manualmente la sugerencia.
- Los errores se muestran sin bloquear la aplicacion.
- La tarea confirmada aparece en el listado con su puntuacion ICE.

**Dependencias**

- Tarea 4.
