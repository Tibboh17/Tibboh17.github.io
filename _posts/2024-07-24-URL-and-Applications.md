---
title: Structure URL and Applications
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Overview
- Set up a specific URL structure and view functions.
- Use models to handle database tables.

# Set Up for the Project

## Create a New Application
- By the following command, create an application and name it `post`.

```shell
django-admin startapp post
```

## Update Setting
- Modify `TEMPLATES` in `blog/setting.py` to include and use templates in `main/templates` directory.
- Add `post` to `INSTALLED_APPS`, just like `main`.

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "main",
    "post",
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
```

## Create Templates
- First, create `post` folder in `main/templates`, then generate `post_list.html` and `post_details.html` inside the folder as follows.
- These templates are only for posts that will be uploaded on the blog.

```html
<!-- main/templates/post/post_list.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blog List</title>
</head>
<body>
    <h1>The page for Blog List</h1>
    <ul>
        {% raw %}
        {% for blog in object_list %}
        <li><a href="/post/{{ object.id }}">{{ object.title }}</a></li>
        {% endfor %}
        {% endraw %}
    </ul>
</body>
</html>
```

```html
<!-- main/templates/post/post_details.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Post Details</title>
</head>
<body>
    <h1>Post Details</h1>
    <h2>{{ object.title }}</h2>
    <p>{{ object.content }}</p>
    <p>{{ object.created_at }}</p>
    <p>{{ object.updated_at }}</p>
</body>
</html>
```

# Define the Model

## Create the Blog Post Model

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

## Register the Model in the Admin Interface
- Register `Post` model with the admin interface to enable CRUD operations.

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

# Reflect All the Changes

## Define URL
- Create `urls.py` in `post` directory.
- Define `urlpatterns` in `post/urls.py`.
- Modify `blog/urls.py` to include the URL patterns defined in `post/urls.py`.

```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.post_list, name="post_list"),
    path("<int:id>/", views.post_details, name="post_details"),
]
```

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("main.urls")),
    path("post/", include("post.urls")),
]
```

## Define Views for Post
- Define `post_list` and `post_details` functions in `post/views.py`

```python
from django.shortcuts import render
from .models import Post

def post_list(request):
    object_list = Post.objects.all()
    context = {"object_list": object_list}
    return render(request, "post/post_list", context)

def post_details(request, id):
    object = Post.objects.get(id=id)
    context = {"object": object}
    return render(request, "post/post_details", context)
```

# Run the Server

## Data Migration
- Create and apply the migrations for the project

```shell
python manage.py makemigrations
```

```shell
python manage.py migrate
```

## Superuser
- Create a superuser to access the admin interface.
- Follow the prompts in the shell to set up a superuser account.

```shell
python manage.py createsuperuser
```

```shell
Username (leave blank to use 'User'): Tibboh
Email address: tibboh@abc.com
Password: 
Password (again):
Superuser created successfully.
```

## Developement Server
- Start the Django development server by using the following command.
- Posts should be written in the admin interface before accessing to the page, as the view function does not handle the case of missing data.
- If there are any posts, navigate to ‘http://127.0.0.1:8000/post’ to see the list of posts.

```
python manage.py runserver
```