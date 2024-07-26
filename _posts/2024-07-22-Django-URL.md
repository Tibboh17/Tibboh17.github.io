---
title: Tibboh Wants to Make Several Pages
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Overview
- In this post, we will set up URL structures, create views, and render templates using the previous step.
- By the end of this post, we can understand how to organize URLs and display dynamic content using Django’s template system.

# URLs Configuration and Description

### URLs and Description
- Before writing codes directly, we should design the structures about URL.

| URL         | Description                                    |
|-------------|------------------------------------------------|
| /admin      | Admin                                          |
| /           | Home                                           |
| /blog       | blog list (URL to view the list of blog posts) |
| /blog/1     | blog details (URL for project post details)    |
| /blog/2     | blog details (URL for project post details)    |

### Define URL Patterns
- Define the URL patterns for the Django project.
- Modify `main/urls.py`.

```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),
    path("about/", views.about, name="about"),
    path("contact/", views.contact, name="contact"),
    path("blog/", views.blog, name="blog"),
    path("blog/<int:id>", views.blog_details, name="blog_details"),
]
```

# Define View Functions

### Set Up Sample Data
- Use sample data to demonstate how Django views handle and display data.
- Instead of using a database, we will use a list as an example.

```python
blog_list_db = [
    {
    "id": 1,
    "title": "Let Tibboh Use Django", 
    "content": "This is the content of blog 1",
    "author": "Author 1"
    },
    {
    "id": 2,
    "title": "How can I become well on Python", 
    "content": "This is the content of blog 2",
    "author": "Author 2"
    },
]
```

### Reflect Views
- In `main/views.py`, define the views that correspond to our URLs using the sample data.

```python
from django.shortcuts import render

blog_list_db = [
    {
    "id": 1,
    "title": "Let Tibboh Use Django", 
    "content": "This is the content of blog 1",
    "author": "Author 1"
    },
    {
    "id": 2,
    "title": "How can I become well on Python", 
    "content": "This is the content of blog 2",
    "author": "Author 2"
    },
]

def index(request):
    return render(request, "main/index.html")

def about(request):
    return render(request, "main/about.html")

def contact(request):
    return render(request, "main/contact.html")

def blog_list(request):
    context = {"object_list": blog_list_db}
    return render(request, "main/blog_list.html", context)

def blog_details(request, id):
    blog = blog_list_db[id - 1]
    context = {"object": blog}
    return render(request, "main/blog_details.html", context)
```

# Create Templates

### Blog List Template
- Create `blog_list.html` in `main/templates/main` for displaying the blog list.

```html
{% raw %}
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
        {% for blog in object_list %}
        <li><a href="/blog/{{blog.id}}">{{ blog.title }}</a></li>
        {% endfor %}
    </ul>
</body>
</html>
{% endraw %}
```

### Blog Details Template
- Create `blog_details.html` in `main/templates/main` for displaying the details of a blog post.

```html
{% raw %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blog Details</title>
</head>
<body>
    <h1>Blog Detail</h1>
    <h2>{{ object.title }}</h2>
    <p>{{ object.author }}</p>
    <p>{{ object.content }}</p>
</body>
</html>
{% endraw %}
```