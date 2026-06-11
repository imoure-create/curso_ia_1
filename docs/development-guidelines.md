# Guia tecnica de desarrollo

## 1. Estructura de carpetas

| Ruta | Uso |
| --- | --- |
| `src/app/` | Configuracion global de la app. |
| `src/pages/` | Vistas principales y composicion de pantalla. |
| `src/components/` | Componentes reutilizables de UI. |
| `src/features/` | Funcionalidad agrupada por dominio. |
| `src/hooks/` | Hooks reutilizables no ligados a una feature. |
| `src/services/` | Llamadas externas y clientes API. |
| `src/types/` | Tipos compartidos entre modulos. |
| `src/utils/` | Funciones puras y helpers pequenos. |
| `src/constants/` | Constantes de aplicacion. |
| `src/theme/` | Tema y ajustes de Material UI. |

- [ ] Mantener `App.tsx` como composicion principal, no como contenedor de logica pesada.
- [ ] Crear carpetas nuevas solo cuando haya al menos dos archivos relacionados.
- [ ] Agrupar por feature cuando la logica sea especifica del dominio.
- [ ] Usar carpetas globales solo para codigo realmente compartido.
- [ ] Evitar capas genericas como `core`, `shared` o `common` si no aportan claridad.
- [ ] Mantener los archivos cerca del lugar donde se usan.
- [ ] No duplicar tipos entre `features` y `types`.
- [ ] No crear barrels (`index.ts`) salvo que simplifiquen imports reales.

## 2. Convenciones de nombres

| Elemento | Convencion | Ejemplo |
| --- | --- | --- |
| Componentes | PascalCase | `TaskForm.tsx` |
| Hooks | camelCase con `use` | `useTasks.ts` |
| Tipos e interfaces | PascalCase | `Task`, `IceScore` |
| Funciones | camelCase | `calculateIceScore` |
| Constantes | UPPER_SNAKE_CASE | `MAX_ICE_VALUE` |
| Archivos de componentes | PascalCase | `TaskItem.tsx` |
| Archivos utilitarios | camelCase | `iceUtils.ts` |
| Variables booleanas | prefijo claro | `isLoading`, `hasError` |

- [ ] Usar nombres orientados al dominio, no a la implementacion.
- [ ] Evitar abreviaturas salvo las conocidas por el proyecto.
- [ ] Nombrar eventos con intencion: `onSave`, `onDelete`, `onToggleStatus`.
- [ ] Nombrar handlers internos con `handle`: `handleSubmit`.
- [ ] Usar `Props` solo junto al componente: `TaskFormProps`.
- [ ] Evitar nombres genericos como `data`, `item`, `manager` o `helper`.
- [ ] Mantener el idioma del codigo en ingles.
- [ ] Mantener los textos visibles al usuario en espanol.

## 3. Organizacion de componentes

| Tipo | Responsabilidad |
| --- | --- |
| Page | Orquesta vista, datos y acciones principales. |
| Feature component | Implementa una parte del dominio. |
| UI component | Renderiza interfaz reutilizable sin reglas de negocio. |
| Form component | Gestiona entrada, validacion local y envio. |
| List component | Renderiza colecciones y estados vacios. |
| Item component | Renderiza una entidad y sus acciones directas. |
| Dialog component | Encapsula confirmaciones o edicion modal. |

- [ ] Un componente debe tener una responsabilidad principal.
- [ ] Mantener componentes pequenos y faciles de leer.
- [ ] Subir estado solo cuando dos componentes lo necesiten.
- [ ] Pasar callbacks explicitos en lugar de objetos de acciones genericos.
- [ ] Evitar componentes que mezclen formulario, listado y llamadas API.
- [ ] Usar Material UI como base visual antes de crear UI propia.
- [ ] No envolver componentes MUI sin una necesidad repetida.
- [ ] Mantener estilos locales con `sx` cuando sean simples.
- [ ] Mover estilos repetidos al tema o a componentes reutilizables.
- [ ] Evitar componentes puramente decorativos sin valor funcional.

## 4. Uso de hooks

| Hook | Uso recomendado |
| --- | --- |
| `useState` | Estado local simple. |
| `useEffect` | Sincronizacion con efectos externos. |
| `useMemo` | Calculos derivados costosos o listas filtradas. |
| `useCallback` | Callbacks estables solo si aporta valor. |
| Hook custom | Reutilizar logica de estado o efectos. |

- [ ] Preferir estado derivado antes que duplicar datos.
- [ ] No usar `useEffect` para calculos que pueden hacerse durante render.
- [ ] Mantener cada efecto con una sola razon de existir.
- [ ] Limpiar efectos con timers, listeners o peticiones cancelables.
- [ ] Crear hooks custom solo si reducen duplicacion real.
- [ ] No ocultar reglas de negocio importantes en hooks genericos.
- [ ] Devolver nombres claros desde hooks custom.
- [ ] Evitar optimizaciones prematuras con `useMemo` y `useCallback`.
- [ ] Mantener dependencias de efectos completas.
- [ ] No llamar hooks de forma condicional.

## 5. Gestion del estado

| Estado | Ubicacion |
| --- | --- |
| Campo de formulario | Componente de formulario. |
| Filtro u ordenacion | Page o hook de feature. |
| Lista de tareas | Page o hook de feature. |
| Carga de API | Servicio consumidor o hook de feature. |
| Error de API | Cerca de la accion que lo muestra. |
| Tema | Configuracion MUI. |

- [ ] Usar estado local de React por defecto.
- [ ] No anadir libreria global de estado para el MVP.
- [ ] Separar estado editable de datos ya guardados.
- [ ] Calcular valores derivados en render o con `useMemo`.
- [ ] Mantener actualizaciones inmutables.
- [ ] Centralizar reglas de dominio en funciones puras.
- [ ] Evitar sincronizar el mismo dato en varios estados.
- [ ] Reiniciar errores cuando el usuario reintenta la accion.
- [ ] Usar `useReducer` solo si reduce complejidad.
- [ ] No persistir datos salvo requisito explicito.

## 6. Gestion de llamadas API

| Pieza | Norma |
| --- | --- |
| Cliente API | Vivir en `src/services/`. |
| Variables de entorno | Usar prefijo `VITE_`. |
| Respuesta externa | Validar antes de usar. |
| Errores | Convertir a mensajes controlados. |
| Timeouts | Aplicar si el proveedor puede bloquear la UX. |
| Secretos | No asumir privacidad en frontend. |

- [ ] Encapsular `fetch` fuera de los componentes.
- [ ] Exponer funciones de servicio con nombres de dominio.
- [ ] No acoplar componentes al formato bruto del proveedor.
- [ ] Normalizar respuestas externas a tipos internos.
- [ ] Validar rangos y campos obligatorios.
- [ ] Tratar respuestas no parseables como error controlado.
- [ ] Evitar clientes HTTP adicionales si `fetch` es suficiente.
- [ ] Mantener proveedor de IA intercambiable desde el servicio.
- [ ] No repetir URLs ni claves en componentes.
- [ ] Documentar solo variables necesarias en `.env.example` si existe.

## 7. Manejo de errores y cargas

| Situacion | Comportamiento |
| --- | --- |
| Carga inicial | Mostrar indicador discreto. |
| Accion en curso | Deshabilitar accion afectada. |
| Error recuperable | Mostrar mensaje y permitir reintento. |
| Respuesta invalida | Explicar que no se pudo interpretar. |
| Lista vacia | Mostrar estado vacio accionable. |
| Validacion | Mostrar error junto al campo. |

- [ ] No bloquear toda la pantalla por una accion local.
- [ ] Usar `Alert`, `Snackbar` o `FormHelperText` de MUI segun contexto.
- [ ] Mantener mensajes breves y comprensibles.
- [ ] No mostrar detalles tecnicos crudos al usuario.
- [ ] Registrar en consola solo durante desarrollo si ayuda a depurar.
- [ ] Deshabilitar botones mientras se ejecuta la misma accion.
- [ ] Mantener datos existentes visibles si falla una actualizacion.
- [ ] Limpiar estados de carga en `finally` o flujo equivalente.
- [ ] Dar feedback visual a toda accion asincrona.
- [ ] No silenciar errores.

## 8. Librerias aprobadas

| Libreria | Estado | Uso |
| --- | --- | --- |
| `react` | Aprobada | UI y estado local. |
| `react-dom` | Aprobada | Render de la app. |
| `typescript` | Aprobada | Tipado estatico. |
| `@mui/material` | Aprobada | Componentes Material UI. |
| `@mui/icons-material` | Aprobada | Iconos Material. |
| `@emotion/react` | Aprobada | Requisito de MUI. |
| `@emotion/styled` | Aprobada | Requisito de MUI. |
| `vite` | Aprobada | Desarrollo y build. |

- [ ] No instalar librerias sin necesidad repetida y clara.
- [ ] Preferir APIs nativas del navegador cuando basten.
- [ ] Evitar librerias de estado global en proyectos pequenos.
- [ ] Evitar librerias de formularios si el formulario es simple.
- [ ] Evitar librerias de fechas salvo reglas complejas.
- [ ] Evitar utilidades grandes para una sola funcion.
- [ ] Revisar peso, mantenimiento y tipos antes de aprobar una libreria.
- [ ] Mantener dependencias alineadas con React, TypeScript y MUI.
- [ ] Documentar cualquier nueva libreria aprobada en esta tabla.
- [ ] Eliminar dependencias que dejen de usarse.
