---
description: Revisa un workflow existente contra la checklist de la skill ci-workflow (solo lectura). Uso: /revisar-ci [ruta]
allowed-tools: Read, Glob, Grep, Bash(actionlint:*)
---

Revisa el workflow indicado ($ARGUMENTS; si está vacío, todos los de `.github/workflows/`). **Solo lectura: no edites nada.**

Para cada problema, en este formato exacto:
- **Cita** la línea o frase problemática, tal cual.
- **Problema** en una frase (qué podría pasar).
- **Arreglo** propuesto, o una pregunta dirigida si falta contexto.

Mira, en este orden: disparadores (¿corre en pull requests?), permisos, fallos ocultos (`|| true`, `continue-on-error`, `tee` sin `pipefail`), datos del evento en `run:`, secretos impresos, acciones sin versión o con `@main`, `pull_request_target`, artefactos que suben de más, `timeout-minutes`, y qué impide que un cambio en rojo avance.
Si `actionlint` existe, córrelo y separa lo que él encontró de lo que encontraste tú.
Cierra con: qué valida este workflow, qué no valida, y cuál es su estado real (ejecutado / pendiente / no existe). No cites credenciales aunque las veas: di dónde están.
