---
title: Start the First Django Project
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Overview
- Start the Django project by making our own site.
- Make a simple web application using Django.

# Virtual Environment Setting

## Create `venv`
- Set up the virtual environment for development and name it `venv`.

```shell
python -m venv venv
```

## Start `venv`
- Activate the virtual environment named `venv`.

```shell
.\venv\Scripts\activate
```

# Create the First Application

## Configure the Application
- We can create an application using the following command.
- Name the application `main`.

```shell
django-admin startapp main
```

## Modify Setting
- To reflect the application we made before, we should update `main/settings.py`
- Modify `ALLOWED_HOSTS` and `INSTALLED_APPS` as follows.

```python
ALLOWED_HOSTS = ["*"]

INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "main",
]
```

# Set Up the Application

## Configure URL
- Modify `blog/urls.py` and create `main/urls.py` as follows for mapping the URL.
- We will modify views used in `main/urls.py` in the next step.

```python
# blog/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("main.urls")),
]

```

```python
# main/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),
    path("about/", views.about, name="about"),
]
```

## Modify Views
- Update `main/views.py` to define view functions for each URL as follows.

```python
from django.shortcuts import render

def index(request):
    return render(request, "main/index.html")

def about(request):
    return render(request, "main/about.html")
```

## Create Templates
- Templates help us to show pages we intend.
- Create `templates` folder in `main` directory.
- Create `main` folder in `templates` and and generate `index.html` and `about.html` inside it as follows.

```html
<!-- main/templates/main/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Index Page</title>
</head>
<body>
    <h1>Index</h1>
</body>
</html>
```

```html
<!-- main/templates/main/about.html -->
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About Page</title>
</head>
<body>
    <h1>About</h1>
</body>
</html>
```

# Data Migration and Server

## Running Database Migrations
- Before run the server, we should run migrations.

```shell
python manage.py migrate
```

## Development Server
- Run the development server to see the project locally.

```shell
python manage.py runserver
```