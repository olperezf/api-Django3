# Elaborando desde 0 un api en Django version 3.2 - En Linux Debian 10

## Parte 1 - Preparando el entorno

### 1. Instalar última version de Python

Los repositorios predeterminados de Debian 10 vienen con Python 3.7. Instálela desde los repositorios predeterminados usando el comando sudo apt install python3. Para instalar Python a una version mayor a 3.7 siga estos pasos:

- Consultar la página de python: https://www.python.org/downloads/  , en nuestro caso la ultima version es 3.9.6
- Inicie sesión en el sistema Debian con acceso a la cuenta privilegiada sudo. Abra una terminal (CTRL + ALT + T) y ejecute los siguientes comandos para actualizar los paquetes:

      $ sudo apt update && sudo apt upgrade 

- Instale los paquetes necesarios para la compilación del código fuente de Python:

      $ sudo apt install wget build-essential libreadline-gplv2-dev libncursesw5-dev \
       libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
       
- Descargar el archivo fuente Python 3.9 desde su sitio oficial:

      $ wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz 

- Extraer archivo, después de la descarga:

      $ tar xzf Python-3.9.6.tgz 

- Compilar Python, cambie al directorio extraído con el comando cd, luego prepare el código fuente de Python para la compilación en su sistema:

      $  cd Python-3.9.6 
      $  ./configure --enable-optimizations 
      
- Instale Python 3.9.6, finalmente, ejecute el siguiente comando para completar la instalación. Altinstall evita que el compilador anule las versiones predeterminadas de Python:

      $ sudo make altinstall 
      
- Chequear la versión de python:

      $ python3.9 -V 
      Python 3.9.6
      
- También instala Pip(sistema de gestión de paquetes utilizado para instalar y administrar paquetes de software escritos en Python) para python3.9:

      $ pip3.9 -V
      pip 21.1.3   ...
      
### 2. Instalación del entorno virtual pipenv

Pipenv es una herramienta que crea y administra un entorno virtual para sus proyectos y agrega o elimina paquetes a medida que los instala o desinstala. Pipenv nos proporciona un método sencillo para configurar un entorno de trabajo. Ya no es necesario utilizar pip y virtualenv por separado. Trabajan juntos.

- Dependiendo de la instalación , es posible que deba usar pip3.9 en lugar de pip:
      
      $ pip install pipenv
      
- Crear nuestro ambiente virtual con la última versión de python en un directorio donde iría nuestro proyecto. Vamos a llamar nuestro directorio (backend), donde internamente estará nuestro proyecto:

      /backend$  pipenv install –python 3.9.6
      
- Activar el directorio donde va el proyecto en virtualenv:

      /backend$ pipenv shell
      
### 3. Instalación Django, Creación del proyecto y de la aplicación  

- Instalar django:

      (entorno activado: backend-_zwhdufhi)/backend$ pipenv install django 3
      
- Creación del proyecto llamado api:

       backend-_zwhdufhi/backend$ django-admin startproject api
      
- Luego ir al directorio api:

      $ cd api
      
- Crear aplicación llamada user:

      backend-_zwhdufhi/backend/api$ python manage.py startapp user
      
- Ejecutar la migracion (Nota: la migración se ejecutará en sqlite, si deseas ejecutarla en otra base de datos ir al directorio mas interno api/api/settings.py y configurar la base de datos):

      backend-_zwhdufhi/backend/api$ python manage.py migrate

- Levantar el servidor:

      backend-_zwhdufhi/backend/api$ python manage.py runserver
      
- Poner en el navegador la dirección:

      http://localhost:8000
      
Y aparecerá el Cohete de Bienvenida que todo ha sido un éxito!!!

En este punto, verá una instancia de una aplicación Django ejecutándose correctamente. puede detener el servidor (CTRL + C).

## Parte 2. Configuración de la aplicación user.

Ahora que ha completado la configuración del entorno, comenzamos a registrar la aplicación user para que Django pueda reconocerla.

1. Abra el archivo api / settings.py en su editor de código y agregue user a INSTALLED_APPS:

            INSTALLED_APPS = [
                'django.contrib.admin',
                'django.contrib.auth',
                'django.contrib.contenttypes',
                'django.contrib.sessions',
                'django.contrib.messages',
                'django.contrib.staticfiles',
                'user',
            ]

Luego salvar los cambios.

Creemos un modelo para definir cómo se deben almacenar los elementos de user en la base de datos.

2. Abra el archivo user / models.py y agregue las siguientes líneas de código:

            class User(models.Model):
                   name = models.CharField("Name", max_length=240)
                   email = models.EmailField()
                   phone = models.CharField(max_length=20)
                   active = models.BooleanField(default=True)
                   created_at = models.DateTimeField(auto_now_add=True)
                   updated_at = models.DateTimeField(auto_now=True)

                   def __str__(self):
                       return self.name 
                       
3. Crear un archivo de migración:

                  $ python manage.py makemigrations user
                  
4. Aplicar los cambios a la base de datos:

                  $ python manage.py migrate user
                  
Puede probar para ver que las operaciones CRUD funcionan en el modelo User, utilizando la interfaz de administración que Django proporciona de forma predeterminada.
                  
5. Abra el archivo user / admin.py y agregue las siguientes líneas de código y luego salvar los cambios:

                  from django.contrib import admin
                  from .models import User

                  class UserAdmin(admin.ModelAdmin):
                      list_display = ('name', 'email', 'phone','active')

                  # Register your models here.

                  admin.site.register(User, UserAdmin)



6. Deberá crear una cuenta de "superusuario" para acceder a la interfaz de administración. Ejecute el siguiente comando:

                  $ python manage.py createsuperuser
                  
Se le pedirá que ingrese un nombre de usuario, correo electrónico y contraseña.

7. Levantar el servidor otra vez:

                  $ python manage.py runserver
                  
Ir a http://localhost:8000/admin en su navegador web. E inicie sesión con el nombre de usuario y la contraseña que se creó anteriormente.
Puede crear, editar y eliminar elementos de User usando el panel de administrador. 
Luego para el servidor control + c.

## Parte 3. Configuración del API



#


