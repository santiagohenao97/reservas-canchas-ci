# Empieza aquí · Encuentro 4

Este ejemplo (`reservas-canchas`) sirve para practicar una comprobación automática: qué se valida, cómo se lee un fallo, qué todavía no está automatizado y cómo trabajar con un agente sin perder el criterio. Usa solo datos ficticios.

## Cómo obtenerlo
- Si ya tienes el repositorio: `git fetch origin` y `git checkout ejercicio-4-ci`.
- Si no: `git clone -b ejercicio-4-ci https://github.com/santiagohenao97/reservas-canchas.git`
- Sin git: descarga el ZIP de la rama desde GitHub (Code → Download ZIP) y descomprímelo.

## Qué necesitas
- **Con GitHub:** un repositorio tuyo, **vacío**, donde subir estos archivos **a la rama principal** (el botón *Run workflow* solo aparece si el workflow está en la rama por defecto). Desde la carpeta que clonaste:
  ```
  git remote set-url origin https://github.com/TU_USUARIO/TU_REPO.git
  git push -u origin ejercicio-4-ci:main
  ```
  Las ejecuciones aparecen en la pestaña **Actions**.
  - Si prefieres un fork: al crearlo desmarca “Copy the main branch only” y, en la pestaña Actions del fork, pulsa “I understand my workflows, go ahead and enable them”. Los forks traen los flujos apagados.
- **Sin GitHub:** Python 3 y una terminal. Puedes correr la comprobación en tu equipo (paso 1). Eso **no** demuestra cómo trabaja el servicio de CI: anota qué te falta probar.
- Docker es opcional: solo lo usa el job `construccion`.

## Lo mínimo para verlo
**1. Corre las pruebas en tu equipo** (desde la carpeta del repositorio):
```
python3 -m unittest discover -s tests -v
```
En Windows usa `py -m unittest discover -s tests -v`. Debe terminar con `OK`.

**2. Mira el flujo.** Abre `.github/workflows/ci.yml`. Busca: cuándo se dispara (`on`), permisos, matriz, qué job depende de otro (`needs`) y qué hace el job `promocion`.

**3. Véelo correr (con GitHub).** Sube el repositorio y abre Actions. Para verlo en vivo: **Run workflow** (ejecución manual). Abre la ejecución y entra al job `pruebas`. Al final de la página de la ejecución verás el **resumen** y los **artefactos**.

**4. Rómpelo a propósito.** En una rama nueva, cambia la línea 6 de `app/main.py`: `2[0-1]` por `2[0-3]` (acepta 23:00). Haz commit y push. Debe fallar la prueba `test_hora_fuera_de_rango` en ambas versiones de Python, y `construccion` **no corre**. Lee el mensaje antes de tocar nada.
Sin GitHub, lo mismo en tu equipo:
```
git apply cambios/rompe-horario.patch
python3 -m unittest discover -s tests -v
git checkout app/main.py
```

**5. Mira qué no cubre tu verde.** El paso de cobertura lista las líneas de `app/main.py` que ninguna prueba ejecuta. ¿Cuántas son? ¿Qué cambio podrías hacer ahí sin que el CI se entere?

**6. Trabaja con un agente.** Lee `docs/agente.md`. Prueba `/crear-ci`, `/revisar-ci` o `/mutante` (o el texto para cualquier asistente) y aplica el protocolo de verificación de 6 pasos.

## Si no puedes ejecutar
Lee la carpeta `evidencias/`: trae la salida correcta y la fallida de las pruebas, y compáralas.

## Pregúntate
- ¿Qué cambio protege cada prueba? ¿Qué cambio **no** protege ninguna?
- Si el job sale en verde, ¿qué sabes y qué no sabes?
- ¿Qué impide hoy que una versión con un check en rojo se fusione? (Pista: mira el final de `ci.yml`.)
- ¿Qué secretos usa este flujo? ¿Cuáles usarías tú, y dónde irían?
- ¿Qué parte de lo que propuso el agente no podrías defender frente a otra persona?
