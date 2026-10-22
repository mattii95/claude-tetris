---
description: Crea un worktree aislado en .trees/ y ejecuta ahí el requerimiento
argument-hint: <requerimiento a implementar>
---

Requerimiento: $ARGUMENTS

1. Deriva un nombre corto en kebab-case (2-4 palabras, sin espacios ni acentos) a partir del requerimiento, p. ej. `pieza-fantasma`.
2. Crea el worktree desde la rama actual: `git worktree add .trees/<nombre> -b <nombre>`. Si la rama o carpeta ya existe, elige otro nombre.
3. Implementa el requerimiento **únicamente dentro de `.trees/<nombre>`**: todas las lecturas, ediciones y comandos usan esa ruta. No toques el árbol de trabajo principal ni su rama.
4. Respeta el CLAUDE.md del proyecto. Verifica el cambio y haz commit en la rama del worktree (sin push ni merge salvo que se pida).
5. Al terminar informa: nombre del worktree, rama, ruta y resumen de cambios.
