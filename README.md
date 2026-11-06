# 📚 Apuntes ASIR - Web con GitHub Pages

Este proyecto contiene la web completa de apuntes para el ciclo superior de **ASIR** (Administración de Sistemas Informáticos en Red), generada con **Material for MkDocs**.

---

## 🚀 Cómo probar la web en tu ordenador (modo local)

Para ver la web en local mientras escribes tus apuntes:

1. Abre una terminal dentro de esta carpeta:
   ```bash
   cd ~/apuntes-asir
   ```

2. Instala MkDocs Material si aún no lo tienes:
   ```bash
   pip install -r requirements.txt --break-system-packages
   ```

3. Arranca el servidor local:
   ```bash
   mkdocs serve
   ```

4. Abre en tu navegador web: [http://127.0.0.1:8000](http://127.0.0.1:8000).  
   ¡Cualquier cambio que guardes en los archivos `.md` se actualizará automáticamente en tu navegador!

---

## 🌐 Cómo publicarlo en tu cuenta de GitHub

1. Entra en tu cuenta de **[GitHub](https://github.com/)** y crea un nuevo repositorio llamado:
   `apuntes-asir` (o el nombre que prefieras). **No** marques la opción de añadir README ni .gitignore porque ya los tenemos aquí.

2. En tu terminal, conecta este proyecto a tu repositorio remoto:
   ```bash
   cd ~/apuntes-asir
   git remote add origin https://github.com/TU_USUARIO/apuntes-asir.git
   git push -u origin main
   ```

3. En GitHub, ve a **Settings** -> **Pages**:
   - En **Build and deployment** > **Source**, comprueba que esté en **Deploy from a branch**.
   - Selecciona la rama **gh-pages** (que se creará automáticamente gracias a GitHub Actions) y pulsa **Save**.

4. ¡Listo! En 2-3 minutos tu web estará publicada en:
   `https://TU_USUARIO.github.io/apuntes-asir/`
