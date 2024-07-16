---
title: Let Tibboh Use Django
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Start Django with Creating a Simple Website
- In this post, we will create a simple website using Django.
- The main steps involve setting up the project, creating an application, configuring URLs, writing views, and creating templates.
 
# Setting Up the Project
### Python Virtual Environment
- First, set up and activate a virtual environment.

```powershell
python -m venv venv
```

```powershell
.\venv\Scripts\activate
```

### Install Django and Start Project
- With the vitual environment activated, install Django and start a new project.
- The project name is `tibboh`.

```powershell
pip install django
```

```powershell
django-admin startproject tibboh
```

### Run Database Migrations and Start Development Server
- After the initial setup, run database migrations amd start the development server.
- The command `migrate` reflects all the code we have written into the database.
- Open your web browser and navigate to `http://127.0.0.1:8000/` to see the Django welcome page.

```powershell
python manage.py migrate
```

```powershell
python manage.py runserver
```

### Modify Project Settings
- Open the project setting file and configure allowed hosts.
- In `tibboh/settings.py`, modify `ALLOWED_HOSTS`.
```python
ALLOWED_HOSTS = ["*"]
```

# Creating an Application
### Create a New Application
- Use the `startapp` command for this step.
- We will call the application `main`.

```powershell
python manage.py startapp main
```

### Apply the Application
- In `tibboh\settings.py`, modify `INSTALLED_APP` to use the application `main`.

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "main"
]
```

# Cofiguring URLs
### Edit the Main URL File
- We will include the new views.
- Modify `tibboh/urls.py`.
- Import `index`, `about`, `tibboh` from `views.py`, and we will write them in the next step.

```python
from django.contrib import admin
from django.urls import path
from main.views import index, about, tibboh

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", index),
    path("about/", about),
    path("tibboh/", tibboh)
]
```

# Writing Views
### Create Views for Each URL
- As we import funtions from `views.py`, we must write codes for them.
- Edit `main/veiws.py`.

```python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
    return HttpResponse("<h1>Index</h1>")

def about(request):
    return HttpResponse("<h1>About</h1>")

def tibboh(request):
    return HttpResponse("<h1>Tibboh</h1>")
```

# Creating Templates
### Create Template Files for Each View
- We will make `index.html`, `about.html`, and `tibboh.html`.
- Create `main/templates/main/` directory and store these templates.
- These files will show us pages we intend.

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Index</title>
</head>
<body>
    <h1>Index</h1>
</body>
</html>
```

```html
<!-- about.html -->
<!DOCTYPE html>
<html>
<head>
    <title>About</title>
</head>
<body>
    <h1>About</h1>
</body>
</html>
```

```html
<!-- tibboh.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Tibboh</title>
</head>
<body>
    <h1>Tibboh</h1>
</body>
</html>
```

### Modify the Views
- We should edit `main/views.py` to render the corresponding templates.

```python
from django.shortcuts import render

def index(request):
    return render(request, 'main/index.html')

def about(request):
    return render(request, 'main/about.html')

def tibboh(request):
    return render(request, 'main/tibboh.html')
```

### Check the Development Server
- Restart the server and verify that each URL is working.
- We can see that from `http://127.0.0.1:8000/`, `http://127.0.0.1:8000/about/`,and `http://127.0.0.1:8000/tibboh/`.

```powershell
python manage.py runserver
```