
# Proyecto WordPress con Docker en Windows

Este proyecto permite levantar un entorno local de WordPress utilizando Docker y Docker Compose en Windows.

## Requisitos Previos

Antes de comenzar, asegúrate de tener los siguientes programas instalados en tu máquina:

### 1. **Docker Desktop**
   - Descarga e instala **[Docker Desktop para Windows](https://www.docker.com/products/docker-desktop)**.
   - Durante la instalación, asegúrate de habilitar **WSL 2** si estás utilizando Windows 10 o 11.
   - **Verifica que Docker esté funcionando correctamente** ejecutando el siguiente comando en PowerShell o CMD:

     ```bash
     docker --version
     ```

### 2. **WSL 2** (solo para Windows 10 o 11)
   - Si usas **Windows 10** o **Windows 11**, asegúrate de tener **WSL 2** instalado. Para instalar **WSL 2**, abre PowerShell como administrador y ejecuta:

     ```bash
     wsl --install
     ```

   - **Nota**: Si ya tienes WSL 1 y deseas actualizar a WSL 2, puedes usar el siguiente comando:

     ```bash
     wsl --set-default-version 2
     ```

   - Luego, puedes instalar una distribución de Linux desde la **Microsoft Store**, como **Ubuntu**.

### 3. **Git** (opcional)
   - Si prefieres clonar el repositorio desde GitHub, necesitarás tener **Git** instalado. Puedes descargarlo desde **[aquí](https://git-scm.com/)**.

## Instrucciones

### 1. Clonar el repositorio (si es necesario)
Si aún no tienes el proyecto, clónalo desde GitHub con el siguiente comando:

```bash
git clone https://github.com/pamelars86/wordpress-local
cd wordpress-docker
```

### 2. Configurar el entorno en Windows

#### Para usuarios con **WSL 2**:
Si estás utilizando **WSL 2**, no necesitarás hacer ninguna modificación adicional, ya que **Docker Desktop** integrará automáticamente el sistema de archivos y las rutas de Linux.

#### Para usuarios sin **WSL 2**:
Si no estás utilizando **WSL 2**, será necesario ajustar las rutas de los volúmenes en el archivo `docker-compose.yml` para usar rutas absolutas en Windows. A continuación se muestra un ejemplo de cómo hacerlo:

```yaml
volumes:
  wordpress_data:
    driver_opts:
      type: none
      o: bind
      device: C:\path\to\wordpress_data  # Ajusta la ruta al directorio en tu máquina
  db_data:
    driver_opts:
      type: none
      o: bind
      device: C:\path\to\db_data  # Ajusta la ruta al directorio en tu máquina
```

Asegúrate de sustituir `C:\path\to\wordpress_data` y `C:\path\to\db_data` con las rutas correctas en tu máquina.

### 3. Levantar Docker y los contenedores

Una vez configurado el entorno, puedes levantar los contenedores de WordPress y MySQL ejecutando el siguiente comando en el directorio del proyecto:

```bash
docker-compose up -d
```

Este comando descargará las imágenes necesarias y creará los contenedores en segundo plano. La opción `-d` indica que se ejecutará en modo **desprendido** (background).

### 4. Acceder a WordPress

Una vez que los contenedores estén en ejecución, puedes acceder a tu instalación de WordPress desde tu navegador en la siguiente URL:

```
http://localhost:8080
```

Allí podrás configurar tu sitio de WordPress.

### 5. Detener y eliminar los contenedores

Cuando ya no necesites los contenedores, puedes detenerlos y eliminarlos con el siguiente comando:

```bash
docker-compose down
```

Si deseas eliminar también los volúmenes de datos (como las bases de datos), ejecuta:

```bash
docker-compose down -v
```

### Notas adicionales

- Si tienes problemas de **permisos** con los volúmenes en Windows, intenta ejecutar **Docker Desktop** como **administrador**.
- Puedes modificar las credenciales de la base de datos en el archivo `docker-compose.yml` según tus necesidades.
- Para instalar **plugins** o **temas** en WordPress, puedes acceder al contenedor de WordPress con el siguiente comando:

  ```bash
  docker exec -it wordpress bash
  ```

  Luego, navega al directorio `/var/www/html` para administrar los archivos manualmente.

---

¡Con esto, ya tendrás tu entorno de WordPress corriendo en Windows usando Docker!
