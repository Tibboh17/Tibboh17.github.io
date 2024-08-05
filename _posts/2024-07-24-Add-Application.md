---
title: How Can Tibboh Structure URLs and Applications?
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Project Structure and Configuration
- This post outlines the setup and configuration of the previous Django project witha specific URL structure and view functions.
- After following steps, we will have a Django project with a structured routing system, corresponding view functions, and organized templates for applications.

# URL Structure and View Functions

## Design Structure
- The project consists of two applications `main`, `project`.
- Below is a detailed mapping of URLs, view functions, and corresponding HTML files for each application.

| URL        | View Function  | HTML Filename | Notes |
|------------|----------------|---------------|-------|
| /          | index          | index.html    |       |
| about/     | about          | about.html    |       |
| contact/   | contact        | contact.html  |       |

| URL                 | View Function    | HTML Filename      | Notes                              |
|---------------------|------------------|--------------------|------------------------------------|
| project/            | project_list     | project_list.html  |                                    |
| project/<int:id>    | project_details  | project_detail.html| Redirects to 404 if post not found |

## Set Up the Application
- Use `startapp` to create a new Django application named `project`.
- Add the `project` application to `INSTALLED_APPS` in `tibboh/settings.py`
- Configure the template settings to include the project's templates directory.
- Configure static files settings.

```powershell
python manage.py startapp project
```

``` python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "main",
    "project"
]

TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [BASE_DIR / "templates"],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]

STATIC_URL = "static/"
STATICFILES_DIRS = [BASE_DIR / "static"]
```

# Configure URL Routing

## Reflect URLs
- Modify `urls.py` in each folder.

```python
# tibboh/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("main.urls")),
    path("project/", include("project.urls"))
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

```python
# project/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path("", views.project_list, name="project_list"),
    path("<int:pk>/", views.project_details, name="project_details")
]
```

# Define Views

## View Functions
- Create view functions for the main and project applications.

```python
from django.shortcuts import render

def index(request):
    return render(request, "main/index.html")

def about(request):
    return render(request, "main/about.html")

def contact(request):
    return render(request, "main/contact.html")
```

```python
from django.shortcuts import render

# Simulating a database with a list of dictionaries
project_database = [
    {
        "id": 1,
        "title": "Title1",
        "content": "Content1",
        "created_at": "2021-02-22",
        "updated_at": "2021-02-22",
        "author": "John Doe",
        "category": "Machine Learning",
        "tag": ["Tag1", "Tag2"],
        "view_count": 0,
        "thumbnail": "https://picsum.photos/200/300",
        "like_count": 3,
        "like_user": [10, 20, 21],
    },
    {
        "id": 2,
        "title": "Title2",
        "content": "Content2",
        "created_at": "2021-02-23",
        "updated_at": "2021-02-23",
        "author": "Jane Smith",
        "category": "Artificial Intelligence",
        "tag": ["Tag1", "Tag3"],
        "view_count": 0,
        "thumbnail": "https://picsum.photos/200/300",
        "like_count": 10,
        "like_user": [10, 20, 21, 22, 23, 24, 25, 26, 27, 28],
    },
    {
        "id": 3,
        "title": "Title3",
        "content": "Content3",
        "created_at": "2021-02-24",
        "updated_at": "2021-02-24",
        "author": "Alice Johnson",
        "category": "Data Science",
        "tag": ["Tag1", "Tag3"],
        "view_count": 0,
        "thumbnail": "https://picsum.photos/200/300",
        "like_count": 20,
        "like_user": [10, 20, 21, 22, 23, 24, 25, 26, 27, 28],
    },
    {
        "id": 4,
        "title": "Title4",
        "content": "Content4",
        "created_at": "2021-02-25",
        "updated_at": "2021-02-25",
        "author": "Bob Brown",
        "category": "Deep Learning",
        "tag": ["Tag1", "Tag3"],
        "view_count": 0,
        "thumbnail": "https://picsum.photos/200/300",
        "like_count": 30,
        "like_user": [10, 20, 21, 22, 23, 24, 25, 26, 27, 28],
    },
    {
        "id": 5,
        "title": "Title5",
        "content": "Content5",
        "created_at": "2021-02-26",
        "updated_at": "2021-02-26",
        "author": "Charlie Davis",
        "category": "Computer Vision",
        "tag": ["Tag2", "Tag4"],
        "view_count": 0,
        "thumbnail": "https://picsum.photos/200/300",
        "like_count": 5,
        "like_user": [10, 29, 30, 31, 32],
    }
]

def project_list(request):
    context = {"object_list": project_database}
    return render(request, "project/project_list.html", context)

def project_details(request, id):
    context = {"object": project_database[id - 1]}
    return render(request, "project/project_details.html", context)
```

# Create Templates

## Directory Structure

```
Top-Level Directory/
    ├── templates/
    │   ├── base/
    │   │   └── base.html
    │   ├── main/
    │   │   ├── index.html
    │   │   ├── about.html
    │   │   └── contact.html
    │   └── project/
    │       ├── project_list.html
    │       └── project_details.html
```

## Base Template

```html
{% raw %}
<!-- templates/base/base.html -->
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tibboh</title>
    <link rel="stylesheet" href="{% static 'css/custom.css' %}">
</head>
<body>
    <header>
        <h1>Tibboh</h1>
        <nav>
            <ul>
                <li><a href="{% url 'index' %}">Main</a></li>
                <li><a href="{% url 'about' %}">Introduction</a></li>
                <li><a href="{% url 'contact' %}">Contact</a></li>
                <li><a href="{% url 'project_list' %}">Project</a></li>
            </ul>
        </nav>
    </header>

    <main>
        {% block contents %}{% endblock %}
    </main>

    <script src="{% static 'js/custom.js' %}"></script>
</body>
</html>
{% endraw %}
```

## Main Templates

```html
{% raw %}
<!-- templates/main/index.html -->
{% extends 'base/base.html' %}
{% block contents %}
    <h1>Main Page</h1>
{% endblock %}
{% endraw %}
```

```html
{% raw %}
<!-- templates/main/about.html -->
{% extends 'base/base.html' %}
{% block contents %}
    <h1>Introduction Page</h1>
{% endblock %}
{% endraw %}
```

```html
{% raw %}
<!-- templates/main/contact.html -->
{% extends 'base/base.html' %}
{% block contents %}
    <h1>Contact Page</h1>
{% endblock %}
{% endraw %}
```

## Project Templates

```html
{% raw %}
<!-- templates/project/project_list.html -->
{% extends 'base/base.html' %}
{% block contents %}
    <h1>Project List</h1>
    <ul>
        {% for project in object_list %}
        <li>
            {{ forloop.counter }}
            <a href="{% url 'project_details' project.id %}">{{ project.title }}</a>
        </li>
        {% endfor %}
    </ul>
{% endblock %}
{% endraw %}
```

```html
{% raw %}
<!-- templates/project/project_details.html -->
{% extends 'base/base.html' %}
{% block contents %}
    <h1>{{ object.title }}</h1>
    <p>{{ object.content }}</p>
    <p>{{ object.created_at }}</p>
    <p>{{ object.updated_at }}</p>
    <a href="{% url 'project_list' %}">Content</a>
{% endblock %}
{% endraw %}
```

# Add Static Files

## Directory Structure
- Create a static directory and add CSS and JavaScript files.

```
Top-Level Directory/
    ├── static/
    │   ├── css/
    │   │   └── custom.css
    │   └── js/
    │       └── custom.js
```

## CSS

```css
/* static/css/custom.css */
body {
    font-family: Arial, sans-serif;
}

header {
    background-color: #333;
    color: white;
    padding: 10px 0;
}

header h1 {
    text-align: center;
}

nav ul {
    list-style: none;
    padding: 0;
    display: flex;
    justify-content: center;
}

nav ul li {
    margin: 0 15px;
}

nav ul li a {
    color: white;
    text-decoration: none;
}
```

## JS

```javascript
// static/js/custom.js
document.addEventListener('DOMContentLoaded', function () {
    console.log('Custom JS Loaded');
});
```