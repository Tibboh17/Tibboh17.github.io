---
title: Start the Django Project for My Game Schedule
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Overview
- Start the Django project by making a scheduler application.
- Implement the basic app structure and views in this post.

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

# Start the Project

## Generate the Project Files
- By using the command as follows, generate the project.
- The name of the project can be anything, but in this post, it will be `scheduler`.

```shell
django-admin startproject scheduler
```

## Configure the Application
- We can create an application using the following command.
- Name the application `todo_app`.

```shell
cd scheduler
django-admin startapp todo_app
```

## Modify Setting
- To reflect the application we made before, we should update `scheduler/settings.py`
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
    "todo_app",
]
```

# Set Up the Application

## Configure Model
- The model defines the structure of the database, and determines how data is stored and managed.
- In this post, we will define `List` and `Task` in `todo_app/models.py` 

```python
from django.db import models

class List(models.Model):
    title = models.CharField(max_length=100)
    description = models.TextField(blank=True)

    def __str__(self):
        return self.title
    
class Task(models.Model):
    title = models.CharField(max_length=100)
    description = models.TextField(blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    due = models.DateTimeField(null=True, blank=True)
    done = models.BooleanField(default=False)
    task_list = models.ForeignKey(List, on_delete=models.CASCADE, related_name="tasks")

    def __str__(self):
        return self.title
```

# Data Migration and Server

## Running Database Migrations
- Before run the server, we should run migrations.

```shell
python manage.py makemigrations
python manage.py migrate
```

## Development Server
- Run the development server to see the project locally.

```shell
python manage.py runserver
```