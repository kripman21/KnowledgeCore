# 🎓 KnowledgeCore — Sistema Integral de Gestión Académica & Bóveda de Conocimiento

**KnowledgeCore** es un sistema web integral diseñado para la gestión de trayectoria universitaria, programas de certificación técnica y talleres de alta especialización. Combina un diseño de alta densidad de información con una arquitectura en capas desacopladas, cero datos falsos, persistencia relacional local y control amigable para agentes de inteligencia artificial.

---

## 📑 Tabla de Contenidos
1. [Características Principales](#-características-principales)
2. [Arquitectura del Sistema & Stack Tecnológico](#-arquitectura-del-sistema--stack-tecnológico)
3. [Estructura del Proyecto (Monorepo)](#-estructura-del-proyecto-monorepo)
4. [Flujo de Funcionamiento del Sistema](#-flujo-de-funcionamiento-del-sistema)
   - [4.1 Hub Central Multifuente & Estadísticas](#41-hub-central-multifuente--estadísticas)
   - [4.2 Dashboard de la Carrera Universitaria](#42-dashboard-de-la-carrera-universitaria)
   - [4.3 Dashboard Modular de la Materia / Curso / Taller](#43-dashboard-modular-de-la-materia--curso--taller)
   - [4.4 Drawer Lateral de Recursos & Clases](#44-drawer-lateral-de-recursos--clases)
   - [4.5 Modo Zen de Notas & Flashcards Active Recall](#45-modo-zen-de-notas--flashcards-active-recall)
   - [4.6 Espacio de Estudio y Mazos de Flashcards (Active Recall Leitner)](#46-espacio-de-estudio-y-mazos-de-flashcards-active-recall-leitner)
   - [4.7 Calendario Unificado de Entregables (Fiel a Stitch 45de29e2)](#47-calendario-unificado-de-entregables-fiel-a-stitch-45de29e2)
   - [4.8 Calendario Mensual en el Dashboard de la Carrera & Hub](#48-calendario-mensual-en-el-dashboard-de-la-carrera--hub)
   - [4.9 Administración Integral (CRUD Modular)](#49-administración-integral-crud-modular)
5. [Modelo de Datos & Esquema Relacional](#-modelo-de-datos--esquema-relacional)
6. [API RESTful & Especificación OpenAPI](#-api-restful--especificación-openapi)
7. [Guía de Instalación y Puesta en Marcha](#-guía-de-instalación-y-puesta-en-marcha)
8. [Verificación de Calidad y Suites de Prueba](#-verificación-de-calidad-y-suites-de-prueba)

---

## 🌟 Características Principales

- **Gestión Multifuente Unificada:** Centraliza carreras universitarias formales (con créditos UC y semestres), cursos técnicos (con horas estimadas y certificados) y talleres prácticos intensivos en un solo panel de control.
- **Malla Curricular Oficial Completa:** Carga e integra las **39 materias de la carrera de Ingeniería en Desarrollo de Software (ING-DS)**, distribuidas a lo largo de sus 10 cuatrimestres con sus respectivos códigos institucionales, profesores y cargas de créditos.
- **Visualización Dual (Tabla Notion-Style y Galería de Tarjetas):** Permite explorar asignaturas en modo tabla estructurada o en modo galería visual con avance de clases, estados y calificaciones.
- **Calendario Mensual Interactivo & Entregas Críticas:** Cuadrícula de 7 columnas que consolida horas de estudio reales por día y entregables clasificados cromáticamente (Proyectos en `#70dba4`, Exámenes en `#ffb4ab`, Tareas en `#87ceff`).
- **Modo Zen de Notas de Estudio:** Entorno minimalista y libre de distracciones con barra de herramientas Markdown, conteo de palabras en tiempo real, tiempo de lectura y guardado asíncrono.
- **Sistema de Flashcards (Active Recall) & Espacio de Estudio:** Sección de navegación dedicada (`Estudio (Flashcards)`) con mazos jerarquizados por Asignaturas universitarias y mazos directos independientes para Cursos y Talleres. Modal interactivo de tarjetas giratorias (*Flip Card*) con sistema Leitner de 3 niveles de maestría y persistencia en SQLite.
- **Gestor de Recursos Didácticos:** Organización y descarga directa de documentos PDF, paquetes ZIP con código fuente y enlaces web vinculados a cada clase, con carga independiente y sin datos falsos.
- **Administración Integral sin Stubs (CRUD Completo):** Creación, edición y eliminación en caliente de fuentes, materias, clases, recursos didácticos y entregables con validación de reglas de negocio y borrado en cascada.

---

## 🏛️ Arquitectura del Sistema & Stack Tecnológico

KnowledgeCore se rige bajo el principio innegociable de separación estricta de capas:

```
[ FRONTEND: React 19 + Vite + Tailwind CSS ]
                     │ (Fetch / JSON)
                     ▼
[ API RESTful: Fastify + Swagger OpenAPI 3.0 ]
                     │ (Inyección de Dependencias)
                     ▼
[ CORE DOMAIN: Lógica de Negocio POO & Servicios ]
                     │ (Consultas Fuertemente Tipadas)
                     ▼
[ DATABASE: SQLite Local (WAL) + Drizzle ORM ]
```

### Tecnologías Empleadas:
- **Base de Datos:** SQLite local en archivo `knowledgecore.db` configurado en modo WAL (*Write-Ahead Logging*) y con integridad referencial forzada (`PRAGMA foreign_keys = ON;`).
- **ORM & Migraciones:** **Drizzle ORM** (`@libsql/client`) con soporte para tipos de datos estrictos, esquemas relacionales declarativos y borrado en cascada.
- **Backend / Capa de Servicio:** **Node.js (v24+)** con **Fastify** y TypeScript. Implementación de controladores RESTful, validación RFC 9457 para Problem Details y documentación viva mediante OpenAPI 3.0 / Swagger UI.
- **Capa de Dominio:** Servicios desacoplados de persistencia (`KnowledgeSourceService`, `AcademicService`, `StudyService`, `DashboardService`) siguiendo principios SOLID y patrones de diseño limpios.
- **Frontend Web:** **React 19** con **Vite**, **TypeScript** y **Tailwind CSS**. Paleta de diseño dark obsidian extraída de Stitch (`#131313`, `#1c1b1b`, `#201f1f`, `#2a2a2a`) con iconos vectoriales de **Lucide React**.
- **Gestor de Paquetes:** **pnpm (v11+)** estructurado como monorepo modular.
- **Pruebas Automatizadas:** **Vitest** para pruebas unitarias de dominio y pruebas de integración de API; TypeScript para verificación estricta de tipos.

---

## 📂 Estructura del Proyecto (Monorepo)

```text
proyecto_05/
├── apps/
│   ├── api/                     # Servidor Fastify, rutas RESTful y contratos OpenAPI
│   │   ├── src/
│   │   │   ├── app.ts           # Configuración de Fastify, plugins y rutas
│   │   │   └── server.ts        # Punto de entrada y bind en puerto 3001
│   │   └── test/                # Pruebas de integración de endpoints y pipeline E2E
│   └── web/                     # Aplicación cliente React 19 SPA
│       ├── src/
│       │   ├── App.tsx          # Componente principal, enrutamiento interno y modales
│       │   ├── main.tsx         # Montaje del Virtual DOM
│       │   └── index.css        # Configuración global y tokens de Tailwind CSS
│       └── vite.config.ts       # Proxy inverso /api/v1 -> http://localhost:3001
├── packages/
│   ├── core/                    # Lógica de negocio y entidades de dominio (POO)
│   │   ├── src/
│   │   │   ├── academic/        # Servicios de materias, notas y ponderaciones
│   │   │   ├── dashboard/       # Agregadores de estadísticas y racha de estudio
│   │   │   ├── sources/         # Entidad KnowledgeSource y reglas de estado
│   │   │   ├── study/           # Servicios de clases, notas zen y flashcards
│   │   │   └── errors.ts        # Jerarquía de excepciones de dominio tipadas
│   │   └── test/                # Pruebas unitarias de reglas de negocio
│   └── db/                      # Capa de datos relacional
│       ├── src/
│       │   ├── schema.ts        # Definición de tablas SQLite con Drizzle ORM
│       │   ├── index.ts         # Factoría de conexión y configuración WAL
│       │   ├── seed.ts          # Datos semilla oficiales del sistema
│       │   └── import-malla.ts  # Importador de la malla curricular ING-DS (39 materias)
│       └── test/                # Pruebas de integridad relacional y cascada
├── knowledgecore.db             # Archivo SQLite local de base de datos
├── malla_curricular_carrera_ING-DS.json # Datos canónicos de la carrera
├── package.json                 # Scripts de orquestación global
└── pnpm-workspace.yaml          # Configuración de paquetes del monorepo
```

---

## 🔄 Flujo de Funcionamiento del Sistema

### 4.1 Hub Central Multifuente & Estadísticas
Es la pantalla de inicio principal (`currentView: 'hub'`).
- **Barra de Métricas y Hábitos de Estudio:**
  - **Horas de Estudio Totales:** Muestra el progreso acumulado de dedicación (ej. `142 / 220 hrs`).
  - **Días Activos:** Registro mensual de días con sesiones de estudio efectivas.
  - **Racha de Estudio (Streak):** Indicador visual con llama que premia la constancia diaria consecutiva (ej. `12 días seguidos`).
  - **Programas Activos:** Conteo en tiempo real de fuentes activas (Grado, Cursos y Talleres).
- **Tabla de Fuentes de Aprendizaje:**
  - Filtros por categoría: `Todos`, `Carrera / Grado`, `Cursos Cloud`, `Talleres IA`.
  - Muestra columnas de tipo con badges semánticos, nombre, entidad proveedora, carga horaria o créditos UC, barra de progreso y estado.
  - Al hacer clic en la **Carrera Universitaria**, navega al Dashboard de la Carrera.
  - Al hacer clic en un **Curso** (*p. ej. Terraform en AWS*) o en un **Taller** (*p. ej. Servidores MCP y Multi-Agentes*), abre inmediatamente su propio dashboard con temario, sesiones, drawer lateral y notas.
- **Calendario Multifuente & Próximas Entregas:** Consolida las entregas y horas de estudio de todas las fuentes registradas.

---

### 4.2 Dashboard de la Carrera Universitaria
Se accede desde el Hub Central o la barra de navegación lateral (`currentView: 'carrera-dashboard'`).
- **Encabezado Académico:** Muestra el grado (`Ingeniería en Desarrollo de Software`), código oficial (`ING-DS`), promedio general acumulado sobre 10 puntos (`GPA: 9.48 / 10`), periodo en curso (`Cuatrimestre II`) y créditos acumulados vs pendientes.
- **Selector Rápido de Cuatrimestres:** Permite filtrar instantáneamente entre los periodos actuales (`C2`, `C1`, `C3`) o abrir el modal de consulta con las 39 materias completas organizadas del Cuatrimestre 1 al 10.
- **Conmutador de Vista (Tabla vs Galería):**
  - **Modo Tabla:** Lista con código oficial, nombre, profesor, porcentaje de clases vistas, nota actual obtenida, badges de estado (`Cursando`, `Aprobada`, `Disponible`) y botones de acción (Editar, Eliminar, Ingresar).
  - **Modo Galería:** Cuadrícula de tarjetas con avance porcentual, créditos UC y acceso rápido a la materia.
- **Botón `+ Materia`:** Permite registrar una nueva asignatura formal en el cuatrimestre seleccionado.

---

### 4.3 Dashboard Modular de la Materia / Curso / Taller
Es la vista individual de cualquier módulo o asignatura (`currentView: 'materia-dashboard'`).
- **Encabezado Contextual:** Nombre de la cátedra, profesor asignado, avance global porcentual y accesos a temario, repositorio y syllabus.
- **Base de Datos de Clases / Sesiones:**
  - Tabla que detalla número de sesión (`01`, `02`, etc.), título del tema, cantidad de recursos didácticos adjuntos, entregable vinculado y estado de la sesión (`Visto`, `Activa`, `Pendiente`).
  - Botones de acción rápida en cada fila: **Editar Clase** (lápiz), **Eliminar Clase** (papelera) y **Ver Clase** (abre el Drawer lateral).
  - Botón **`+ Clase`** para añadir nuevas sesiones temáticas.
- **Tabla de Entregables de la Materia:**
  - Lista de proyectos, exámenes y tareas con fecha límite, nota obtenida vs máxima y estado evaluativo.
  - Botón **`+ Entregable`** para registrar entregas específicas de la asignatura.
  - Acciones para editar o eliminar entregables con actualización reactiva.

---

### 4.4 Drawer Lateral de Recursos & Clases
Al hacer clic en una clase de la tabla o presionar "Ver", se despliega un panel lateral deslizante (*drawer*):
- **Detalle de la Sesión:** Número de clase, fecha programada, duración estimada y profesor.
- **Lista de Recursos Didácticos Independientes:**
  - Renderiza los archivos adjuntos clasificados por formato: PDFs con peso en MB, ZIPs con código fuente o enlaces externos.
  - Cada clase tiene sus propios recursos didácticos de forma desacoplada (de 0 a múltiples archivos) sin stubs ni fallbacks artificiales.
  - Enlace de descarga interactivo para cada recurso.
  - Botón **`+ Recurso`**: Abre un modal directo para agregar un nuevo archivo o enlace a la clase en tiempo real.
- **Acceso Directo al Modo Zen:** Botón **`Notas (Modo Zen)`** para acceder a pantalla completa al editor de notas de la clase.

---

### 4.5 Modo Zen de Notas & Flashcards Active Recall
Pantalla completa dedicada a la escritura y retención (`currentView: 'zen-editor'`).
- **Editor Markdown Zen (Columna Izquierda - 70% del ancho):**
  - Barra de herramientas con accesos rápidos para títulos (`H1`, `H2`), negrita (`B`), cursiva (`I`), listas (`≡`), bloques de código (`<>`), citas (`”`) y fórmulas LaTeX (`Σ`).
  - Área de escritura con tipografía monoespaciada para evitar fatiga visual.
  - Indicadores en tiempo real de cantidad de palabras, minutos estimados de lectura y botón **Guardar** que persiste las notas en la base de datos local.
- **Panel de Active Recall & Flashcards (Columna Derecha - 30% del ancho):**
  - Listado de tarjetas de memoria asociadas a la lección.
  - Botón **`+ Flashcard`** en el encabezado: Abre un modal para redactar la pregunta / concepto clave y su respuesta técnica.
  - Almacenamiento con nivel de maestría (*Mastery Level*) para repetición espaciada.

---

### 4.6 Espacio de Estudio y Mazos de Flashcards (Active Recall Leitner)
Se accede desde la barra de navegación lateral fija (`currentView: 'estudio'`), inmediatamente debajo de *Calendario Unificado*.
- **Agrupación Inteligente por Categoría:**
  - **Carrera Universitaria:** Agrupa las asignaturas formales de la carrera (*Ingeniería en Desarrollo de Software*), organizadas por cuatrimestre y con métricas de retención acumuladas.
  - **Cursos y Talleres (Tratados como Mazos Independientes):** Se muestran directamente al nivel de materia, sin categorías anidadas innecesarias. Un curso (*p. ej. "Arquitectura Cloud & DevOps con AWS"*) o taller (*p. ej. "Taller Intensivo de Creación de Agentes de IA & MCP"*) es en sí mismo un mazo autónomo de estudio.
- **Métricas y Estado de Dominio del Mazo:**
  - Conteo y desglose en tiempo real: ⚪ Nuevas, 🔵 En Proceso / Repaso y 🟢 Dominadas.
  - Barra de progreso de retención visual con cálculo porcentual.
- **Modal Interactivo de Flip-Card:**
  - Al hacer clic en **`Estudiar Mazo`**, se abre una interfaz modal de estudio activo.
  - Tarjeta giratoria interactiva (*pregunta $\to$ respuesta*) con un clic.
  - Calificación en 3 niveles Leitner: `Repetir (Difícil)`, `En Proceso (Regular)` y `Dominada (Fácil)` con actualización en caliente sobre SQLite (`PUT /api/v1/flashcards/:id/mastery`).
  - Controles de navegación manual anterior/siguiente y barra de avance de la sesión.

---

### 4.7 Calendario Unificado de Entregables (Fiel a Stitch 45de29e2)
Se accede desde la barra de navegación lateral fija (`currentView: 'calendario'`) o mediante los accesos directos de calendario en la aplicación:
- **Navegación Temporal Superior:**
  - Encabezado dinámico (`Noviembre 2026`) con botones de control temporal: `Anterior`, `Siguiente` y acceso rápido a `Hoy`.
  - Botón principal **`+ Agendar Entregable`** para registrar proyectos, exámenes, talleres y tareas.
  - Botón **`Sincronizar`** con actualización reactiva en caliente sobre SQLite.
- **Tarjetas de Métricas del Mes (KPIs Superiores):**
  - **Carga Semanal `[CARGA]`:** Horas estimadas y conteo de materias/cursos activos (ej. `18.5 hrs` en 4 materias y 2 cursos).
  - **Ponderación en Juego `[PONDERACIÓN]`:** Cálculo automático del porcentaje de nota final en disputa en las evaluaciones pendientes (ej. `70% Nota Final`).
  - **Hitos Críticos `[CRÍTICO]`:** Conteo consolidado de entregas clave próximas a vencer.
- **Barra de Filtros Multifuente:**
  - Filtro interactivo por tipo de programa: `Todos`, `Carrera (Licenciatura)`, `Cursos` y `Talleres`.
  - Leyenda cromática de clasificación: Proyecto (`#70dba4`), Examen (`#ffb4ab`), Tarea (`#87ceff`), Taller/Lab (`#d7baff`).
- **Cuadrícula Mensual Completa (7 Columnas: Lunes a Domingo):**
  - Distribución estricta con días del mes anterior (Octubre), los 30 días de Noviembre y días de Diciembre.
  - Indicador destacado para el día actual (**Lunes 09** con badge `HOY` en esmeralda).
  - Chips interactivos de entregables por celda con hora de entrega, título de la actividad y código de la asignatura.
- **Base de Datos Inferior: Entregables a Punto de Vencer:**
  - Tabla relacional completa ordenada cronológicamente por urgencia.
  - Muestra columnas de **Urgencia** con pulsación animada (`¡Vence Hoy!`, `Vence en X días`), **Tipo**, **Título**, **Fuente / Origen**, **Fecha Límite**, **Ponderación** y acciones directas para **Editar** o **Eliminar** entregables.

---

### 4.8 Calendario Mensual en el Dashboard de la Carrera & Hub
Adicionalmente a la pantalla dedicada de Calendario Unificado, tanto el Hub Central como el Dashboard de la Carrera Universitaria cuentan con un calendario integrado de entregables con columna lateral de **Próximos Críticos** (conteo de días restantes `en 3 días`, `en 7 días`, etc.) y botón contextual de agendamiento.

---

### 4.9 Administración Integral (CRUD Modular)
El sistema cuenta con un ecosistema completo de modales con validaciones tanto en cliente como en servidor:
1. **Modal "Agendar Entregable":**
   - Selector de materia, título, selector visual de categoría (Proyecto, Parcial, Final, Tarea), fecha (`YYYY-MM-DD`), hora (`HH:MM`) y ponderación porcentual con validación de límite de 100%.
2. **Modal "Nueva / Editar Materia":**
   - Nombre, código, instructor, créditos UC, cuatrimestre asignado (C1 a C10), estado y nota acumulada.
3. **Modal "Nueva / Editar Clase":**
   - Título, fecha, resumen conceptual y gestor de recursos didácticos adjuntos integrados.
4. **Modal "Gestionar Recursos Didácticos":**
   - Permite añadir o quitar recursos en caliente desde el panel drawer lateral.
5. **Modal "Nueva Fuente de Conocimiento":**
   - Creación de nuevas carreras, cursos con horas o talleres prácticos.
6. **Modal "Nueva Flashcard":**
   - Redacción de pares pregunta/respuesta técnica con validación obligatoria.

---

## 🗄️ Modelo de Datos & Esquema Relacional

El esquema está definido en TypeScript mediante **Drizzle ORM** sobre SQLite en `packages/db/src/schema.ts`:

```
┌─────────────────────────────────┐
│        knowledge_sources        │
├─────────────────────────────────┤
│ id (PK, TEXT)                   │
│ name (TEXT)                     │
│ type (TEXT: degree|course|wks)  │
│ provider_or_platform (TEXT)     │
│ code (TEXT)                     │
│ status (TEXT)                   │
│ total_credits / hours           │
└────────────────┬────────────────┘
                 │ 1:N (ON DELETE CASCADE)
                 ▼
┌─────────────────────────────────┐
│            subjects             │
├─────────────────────────────────┤
│ id (PK, TEXT)                   │
│ source_id (FK -> sources)       │
│ name (TEXT)                     │
│ code (TEXT)                     │
│ instructor (TEXT)               │
│ credits (INTEGER)               │
│ period (TEXT)                   │
│ status (TEXT)                   │
│ current_grade (REAL)            │
└────────┬───────────────┬────────┘
         │ 1:N           │ 1:N (ON DELETE CASCADE)
         ▼               ▼
┌─────────────────┐  ┌──────────────────────────────┐
│     classes     │  │         deliverables         │
├─────────────────┤  ├──────────────────────────────┤
│ id (PK, TEXT)   │  │ id (PK, TEXT)                │
│ subject_id (FK) │  │ subject_id (FK)              │
│ session_number  │  │ title (TEXT)                 │
│ title (TEXT)    │  │ type (project|exam|assign)   │
│ date (TEXT)     │  │ weight_percentage (REAL)     │
│ summary (TEXT)  │  │ due_date (TEXT)              │
│ notes_markdown  │  │ status (pending|graded|...)  │
└────────┬────────┘  │ obtained_grade (REAL)        │
         │ 1:N       └──────────────────────────────┘
         ▼
┌─────────────────┐
│   flashcards    │
├─────────────────┤
│ id (PK, TEXT)   │
│ class_id (FK)   │
│ question (TEXT) │
│ answer (TEXT)   │
│ mastery_level   │
└─────────────────┘
```

---

## 🔌 API RESTful & Especificación OpenAPI

El servidor Fastify corre en el puerto `3001` y expone endpoints estandarizados con documentación interactiva en **`http://localhost:3001/docs`**:

| Método | Endpoint | Descripción |
| :--- | :--- | :--- |
| **GET** | `/api/v1/sources` | Lista todas las fuentes de conocimiento con filtros opcionales. |
| **POST** | `/api/v1/sources` | Registra una nueva carrera, curso o taller. |
| **PUT** | `/api/v1/sources/:id` | Actualiza los datos o avance de una fuente. |
| **DELETE**| `/api/v1/sources/:id` | Elimina la fuente y todos sus datos en cascada. |
| **GET** | `/api/v1/subjects` | Lista asignaturas o módulos (filtrable por `sourceId`). |
| **POST** | `/api/v1/subjects` | Crea una nueva materia en la malla curricular. |
| **PUT** | `/api/v1/subjects/:id` | Modifica datos, notas o estado de una materia. |
| **DELETE**| `/api/v1/subjects/:id` | Elimina la materia y sus dependencias en cascada. |
| **GET** | `/api/v1/classes` | Lista clases y sesiones (filtrable por `subjectId`). |
| **POST** | `/api/v1/classes` | Registra una nueva clase con temario y recursos. |
| **PUT** | `/api/v1/classes/:id` | Actualiza la información o recursos de una clase. |
| **DELETE**| `/api/v1/classes/:id` | Elimina una sesión de clase. |
| **PUT** | `/api/v1/classes/:id/notes` | Guarda las notas del editor Zen en formato Markdown. |
| **GET** | `/api/v1/deliverables` | Lista tareas, exámenes y proyectos (filtrable por `subjectId`). |
| **POST** | `/api/v1/deliverables` | Registra un nuevo entregable evaluativo con ponderación. |
| **PUT** | `/api/v1/deliverables/:id` | Modifica título, fecha límite o ponderación del entregable. |
| **DELETE**| `/api/v1/deliverables/:id` | Elimina un entregable registrado. |
| **POST** | `/api/v1/deliverables/:id/grade` | Asigna calificación obtenida y retroalimentación técnica. |
| **GET** | `/api/v1/flashcards` | Obtiene las flashcards de estudio vinculadas a una clase. |
| **POST** | `/api/v1/flashcards` | Crea una nueva tarjeta de estudio activo (Active Recall). |
| **GET** | `/api/v1/dashboard/gpa` | Calcula el promedio general acumulado en escala oficial. |
| **GET** | `/api/v1/dashboard/calendar` | Retorna los eventos y entregas del mes para el calendario. |

---

## 🚀 Guía de Instalación y Puesta en Marcha

### Prerrequisitos
- **Node.js:** Versión 20+ o superior (probado y certificado en Node 24).
- **pnpm:** Versión 9+ o superior (`pnpm -v`).

### 1. Clonar el repositorio e instalar dependencias
```bash
# Instalar todas las dependencias del monorepo
pnpm install
```

### 2. Inicializar la Base de Datos Relacional SQLite
```bash
# 1. Ejecutar migraciones estructurales de Drizzle
pnpm db:migrate

# 2. Cargar datos semilla canónicos y la malla curricular completa de ING-DS
pnpm db:seed
```

### 3. Levantar los Servicios en Desarrollo
Puedes ejecutar la API y la aplicación web de forma concurrente o en terminales independientes:

```bash
# Terminal 1: Iniciar el backend Fastify (Puerto 3001)
pnpm api:dev

# Terminal 2: Iniciar la interfaz React Vite (Puerto 3000)
pnpm web:dev
```

- **Acceso Web:** [http://localhost:3000](http://localhost:3000)
- **Documentación Swagger UI:** [http://localhost:3001/docs](http://localhost:3001/docs)

---

## 🧪 Verificación de Calidad y Suites de Prueba

Para asegurar la confiabilidad del sistema y la ausencia de regresiones, KnowledgeCore cuenta con pruebas automáticas distribuidas por cada capa:

```bash
# Ejecutar todas las suites de prueba en el monorepo
pnpm test
```

### Cobertura de Pruebas:
1. **`packages/db`:** Verificación de creación de tablas, inserción de semillas, restricciones de clave foránea huérfana, unicidad de claves primarias y borrado en cascada (`ON DELETE CASCADE`).
2. **`packages/core`:** 10 pruebas unitarias de lógica de negocio (reglas de estado de fuentes, cálculo de promedios ponderados, validación de límite de ponderación al 100%, cálculo de notas finales y excepciones RFC 9457).
3. **`apps/api`:** 14 pruebas de integración de endpoints Fastify y validación del pipeline End-to-End completo (ciclo de vida de una materia, inscripción de clases, agendamiento de entregables y calificación).
4. **`apps/web`:** Verificación de compilación TypeScript y empaquetado de producción con Vite (`pnpm --filter @knowledgecore/web build`).

---

**KnowledgeCore** • Desarrollado por **Chris (`@cristianchipman`)** en colaboración con **AMI** & **Forge**.
