## Uso de la herramienta Git

### Inicializar un repositorio y subirlo por primera vez

```bash
# Iniciar el repositorio
git init

# Agregar el repositorio remoto (reemplaza con tu URL real)
git remote add origin https://github.com/nombre_de_usuario_github/repositorio_donde_subiras_cosas.git

# Agregar todos los archivos al staging
git add .

# Hacer el primer commit
git commit -m "Primera subida: En esta parte va un comentario para la subida"

# Establecer la rama principal como 'main'
git branch -M main

# Subir al repositorio remoto
git push -u origin main
```

---

### Subir actualizaciones a un repositorio ya existente

#### Subir todos los archivos modificados

```bash
git add .
```

#### Subir solo un archivo o carpeta

```bash
git add tools/wfuzz.md
```

#### Hacer commit con mensaje

```bash
git commit -m "Agregué un archivo que se llamaba wfuzz"
```

#### Subir los cambios a GitHub

```bash
git push
```
