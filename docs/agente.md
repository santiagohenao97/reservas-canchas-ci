# Trabajar con un agente para crear tu CI

El agente **propone**; tú **verificas y decides**. Un workflow que no puedes explicar no es tuyo.

## Con Claude Code (este repositorio ya trae lo necesario)
Abre Claude Code en la carpeta del repositorio. Hay una skill (`ci-workflow`) y tres comandos:

| Comando | Qué hace | Escribe archivos |
|---|---|---|
| `/crear-ci <qué quieres validar>` | Lee tu proyecto, propone un workflow y una ficha `docs/ci.md`. Muestra el diff antes de crear nada. | Solo `.github/` y `docs/ci.md`, con tu confirmación |
| `/revisar-ci [ruta]` | Revisa un workflow contra la checklist. Cita la línea, nombra el problema, propone arreglo. | No |
| `/mutante [archivo]` | Propone 5 cambios pequeños al código y predice si tus pruebas los detectan. Al menos uno sobrevive. | No |

Ejemplo: `/crear-ci que se prueben mis funciones de validación en cada pull request`

## Sin Claude Code (cualquier asistente)
Pega esto y adjunta tu código y tus pruebas. **No pegues `.env`, claves ni tokens.**

> Actúa como revisor de CI. Para este proyecto propón un workflow de GitHub Actions mínimo que valide una sola cosa real: (lo que quiero validar). Reglas: `permissions: contents: read`; sin `|| true` ni `continue-on-error` sobre pruebas; `set -o pipefail` si usas `tee`; sin secretos en el código ni impresos; no uses datos del evento dentro de `run:`; acciones con versión, nunca `@main`; `timeout-minutes`; `needs` si algo depende de las pruebas. Después dime en una tabla qué valida, qué NO valida y qué está solo diseñado. No hagas suposiciones sobre cosas que no ejecutaste.

## Tu protocolo de verificación (6 pasos, obligatorio)
1. **Léelo tú primero.** ¿Puedes explicar cada línea? Si no, pregúntale al agente qué hace y por qué.
2. **Quita lo que no pediste.** Menos es más: cada paso extra es algo que debes mantener.
3. **Corre `actionlint`** (o `/revisar-ci`). Lo que marque, entiéndelo antes de arreglarlo.
4. **Córrelo en verde** y abre el paso: ¿cuántas pruebas corrieron de verdad?
5. **Rómpelo a propósito** con un cambio controlado y lee el mensaje. Si no falla, tu comprobación no comprueba nada.
6. **Escribe qué no valida.** Si la ficha del agente dice más de lo que ejecutaste, corrígela.

## Lo que el agente NO debe hacer (y tú tampoco)
- Pedirte o leer credenciales. Si lo hace, detente.
- Hacer push, crear secretos o cambiar la protección de ramas por ti.
- Dar un verde por garantía de que “todo está bien”.
