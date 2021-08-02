# Elaborando desde 0 un api en Django version 3.2 - En Linux Debian 10

## Parte 1 - Preparación de ambiente

### 1. Instalar última version de Python

Los repositorios predeterminados de Debian 10 vienen con Python 3.7. Instálela desde los repositorios predeterminados usando el comando sudo apt install python3. Para instalar Python a una version mayor a 3.7 siga estos pasos:

- Consultar la página de python: https://www.python.org/downloads/  , en nuestro caso la ultima version es 3.9.6
- Inicie sesión en el sistema Debian con acceso a la cuenta privilegiada sudo. Abra una terminal (CTRL + ALT + T) y ejecute los siguientes comandos para actualizar los paquetes:

      $ sudo apt update && sudo apt upgrade 

- Instale los paquetes necesarios para la compilación del código fuente de Python:

      $ sudo apt install wget build-essential libreadline-gplv2-dev libncursesw5-dev \
       libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
       
- Descargar Python, puede descargar el archivo fuente Python 3.9 desde su sitio oficial:

      $ wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz 

- Extraer archivo, después de la descarga, extraiga el archivo en su sistema:

      $ tar xzf Python-3.9.6.tgz 

- Compilación de la fuente de Python, cambie al directorio extraído con el comando cd, luego prepare el código fuente de Python para la compilación en su sistema:

      $  cd Python-3.9.6 
      $  ./configure --enable-optimizations 
      
- Instale Python 3.9.6, finalmente, ejecute el siguiente comando para completar la instalación de Python en el sistema Debian. Altinstall evita que el compilador anule las versiones predeterminadas de Python:

      $ sudo make altinstall 
      
- Chequear la versión de python:

      $ python3.9 -V 
      Python 3.9.6
      
- También instala Pip para python3.9:

      $ pip3.9 -V
      pip 21.1.3   ...
      
### 2. Instalación del entorno virtual pipenv

Pipenv es una herramienta que crea y administra un entorno virtual para sus proyectos y agrega o elimina paquetes a medida que los instala o desinstala. Pipenv nos proporciona un método sencillo para configurar un entorno de trabajo. Ya no es necesario utilizar pip y virtualenv por separado. Trabajan juntos.

- Dependiendo de la instalación , es posible que deba usar pip3.9 en lugar de pip:
      
      $ pip install pipenv
      
- Crear nuestro ambiente virtual con la última versión de python en un directorio donde iría nuestro proyecto. Vamos a llamar nuestro directorio (backend), donde internamente estará nuestro proyecto:

      /(backend)$  pipenv install –python 3.9.6
      
- Activar el directorio donde va el proyecto en virtualenv:

      /(backend)$ pipenv shell
      
### 3. Instalación Django, Creación del proyecto y de la aplicación  

- Instalar django:

      /(backend)$ pipenv install django 3
      
- Creación del proyecto llamado api:

       /(backend)$ pipenv run django-admin startproject api
      
- Luego ir al directorio api:

      $ cd api
      
- Crear aplicación llamada user:

      /(backend)/api$ pipenv run python manage.py startapp user
      
- Ejecutar la migracion (Nota: la migración se ejecutará en sqlite, si deseas ejecutarla en otra base de datos ir al directorio mas interno api/api/settings.py y configurar la base de datos):

      /(backend)/api$ pipenv run python manage.py migrate

- Levantar el servidor:

      /(backend)/api$ pipenv run python manage.py runserver
      
- Poner en el navegador la dirección:

      http://localhost:8000
      
Y aparecerá el Cohete de Bienvenida que todo ha sido un éxito!!!

En este punto, verá una instancia de una aplicación Django ejecutándose correctamente. puede detener el servidor (CTRL + C).

## Parte 2. Registro de la aplicación user.






#


