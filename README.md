# QA multiagente para Azure DevOps

Primera versión basada en instrucciones para analizar Historias de Usuario/PBI y diseñar casos de prueba de `SGAplicaciones`. El resultado es un borrador revisable; no se ejecutan pruebas ni se publican Work Items o Test Cases automáticamente.

## Estructura

| Archivo | Uso |
| --- | --- |
| `AGENTS.md` | Reglas comunes, seguridad y ejecución desde un PBI |
| `.codex/agente-orquestador.md` | Coordinación y revisión de entregables |
| `.codex/agente-analista.md` | Requisitos, criterios, riesgos, dudas y escenarios |
| `.codex/agente-testdesigner.md` | Casos positivos, negativos, de frontera y trazabilidad |
| `config/projects.json` | Catálogo de nueve proyectos con nombres e IDs; sin proyecto predeterminado |

## Requisitos y ejecución

Usa una sesión de Codex con acceso al checkout y al servidor MCP global `azure-devops`, autenticado mediante la configuración segura del entorno. Este repositorio no contiene secretos ni configura un servidor adicional. Los agentes no cargan `projects.json` automáticamente: deben leerlo siguiendo `AGENTS.md`.

Desde la raíz del repositorio, solicita por ejemplo:

> Lee AGENTS.md y ejecuta como orquestador el flujo QA para el PBI 12345 del proyecto SG_Synergy. Usa los archivos del analista y diseñador. Consulta Azure DevOps solo en modo lectura y entrega análisis, casos, trazabilidad, duplicados y preguntas abiertas. No crees ni modifiques Work Items o Test Cases.

`12345` es un ejemplo, no un PBI validado. Proporciona un ID real y un proyecto explícito. El catálogo procede de la consulta de proyectos realizada durante la preparación; no garantiza acceso permanente y puede necesitar actualización si cambia la organización.

El flujo es: **lectura del PBI → análisis → revisión → diseño → revisión de cobertura y duplicados → borrador**. El resultado debe identificar la revisión del PBI, separar evidencia de supuestos y distinguir diseño de ejecución. Si faltan criterios o acceso, el informe debe señalar los bloqueos.

## Seguridad

Solo lectura por defecto. Antes de cualquier creación o modificación futura, presenta contenido y destinos concretos, verifica duplicados y obtiene aprobación explícita del usuario. Nunca almacenes PAT, tokens ni sus versiones codificadas. No cambies la configuración global MCP. Consulta las reglas completas en `AGENTS.md`.

## Limitaciones de esta versión

- Los Markdown definen roles; no registran subagentes ejecutables ni activan capacidades multiagente de Codex. El uso de agentes separados depende de las herramientas y permisos de la sesión. Sin delegación disponible, se ejecutan los roles secuencialmente, declarando esa modalidad.
- No incluye un runtime de orquestación, configuración nativa de subagentes, CLI propia, almacenamiento de estado ni pipeline de CI. La coordinación se realiza mediante instrucciones en la sesión.
- Una conexión MCP y lectura de proyectos exitosas no validan todos los permisos sobre PBIs o Test Cases. La disponibilidad de herramientas y esquemas debe comprobarse en cada sesión.
- La consulta de duplicados puede ser parcial por permisos, paginación o cobertura de búsqueda; debe declararse. No hay persistencia automática ni garantía transaccional frente a cambios concurrentes.
- No incluye Playwright, automatización ni ejecución de pruebas. El proyecto llamado `Playwright` aparece únicamente como entrada del catálogo.
- No se ha ejecutado un flujo completo con un PBI real como parte de la creación de estos archivos.
