# Mejores practicas

## 🧱 El commit: unidad básica en Git

El concepto más importante en Git es el **commit**, ya que representa una unidad básica de trabajo.  
Debe agrupar **cambios relacionados** y ser:
- **Pequeño**
- **Atómico** (que haga una sola cosa)

### 🎯 ¿Qué es un cambio atómico?
Un cambio que **no se puede dividir más sin perder sentido**.  
Ese es el tamaño ideal para nuestros commits.

---

## 💾 Commits como puntos de guardado

- Permiten avanzar de forma incremental.
- Podemos experimentar y **volver atrás si algo falla**.
- Funcionan como una especie de "guardar partida" del proyecto.

---

## ❌ Evitar `git add .`

Recomendación: **no usar** `git add .`.

En su lugar, usar herramientas para agregar cambios de forma **selectiva**:
- IDEs modernos: permiten seleccionar cambios archivo por archivo.
- [LazyGit](https://github.com/jesseduffield/lazygit): interfaz amigable desde la terminal.
- `git add -p`: permite agregar cambios por bloques de forma interactiva.

---

## 📝 Escribir buenos mensajes de commit

- Deben ser **descriptivos y concisos**: qué y por qué.
- Si el mensaje es muy largo → probablemente el commit es demasiado grande.
- **Tiempo verbal recomendado**: presente imperativo (ej. "Fix typo", no "Fixed typo").
- **Idioma recomendado**: inglés, para mantener coherencia y entendimiento entre todo el equipo.

---

## 🛠️ Corregir un commit

### 🔄 Si todavía no hiciste push:
```bash
git commit --amend
```
Permite modificar el último commit (mensaje y/o contenido).

### 🧯 Si ya hiciste push:
```bash
git revert <commit_hash>
```
Crea un nuevo commit que **revierte los cambios anteriores** (sin alterar la historia).

### ⚠️ Cuidado con `git reset`
```bash
git reset --soft <commit_hash>
```
Permite rehacer un commit, pero **modifica la historia**.

Al hacer push después, Git pedirá forzar el push.  
Nunca uses `--force`, preferí:
```bash
git push --force-with-lease
```
Esto verifica que nadie más haya modificado la rama.

---

## 🚫 Nunca reescribas historia compartida

Modificar la historia de una rama compartida puede generar conflictos y hacerla incompatible con los repos de otros colaboradores.  
**Evitalo siempre**.
