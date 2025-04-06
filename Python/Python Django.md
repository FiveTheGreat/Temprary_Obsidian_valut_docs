#### Django Installation

```python
pip install Django
```

##### Virtual Environment
```python
python -m venv myworld
```

#### Project Creation
```python
django-admin startproject [my_tennis_club]
```

#### Django Structure
```python
my_tennis_club  
    manage.py  
    my_tennis_club/  
        __init__.py  
        asgi.py  
        settings.py  
        urls.py  
        wsgi.py
```

#### Django application run commend
```python
py manage.py runserver
```

#### Django App and Uses
		Django App use to Create a routes like nodejs routes It seperate the project depends upon the urls 
		like if the url name is about 
		[http:\\localhost:8000\about\]

#### Django app creation commend

```python
python manage.py startapp [appname]
```

#### Manage.py

	Manage.py use to control our Django project it contains the details of project 
	like Database connection type, host, models
		And It contains the Authentications

#### Working Principles
Django works **MVC** Pattern Model
	M -> Model
	 V -> View
	 C -> Controll

#### View
	Views hold the logic content of the our application

```python
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```

Here HttpResponse library gives the abliity of tranfering the data server to client through Request

#### URLs
	Urls use to locate the corresponded function when user enter the urls, Its envoked by Python manage.py URL pattern, bcaz it liked to the every App url.py to Main.py urls array section

`my_tennis_club/members/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```


`my_tennis_club/my_tennis_club/urls.py`:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```

#### Templates
Django Template use to send the html file as HTTP request. The act of the Templates in out project look like that

```
my_tennis_club  
    manage.py  
    my_tennis_club/  
    members/  
        templates/  
            myfirst.html
```

myfisrt.html file look like that,

```html
<!DOCTYPE html>
<html>
<body>

<h1>Hello World!</h1>
<p>Welcome to my first Django project!</p>

</body>
</html>
```

view.py file look like that,

```jsx
from django.http import HttpResponse
from django.template import loader

def members(request):
  template = loader.get_template('myfirst.html')
  return HttpResponse(template.render())
```

Add the new app into Installed App array of Manage.py file

```jsx
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'members'
]
```

![[Pasted image 20240729153228.png]]

#### Models

A Djanga model is a table in your database.

#### Django Models
---
Up until now in this tutorial, output has been static data from Python or HTML templates.

Now we will see how Django allows us to work with data, without having to change or upload files in the prosess.

In Django, data is created in objects, called Models, and is actually tables in a database.

---

#### Create Model

`my_tennis_club/members/models.py`:

```jsx
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
```

The first field, firstname holds the first name of members
lastname holds the last name of members

Django project contains the own database named as SQLite
and It is default database of Django too.

#### Migration

Now when we have described a Model in the `models.py` file, we must run a command to actually create the table in the database.

Navigate to the `/my_tennis_club/` folder and run this command:

```python
py manage.py makemigrations members
```

if Successfully migrates the database is shows,
```cmd
Migrations for 'members':  
  members\migrations\0001_initial.py  
    - Create model Member  
  
(myworld) C:\Users\_Your Name_\myworld\my_tennis_club>

```

It Creates the new folder too, named as migrations, This folders contains the stored data files of out Django project



