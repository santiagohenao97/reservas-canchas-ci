# Caza de errores · respuestas (solo docente; no la compartas antes del ejercicio)

| # | Línea | Defecto | Por qué importa | Arreglo |
|---|---|---|---|---|
| 1 | `on: pull_request_target` | Corre en el contexto del repositorio base, con secretos y token con escritura | Un PR de un tercero ejecuta código con tus credenciales | Usar `pull_request` (sin secretos para forks) |
| 2 | `ref: ${{ github.event.pull_request.head.sha }}` junto con (1) | Hace checkout del código del PR dentro de un contexto privilegiado | Combinación clásica de ataque: código ajeno + secretos | No hacer checkout del PR en `pull_request_target`; separar en dos flujos |
| 3 | `permissions: write-all` | Permisos máximos del token | Si algo se compromete, puede escribir en todo | `permissions: contents: read` y solo lo extra con razón |
| 4 | `actions/checkout@main` | Rama móvil, no una versión | El resultado cambia sin que cambie tu código | `@v4` o, mejor, un SHA fijo |
| 5 | `echo "Revisando: ${{ github.event.pull_request.title }}"` | Inyección de scripts: el título del PR entra directo al comando | Quien escribe el título controla lo que se ejecuta | Pasarlo por `env:` y usar `"$TITULO"` |
| 6 | `pytest \|\| true` | Oculta el resultado de las pruebas | El job sale verde aunque fallen | Quitar `\|\| true`; que el fallo falle |
| 7 | `echo "token=${{ secrets.TOKEN_DESPLIEGUE }}"` | Imprime un secreto en el log | El enmascarado no es infalible y los logs se leen | No imprimir secretos |
| 8 | `path: .` en `upload-artifact` | Sube todo el directorio, incluido lo que no debe salir | Puede incluir `.env`, claves, el historial de git | Subir solo el archivo necesario |

Bonus (no cuentan en los 8): no declara `timeout-minutes`, no instala Python con una versión fija, no hay `concurrency`, y el nombre `ci` no dice qué valida. `pip install -r requirements.txt` sin caché ni versión fija de Python tampoco es reproducible.

Cómo usarlo: pares, 8 minutos, un punto por defecto encontrado y explicado (qué podría pasar). Gana el par que además propone el arreglo correcto. Al terminar, pásales `actionlint` (ver `COMANDOS`). En mi prueba solo marcó la inyección (#5): 1 de 8. Esa es otra lección: una herramienta ayuda, no reemplaza el criterio.
