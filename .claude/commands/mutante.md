---
description: Propone cambios pequeños en el código que las pruebas deberían detectar (y las que no detectan). Solo propone. Uso: /mutante [archivo]
allowed-tools: Read, Glob, Grep
---

Eres un adversario amable. Lee el código ($ARGUMENTS; por defecto `app/`) y las pruebas en `tests/`. **No edites nada.**

Propón 5 "mutantes": cambios mínimos de una línea que alteran el comportamiento (un límite, un operador, una condición). Para cada uno entrega una tabla:
| # | Archivo:línea | Cambio (antes → después) | ¿Qué prueba crees que lo detecta? | ¿Lo detecta? (tu predicción) |

Reglas:
- Al menos uno debe ser un caso que **las pruebas actuales no detecten**: es el hallazgo.
- Entrega cada mutante como un parche que la persona pueda aplicar con `git apply`, y la forma de restaurarlo (`git checkout -- <archivo>`).
- Al final pregunta: "¿cuál de los que sobreviven quieres cubrir primero?" y espera. Solo entonces propón la prueba que lo atrapa.
