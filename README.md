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
- [OPCIONAL] 🚀 Distribución (deployment)
- [OPCIONAL] 🏡 Agregar vista CASA


# 🐍 Inicializar un nuevo ambiente de trabajo
Nota: Abrir terminal en VSC con CMD en lugar de PowerShell
<s>
- Navegar a la carpeta del proyecto `$ cd <folder>`
</s>
- Crear un nuevo ambiente de trabajo `$ python -m venv <venv-name>` (\<venv-name>=venv)
<s>
- Crear nuevo archivo ".env" `$ type NUL > .env` (Windows) `$ touch .env` (Linux)
</s>
- Activar ambiente `$ .\<venv-name>\Scripts\activate` (Windows) `source <venv-name>/bin/activate` (Linux) 
- Mover el archivo `requirements.txt` afuera de la carpeta `temp` 
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
- Copiar archivos de imágenes `*.png` y `*.ico` en carpeta `static/img/`
- Copiar archivos `*.pickle` (como los modelos de aprendizaje automático) en carpeta `static/`
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

- Mover archivos `*.html` a `<projec>/<app-name>/templates/<app-name>` (crear carpetas)

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
filename = 'static/classification_svc_sklearn_1_2_2.pickle'
with open(filename, 'rb') as f:
    loaded_model_classification = pickle.load(f)


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
        y_pred = int(loaded_model_classification.predict(x_new))

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


# 🚀 [OPCIONAL] Distribución (deployment)

- Asegurarse que las librerías de `gunicorn` y `whitenoise` estén presentes en el archivo de `requirements.txt`
- Mover el archivo de `requirements.txt` a la carpeta del proyecto
- Crear archivo `Procfile` (sin extensión) dentro de la carpeta del proyecto
- Crear archivo `runtime.txt` dentro de la carpeta del proyecto

```
project/
    portfolio_app/
    project/
    static/
    ...
    Procfile            <---- Crear
    requirements.txt
    runtime.txt         <---- Crear
```

- Agregar `web: gunicorn <project-name>.wsgi` al archivo `Procfile`:

```
web: gunicorn 'project.wsgi' 
```

- Agregar `Python -3.10.4` al archivo `runtime.txt`:

```
Python -3.10.4 
```

- Agregar middleware de librería `whitenoise` y definir `STATICFILES_STORAGE` en `<project-name>/settings.py` (después de `MIDDLEWARE`):

```python
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "whitenoise.middleware.WhiteNoiseMiddleware",  # <---- Aquí
    ...
]
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage' # <--- Aquí
```

- Definir `STATIC_ROOT` en `<project-name>/settings.py`:

```python
# Primero importar librería os al inicio del archivo settings.py
import os                   # <---- Aquí
from pathlib import Path
```

```python
STATIC_URL = 'static/'
STATICFILES_DIRS = [BASE_DIR / 'static'] 
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')  # <--- Aquí
```
- Recolectar archivos estáticos en `STATIC_ROOT` con:
- `$ python manage.py collectstatic`

- - Modificar en `<project-name>/settings.py`: (primero se tiene que implementar en Railway para que te genere un dominio)

```python
CSRF_TRUSTED_ORIGINS = ['https://...']  # Aquí agregas el dominio de Railway, ejemplo: https://mywebsite-production-a8ce.up.railway.app/
```

# 🏡 [OPCIONAL] Crear vista CASAS 

1.- Copiar `iris.html` y renombrarlo como `casa.html`

2.- En `casa.html`, reemplazar la sección de contenido y modificar el formulario para contar con los campos: `['OverallQual', 'GrLivArea', '1stFlrSF', 'FullBath', 'YearBuilt']`

```html

<!-- Contenido -->
<div class="container">

    <!-- Nueva fila -->
    <div class="row">

        <!-- Columna izquierda vacía -->
        <div class="col-3"></div>

        <!-- Columna central -->
        <div class="col-6 mt-5">
            <div class="row">
                <br>
                <!-- Formulario -->
                <form method="post">
                    {% csrf_token %}

                    <!-- Campo 1: Calidad general del 1 al 10 (OverallQual) -->
                    <div class="form-floating mb-3">
                        <input type="number" name="OverallQual" class="form-control" id="OverallQual" step="1" placeholder="8" value="" min="1" max="10">
                        <label for="OverallQual">Calidad general (1 al 10) (ej. 8)</label>  
                    </div>

                    <!-- Campo 2: Superficie habitable (GrLivArea) -->
                    <div class="form-floating mb-3">
                        <input type="number" name="GrLivArea" class="form-control" id="GrLivArea" step="1" placeholder="1660" value="">
                        <label for="GrLivArea">Superficie habitable (ej. 1660)</label> 
                    </div>

                    <!-- Campo 3: Área en pies cuadrados del 1er piso (1stFlrSF) -->
                    <div class="form-floating mb-3">
                        <input type="number" name="1stFlrSF" class="form-control" id="1stFlrSF" step="1" placeholder="1500" value="">
                        <label for="1stFlrSF">Área 1er piso en pies^3 (ej. 1500)</label> 
                    </div>

                    <!-- Campo 5: Número de baños (FullBath) -->
                    <div class="form-floating mb-3">
                        <input type="number" name="FullBath" class="form-control" id="FullBath" step="1" placeholder="2" value="">
                        <label for="FullBath">Número de baños (ej. 2)</label> 
                    </div>

                    <!-- Campo 5: Año de construcción (YearBuilt) -->
                    <div class="form-floating mb-3">
                        <input type="number" name="YearBuilt" class="form-control" id="YearBuilt" step="1" placeholder="2006" value="">
                        <label for="YearBuilt">Año de construcción (ej. 2006)</label> 
                    </div>

                    <!-- Botón submit -->
                    <button type="submit" class="btn btn-info text-center w-100">Estimar</button>
                </form>
            </div>
        </div>

        <!-- Columna derecha vacía -->
        <div class="col-3"></div>

    </div>

    <br>

    <!-- Nueva fila -->
    <!-- Etiqueta con la predicción (solo aparece si existe la etiqueta) -->
    {% if pred_label %}
    <div class="row text-center mt-5">
        <div class="col-3"></div>
        <div class="col-6">
            <div class="alert alert-info" role="alert">
                <h4>{{ pred_label }}</h4>
            </div>
        </div>
        <div class="col-3"></div>
    </div>
    {% endif %}

</div>

```

- Agregar vista "casa" a `<app-name>/views.py` (no olvidar cargar el modelo `static/regression_rf_sklearn_1_2_2.pickle`)


```python
from django.shortcuts import render
import pickle
import numpy as np


# Cargar modelo guardado
...

filename = 'static/regression_rf_sklearn_1_2_2.pickle'
with open(filename, 'rb') as f:
    loaded_model_regression = pickle.load(f)

# Vista Index
def index(request):
    ...

# Vista Iris
def iris(request):
    ...

# Vista Casa
def casa(request):

    pred_label = None

    if request.method == "POST":

        overall_quality = int(request.POST.get('OverallQual'))
        above_ground_living_area = int(request.POST.get('GrLivArea'))
        first_floor_sf = int(request.POST.get('1stFlrSF'))
        full_bath = int(request.POST.get('FullBath'))
        year_built = int(request.POST.get('YearBuilt'))

        lista_casa = [overall_quality, above_ground_living_area, first_floor_sf, full_bath, year_built]
        x_new = np.array([lista_casa])
        y_pred = float(loaded_model_regression.predict(x_new))   

    template = 'portfolio_app/casa.html'
    context = {'pred_label': y_pred}
    
    return render(request, template, context)
```

- Definir URL para vista `casa`. En `<app-name>\urls.py` agregar:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('iris/', views.iris, name='iris'),
    path('casa/', views.casa, name='casa'), # <----  
]

```
