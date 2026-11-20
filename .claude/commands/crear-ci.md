---
description: Crea (propone) un workflow de CI para este proyecto usando la skill ci-workflow. Uso: /crear-ci <qué quieres validar>
allowed-tools: Read, Glob, Grep, Write(.github/**), Write(docs/ci.md), Edit(.github/**), Bash(python3 -m unittest:*), Bash(actionlint:*), Bash(git status:*), Bash(git diff:*), Bash(ls:*)
---

Aplica la skill `ci-workflow` para este repositorio.

Lo que la persona quiere validar: $ARGUMENTS

Pasos:
1. Lee el proyecto en solo lectura (no abras `.env`). Resume en 3 líneas qué encontraste: lenguaje, pruebas, Dockerfile, flujos existentes.
2. Si lo que quiere validar es ambiguo, haz **una** pregunta corta antes de escribir nada.
3. Propón el workflow y **muestra el diff antes de crear nada**. Espera confirmación.
4. Con confirmación: crea el workflow en `.github/workflows/`, corre las pruebas en local y `actionlint` si existe, y reporta lo que realmente salió.
5. Escribe `docs/ci.md` con la tabla de estado real. Lo que no se ejecutó en GitHub va como "solo diseñado".
6. Termina con 3 preguntas que la persona debe poder responder sobre el flujo: qué lo dispara, qué prueba protege el cambio y qué impide hoy que una versión en rojo avance.

No hagas push, no toques secretos ni la configuración del repositorio.
