# Proyecto Integrador II 
Curso de "Python para el análisis de datos" en UDEMY por Erick Hernández

Adaptado de: https://github.com/lucrae/django-cheat-sheet/ 

# 📋 Django checklist 
- 🐍 Inicializar un nuevo ambiente de trabajo
- 💼 Crear proyecto (`Project`)
- 📂 Crear nueva aplicación (`App`)
- 📄 Crear nueva vista (`View`)
- 🎨 Crear nueva plantilla (`Template`)
<s>
- 📭 Crear nuevo modelo (`Model`)
- 📬 Crearo objetos del modelo
- 🪪 Usar la página de administrador
</s>
- Agregar: 🌷 Modificar vista IRIS


# 🐍 Inicializar un nuevo ambiente de trabajo
Nota: Abrir terminal en VSC con CMD en lugar de PowerShell
- Navegar a la carpeta del proyecto `$ cd <folder>`
- Crear un nuevo ambiente de trabajo `$ python -m venv <venv-name>` (\<venv-name>=venv)
- Crear nuevo archivo ".env" `$ type NUL > .env` (Windows) `$ touch .env` (Linux)
- Activar ambiente `$ .\<venv-name>\Scripts\activate`
- Instalar librerías necesarias `$ pip install -r requirements.txt`

# 💼 Crear proyecto 
- Crear proyecto `$ django-admin startproject <project-name>` (\<venv-name>=project)
- Navegar a la carpeta del proyecto `$ cd <project-name>`
<s>
- Crear nueva clave secreta para el proyecto. Iniciar python con `$ python`. Importar secrets y generar un nuevo `secret-key`

 ```python
import secrets
secrets.token_hex()
```
- Copiar el valor y crear variable en archivo .env `SECRET_KEY='<secret-key>'` 
- Abrir el archivo de settings (`/<project-name>/<project-name>/settings.py`)

- Agregar al inicio de `settings.py`:

```python
import os
from dotenv import load_dotenv
load_dotenv() # lee variables de archivo .env
```

- En el archivo `settings.py` reemplazar valor de variable `SECRET_KEY` por:
```python
SECRET_KEY = os.getenv('SECRET_KEY') # Leer variable de .env
```
</s>

# 📂 Crear nueva aplicación (`App`)
- Crear nueva aplicación `$ python manage.py startapp <app-name>` (\<app-name>=portfolio_app revisa que estés dentro de la carpeta `<project-name>`)
- Agregar nueva app a `settings.py`:
```python
INSTALLED_APPS = [
    ... ,
    '<app-name>',
]
```
- Revisar que funcione la aplicación:
- `$ python manage.py makemigrations`
- `$ python manage.py migrate`
- `$ python manage.py runserver`

- Abrir aplicación desde el servidor en `http://localhost:8000/` (si se corre localmente) o en el respectivo servidor si se está usando computo en la nube
- Cerrar aplicación con `ctrl + c`


# 📄 Crear nueva vista (`View`)

- Ir a la carpeta de la aplicación (`<project-name>/<app-name>`)
- Abrir `views.py`
- Agregar:
```python
from django.shortcuts import render
from django.http import HttpResponse

# Vista "Index"
def index(request):
    template = "<h1> Página principal </h1>"
    return HttpResponse(template)

```
- Agregar vista a lista de urls `urls.py` en `<app-name>` (crear archivo `urls.py` si no existe)

```
project/
    portfolio_app/
        migrations/
        __init__.py
        admin.py
        apps.py
        models.py
        tests.py
        urls.py         <--- CREAR/MODIFICAR ESTE ARCHIVO
        views.py
    project/
```
En `urls.py` en `<app-name>`:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

- Agregar las urls de la aplicación a las urls del proyecto `urls.py` en `<project-name>` 

```
project/
    portfolio_app/
        ...
    project/
        __init__.py
        asgi.py
        settings.py
        urls.py         <--- MODIFICAR ESTE ARCHIVO
        wsgi.py
```
En `urls.py` en `<project-name>`:
```python
from django.contrib import admin
from django.urls import path, include # agregar include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include('portfolio_app.urls')), # Agregar urls de app
]
```
- Correr el servidor para verificar que la app esté funcionando. `$ python manage.py runserver`. Abrir app en servidor `http://localhost:8000/` (si se corre localmente) o en el respectivo servidor si se está usando computo en la nube
- Cerrar aplicación con `ctrl + c`


# 🎨 Crear nueva plantilla (`Template`)
- Crea carpeta `static/` dentro de la carpeta contenedora del proyecto `.`
- Crear carpetas `css/`, `js/` e `img/` dentro de `static/`
- Copiar archivos de imágenes `*.png` y `*.ico` en carpeta `static/images/`
- Copiar modelo de aprendizaje automático `classification_svc_sklearn_1_2_1.pickle` en carpeta `static/`
- Copiar archivos `*.css` en carpeta `css/`
- Copiar archivos `*.js` en carpeta `js/`

```
project/                        <---- Dentro de esta carpeta contenedora
    portfolio_app/
        ...
    project/
        ...
    static/                     <----
        css/                    <----
            style.css           <----
        js/                     <----
            script.js           <----
        img/                    <----
            favicon.ico         <----
            iris.png            <----
            iris-setosa.png     <----
            iris-virginica.png  <----
            iris-versicolor.png <----
            iris-circular.png   <----
            placeholder.png     <----
            modern-art.jpg      <----
    db.sqlite3
    manage.py
```

- Definir carpeta de archivos estáticos al final de `<project-name>/settings.py`:

```python
STATIC_URL = 'static/'
STATICFILES_DIRS = [BASE_DIR / 'static'] 
```

- Mover archivos `index.html` e `iris.html` a `<projec>/<app-name>/templates/<app-name>` (crear carpetas)

```
project/
    portfolio_app/
        templates/          <---- Crear
            portfolio_app/  <---- Crear
                index.html  <---- Mover aquí
                iris.html   <---- Mover aquí
        migrations/
        __init__.py
        admin.py
        apps.py
        models.py
        tests.py
        urls.py 
        views.py
    project/
```

- Modificar `<app-name>/views.py`

```python
from django.shortcuts import render

# Vista Index
def index(request):
    template = 'portfolio_app/index.html'
    return render(request, template)
```


# 🌷 Modificar vista IRIS

- Agregar el siguiente código a `<app-name>/views.py`

```python
from django.shortcuts import render
import pickle
import numpy as np


# Cargar modelo guardado
filename = 'static/classification_svc_sklearn_1_2_1.pickle'
with open(filename, 'rb') as f:
    loaded_model = pickle.load(f)


# Vista Index
def index(request):
    template = 'portfolio_app/index.html'
    return render(request, template)

# Vista Iris
def iris(request):

    pred_label = ""
    pred_image_name = ""

    if request.method == "POST":

        slength = float(request.POST.get('slength'))
        swidth = float(request.POST.get('swidth'))
        plength = float(request.POST.get('plength'))
        pwidth = float(request.POST.get('pwidth'))

        lista_iris = [slength, swidth, plength, pwidth]
        x_new = np.array([lista_iris])
        y_pred = int(loaded_model.predict(x_new))

        dictionary = {0:('Iris Setosa', 'iris-setosa.png'), 
                      1:('Iris Versicolor', 'iris-versicolor.png'), 
                      2:('Iris Virginica', 'iris-virginica.png')}
        
        pred_label = dictionary[y_pred][0]
        pred_image_name = dictionary[y_pred][1]    

    template = 'portfolio_app/iris.html'
    context = {'pred_label': pred_label, 
               'pred_image_name': pred_image_name}
    
    return render(request, template, context)
```

- Definir URL para vista `iris`. En `<app-name>\urls.py` agregar:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('iris/', views.iris, name='iris'), # <---- 
]

```

- Agregar en `<project-name>/settings.py`:

```python
ALLOWED_HOSTS = ['*']
CSRF_TRUSTED_ORIGINS = ['*']  # Aquí agregas el dominio ej.  ....preview.app.github.dev
```



- Correr el servidor para verificar que la app esté funcionando. `$ python manage.py runserver`. Abrir app en servidor `http://localhost:8000/` (si se corre localmente) o en el respectivo servidor si se está usando computo en la nube
- Navegar a `http://localhost:8000/iris` o dar click en la página web al tab portafolio y seleccionar el proyecto iris.
- Copiar la dirección para pegarla en `CSRF_TRUSTED_ORIGINS`
- Cerrar aplicación en terminal con `ctrl + c`

- Modificar en `<project-name>/settings.py`:

```python
CSRF_TRUSTED_ORIGINS = ['https://...']  # Aquí agregas la dirección que te aparece a ti
```
- Vuelve a correr el servidor `$ python manage.py runserver`