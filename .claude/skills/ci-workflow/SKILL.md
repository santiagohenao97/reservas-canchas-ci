---
name: ci-workflow
description: Diseña, revisa y documenta workflows de GitHub Actions (integración continua) para un proyecto real. Úsala cuando se pida crear, mejorar o revisar un workflow de CI, o explicar qué valida y qué no.
---

# Skill: workflow de CI honesto

Objetivo: que el workflow compruebe algo real y que quien lo lea sepa **qué valida, qué no y qué condiciona el avance**.
El agente propone; quien estudia verifica y decide. Nunca presentes un verde como garantía.

## Procedimiento
1. **Leer el proyecto** (solo lectura): lenguaje, cómo se corren las pruebas, si hay `Dockerfile`, qué hay en `.github/`. No leas ni muestres `.env` ni archivos de credenciales.
2. **Preguntar o decidir una cosa que validar**: pruebas de código, construcción de la imagen, o una prueba de función. Una, bien entendida. No añadas escaneos hasta que la persona sepa interpretar la salida y su alcance.
3. **Generar el workflow mínimo** a partir de `plantillas/ci-python.yml`, adaptándolo. Un archivo en `.github/workflows/`.
4. **Validar sin red de seguridad ajena**: corre el comando de pruebas localmente y, si existe, `actionlint`. Informa el resultado real; si algo no se pudo ejecutar, dilo.
5. **Entregar una ficha** en `docs/ci.md` con la tabla "qué valida / qué no / estado real" (ver formato abajo).
6. **Proponer, no aplicar**: muestra el diff y espera confirmación antes de crear archivos fuera de `.github/` y `docs/ci.md`.

## Reglas que no se rompen
- Permisos mínimos: `permissions: contents: read` salvo razón escrita para más.
- Un fallo debe fallar: nada de `|| true`, ni `continue-on-error: true` sobre pruebas, ni scripts que traguen el código de salida. En tuberías con `tee`, usar `set -o pipefail`.
- Cero secretos en el código, ni impresos en el log. Si el flujo necesita uno: nombra cuál y para qué, y pide a la persona que lo cree en Settings. **Nunca pidas ni leas credenciales.**
- No interpolar datos del evento (`github.event.*.title`, nombres de rama, cuerpos) dentro de `run:`; pasarlos por `env:`.
- No usar `pull_request_target` con checkout del código del PR.
- Acciones oficiales con versión (`@v4`); las de terceros, idealmente fijadas por SHA. Nunca `@main`.
- Siempre `timeout-minutes`. Considerar `concurrency` para cancelar ejecuciones duplicadas.
- `needs` para que construir o publicar dependa de que las pruebas pasen. Promoción: nunca automática sin una regla explícita; aprobación mediante `environment`.
- No hagas push, no configures protección de ramas, no crees secretos ni variables. Eso lo hace la persona.

## Formato de la ficha `docs/ci.md`
```
# Qué valida el CI
Disparador: …   Comprobación: <comando>   Permisos: …   Artefactos: …

## Estado real
| Qué | Estado |
|---|---|
| <comprobación> | ejecutado / comprobado con una muestra / solo diseñado |
| Bloqueo de la rama si falla | pendiente / configurado |
| Publicación o despliegue | no existe / simulado / configurado |

## Qué NO valida todavía
- …
## Qué falta demostrar
- (por ejemplo: disparador automático, máquina limpia)
```
Etiqueta cada afirmación con lo que realmente se ejecutó. Si no se ejecutó en GitHub, di "solo diseñado" o "equivalencia local".
