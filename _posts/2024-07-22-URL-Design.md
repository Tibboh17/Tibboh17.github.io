---
title: Tibboh Wants to Make Several Pages
categories: [Python, Django]
tag: [Python, Django, Web, Back-End]
---

# Exploring URL Structures and Templates
- In this post, we will set up URL structures, create views, and render templates using the previous step.
- By the end of this post, we can understand how to organize URLs and display dynamic content using Django's template system.

# Setting up URL Structures
### Design URLs
- Before writing codes directly, we should design the structures about URLs.
- This is updated table with the **/tibboh** and **/about** pages removed. 

| URL              | Description                                      |
|------------------|--------------------------------------------------|
| /admin           | Admin                                            |
| /                | Home                                             |
| /project         | Project list (URL to view the list of blog posts)|
| /project/1       | Project details (URL for project post details)   |
| /project/2       | Project details (URL for project post details)   |
| /accounts/moon   | User details (URL for user details)              |
| /accounts/tibboh | User details (URL for user details)              |

### Map URLs
- Reflect URLs in `tibboh/urls.py`.

```python
from django.contrib import admin
from django.urls import path
from main.views import index, project_list, project_details, account_details

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", index),
    path("project/", project_list),
    path("project/<int:pk>", project_details),
    path("account/<str:username>", account_details)
]
```

# Creating Views
### Sample Data for Views
- Use sample data to demonstate how Django views handle and display data.

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
    }
]

user_list_db = [
    {
    "id": 1,
    "username": "moon", 
    "email": "hojun@gmail.com", 
    "password": "1234"
    },
    {
    "id": 2,
    "username": "tibboh", 
    "email": "jihun@gmail.com", 
    "password": "1234"
    }
]
```

### Reflect Views
- In `main/views.py`, define the views that correspond to our URLs using the sample data.
- Use `HttpResponse` for testing.

```python
from django.shortcuts import render
from django.shortcuts import HttpResponse

project_list_db = [
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
    }
]

user_list_db = [
    {
    "id": 1, 
    "username": "moon", 
    "email": "moon@abc.com", 
    "password": "1234"
    },
    {
    "id": 2, 
    "username": "tibboh", 
    "email": "tibboh@def.com", 
    "password": "1234"
    }
]

def index(request):
    return render(request, "main/index.html")

def project_list(request):
    project_list_html = ""
    for project in project_list_db:
        project_list_html += f'<li><a href="/project/{project["id"]}">{project["title"]}</a></li>'
    return HttpResponse(f"""
    <h1>Project List</h1>
    <ul>
        {project_list_html}
    </ul>
    """)

def project_details(request, pk):
    project = project_list_db[pk - 1]
    return HttpResponse(f"""
    <h1>{project["title"]}</h1>
    <p>{project["content"]}</p>
    <p>{project["author"]}</p>
""")

def account_details(request, username):
    find_user = None
    for user in user_list_db:
        if user["username"] == username:
            find_user = user
            break
    if find_user is None:
        return HttpResponse("User Not Found")
    return HttpResponse(f"""
    <h1>{find_user["username"]}</h1>
    <p>{find_user["email"]}</p>
""")
```

# Rendering Templates
### Create Templates for Views
- Create `project_list.html`, `project_details.html`, `account_details.html` in `main/templates/main`.
- We will use a Django template code to run python codes in html files.

```html
{% raw %}
<!-- project_list.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project List</title>
</head>
<body>
    <h1>The page for Project List</h1>
    <ul>
        {% for project in project_list %}
        <li><a href="/project/{{project.id}}">{{project.title}}</a></li>
        {% endfor %}
    </ul>
</body>
</html>
{% endraw %}
```

```html
{% raw %}
<!-- project_details.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project Details</title>
</head>
<body>
    <h1>Project Detail</h1>
    <h2>{{project.title}}</h2>
    <p>{{project.author}}</p>
    <p>{{project.content}}</p>
</body>
</html>
{% endraw %}
```

```html
{% raw %}
<!-- account_details.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Account Deatils</title>
</head>
<body>
    <h1>The page for Account Details</h1>
    <p>{{user.username}}</p>
    <p>{{user.email}}</p>
</body>
</html>
{% endraw %}
```

### Modify the Views
- Edit `main/views.py` for templates.

```python
from django.shortcuts import render
from django.shortcuts import HttpResponse

project_list_db = [
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
    }
]

user_list_db = [
    {
    "id": 1, 
    "username": "moon", 
    "email": "moon@abc.com", 
    "password": "1234"
    },
    {
    "id": 2, 
    "username": "tibboh", 
    "email": "tibboh@def.com", 
    "password": "1234"
    }
]

def index(request):
    return render(request, "main/index.html")

def project_list(request):
    context = {"project_list": project_list_db}
    return render(request, "main/project_list.html", context)

def project_details(request, pk):
    project = project_list_db[pk - 1]
    context = {"project": project}
    return render(request, "main/project_details.html", context)

def account_details(request, username):
    find_user = None
    for user in user_list_db:
        if user["username"] == username:
            find_user = user
            break
    if find_user is None:
        return HttpResponse("User Not Found")
    context = {"user": find_user}
    return render(request, "main/account_details.html", context)
```
