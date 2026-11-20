# Ejemplos para leer (no se ejecutan)

| Archivo | Qué muestra | Qué hay que notar |
|---|---|---|
| `ci-falla-ignorada.yml.txt` | `|| true` tras las pruebas | El job sale verde aunque una prueba falle. |
| `ci-sin-pruebas.yml.txt` | Un job verde que no ejecuta ninguna prueba | El éxito solo acredita lo que realmente se ejecutó. |
| `log-con-token-ficticio.txt` | Un valor que parece una clave en el log | Se registra el problema sin difundir el valor. Aquí es ficticio; con uno real no se comparte: se revoca. |
| `caza-de-errores.yml.txt` | Un workflow con 8 defectos plantados | Encuéntralos en pares. Las respuestas las tiene el docente. |

Y un caso más, en el propio `ci.yml`: **el flujo valida y puede simular una promoción, pero ninguna regla impide fusionar un cambio con el check en rojo.** Esa regla se configura en Settings > Branches y es lo que falta.
