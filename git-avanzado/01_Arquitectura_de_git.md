# Arquitectura de Git

## ¿Qué es un repositorio en Git?

Un repositorio en Git es una carpeta del sistema de archivos (llamada *árbol de trabajo*) que contiene un directorio oculto `.git`.  
Este directorio guarda **toda la información del control de versiones**.  
Para respaldar un proyecto, basta con conservar esta carpeta `.git`.

---

## 📂 Componentes importantes de `.git`

- **`objects/`**: contiene la base de datos de Git (blobs, trees, commits).
- **`refs/`**: referencias como:
  - `heads/`: ramas locales
  - `remotes/`: ramas remotas
  - `tags/`: etiquetas
  - `stash/`: pila de cambios temporales
- **`HEAD`**: indica la rama o commit actual.
- **`index`**: staging area (cambios preparados para el próximo commit).

---

## 🔄 Tipos de objetos en Git

- **`blob`**: representa el contenido de un archivo (sin su nombre).
- **`tree`**: representa un directorio con blobs y otros trees (con nombres y permisos).
- **`commit`**: representa un punto en la historia con:
  - autor
  - fecha
  - mensaje
  - referencia al commit anterior

---

## 📜 Commits y visualización

- Los commits construyen la historia del proyecto.
- Se visualizan con `git log`.
- En lugar de usar hashes, Git permite usar **referencias** más amigables.

---

## 🏷️ Referencias (refs)

- **`refs/heads/`**: ramas locales (ej. `main`)
- **`refs/remotes/`**: ramas de repositorios remotos (ej. `origin/main`)
- **`refs/stash/`**: cambios temporales con `git stash`
- **`refs/tags/`**: etiquetas para marcar versiones importantes

---

## 🧭 El archivo `HEAD`

- Apunta a una rama (`ref`) o directamente a un commit (estado *detached head*).
- Si hacemos commits en estado detached y no los asociamos a una rama, podemos **perderlos**.
- Para volver al estado anterior:  
  ```bash
  git switch -
  ```
- Para crear una nueva rama desde el estado actual:  
  ```bash
  git switch -c nueva-rama
  ```

---

## 🔁 Referencias relativas

Podemos navegar la historia con:
- `HEAD~1` o `HEAD^`: commit anterior
- `HEAD~2`: dos commits atrás

Esto permite trabajar cómodamente sin recordar hashes.