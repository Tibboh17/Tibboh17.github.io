---
title: How Tibboh Start the First Django Project
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Overview
- In this post, we will walk through the process of setting up a basic Django project.
- This guide is designed by codes I have used for my first Django project.
- By end of this post, you will have a Django web application with a simple sturucture.

# Setting Up a Virtual Environment
### Creating the Virtual Environment
- First, set up a virtual environment to manage the project dependecies.

```sh
python -m venv venv
```

### Activating the Virtual Environment
- Activate the created virtual environment.
- The command differs based on your operating system.

```sh
# Windows
.\venv\Scripts\activate
```

```sh
# macOS/Linux
source venv/bin/activate
```

# Installing Django and Starting the Project

### Installing Django
- With the virtual environment activated, install Django using pip.

```sh
pip install django
```

### Starting a Django Project
- Create a new Django project in the current directory.
- In this post, the project's name is `tibbohlog`.

```sh
django-admin startproject tibbohlog .
```

# Creating and Setting Up the App

### Creating a Django App
- Within the Django project, create and app named `main`.

```sh
django-admin startapp main
```

### Configuring URLs
- Modify the project's URL configuration file and create a URL configuration file for the app to handle routing.
- Update `tibbohlog/urls.py` to include the `main` app URLs.
- Create `main/urls.py` and define the URL patterns.

```python
# tibbohlog/urls.py
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
    path("contact/", views.contact, name="contact"),
]
```

# Setting Up Views

### Defining View Functions
- Modify `main/views.py` to define the view functions for eash URL.

```python
from django.shortcuts import render

def index(request):
    return render(request, "main/index.html")

def about(request):
    return render(request, "main/about.html")

def contact(request):
    return render(request, "main/contact.html")
```

# Creating Templates

### Creating HTML Templates
- Create a directory for templates within the `main` directory and add HTML files.

```html
<!-- main/templates/main/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Index</title>
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
    <title>About</title>
</head>
<body>
    <h1>About</h1>
</body>
</html>
```

```html
<!-- main/templates/main/contact.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contact</title>
</head>
<body>
    <h1>Contact</h1>
</body>
</html>
```

# Running Migrations and the Development Server

### Running Database Migrations
- Apply the Django model changes to the database by running migrations.

```sh
python manage.py migrate
```

### Starting the Development Server
- Run the development server to view the project locally

```sh
python manage.py runserver
```