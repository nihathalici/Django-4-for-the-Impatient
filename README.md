# Django 4 for the Impatient
This is my repo following the book "Django 4 for the Impatient" by Greg Lim and Daniel Correa.

Synopsis taken from the book: "This book enables you to use and learn Django in just a couple of days. In this book, you’ll go on a fun, hands-on, and pragmatic journey to learn Django full stack development."

Links and Appendix
========================================================

- Get the book: https://www.packtpub.com/product/django-4-for-the-impatient/9781803245836?_gl=1*n6k3za*_up*MQ..&gclid=Cj0KCQiA5-uuBhDzARIsAAa21T-hACrqESyBCs7S8ps3mMmwZWS6O9bZd5mS68SKYdO34MSNqIZ2aFYaAuS_EALw_wcB
- Official Repo: https://github.com/PacktPublishing/Django-4-for-the-Impatient

Set-Up
========================================================

* Make a new directory

```shell
mkdir moviereviewsproject
```

* Create a virtual environment within this new directory. 

```shell
python -m venv .venv
```

* Activate the virtual environment called .venv
```shell
source .venv/bin/activate
```

* Install Django
```shell
pip install django
```

* Create a new Django project called moviereviews 
```shell
django-admin startproject moviereviews
```

* Change the current directory
```shell
cd moviereviews
```

* Run the Django development server
```shell
python manage.py runserver
```

* Quit the server with CONTROL-C.

CH-2 / Creating The First App
========================================================


* Quit the server with CONTROL-C. Create the app pages
```shell
python manage.py startapp movies
```

* Edit the settings file and add the movies app 
```python3
# settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Local
    "movies.apps.MoviesConfig",  # new
]
```

CH-3 / Managing Django URLs
========================================================


* Quit the server with CONTROL-C. Update the urls.py file within the moviereviews folder
```python

# moviereviews/urls.py

from django.contrib import admin
from django.urls import path
from movies import views as moviesViews

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", moviesViews.home), 
]
```

* Update the views.py file within the movies folder
```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    return HttpResponse("<h1>Welcome to Home Page</h1>")

```

* Update the urls.py file within the moviereviews folder
```python

# moviereviews/urls.py

from django.contrib import admin
from django.urls import path
from movies import views as moviesViews

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", moviesViews.home),
    path("about/", moviesViews.about), 
]
```

* Update the views.py file within the movies folder again
```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    return HttpResponse("<h1>Welcome to Home Page</h1>")

def about(request):
    return HttpResponse("<h1>Welcome to About Page</h1>")

```

* Start the local server. Check http://127.0.0.1:8000/ and http://127.0.0.1:8000/about/


CH-4 / Generating HTML Pages with Templates
========================================================

* Within the movies folder create the templates folder. Create home.html within the movies/templates folder

```html
<!-- movies/home.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
</head>
<body>
<h1>Welcome to Home Page</h1>
<h2>This is the full home page</h2>
</body>
</html>
```

* Update the view

```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    return render(request, "home.html")

def about(request):
    return HttpResponse("<h1>Welcome to About Page</h1>")

```

* Go to https://getbootstrap.com/ and get bootstrap links

```html
<!-- movies/home.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    <h1>Welcome to Home Page, {{ name }}</h1>
    <h2>This is the full home page</h2>
  </div>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

* Add a search form to the home.html

```html
<!-- movies/home.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
  <div class="container">
   <form action="">
  <div class="mb-3">
  <label class="form-label">Search for Movie:</label>
  <input type="text" name="searchMovie" class="form-control">
  </div>
  <button type="submit" class="btn btn-primary">Search</button>
</form>
</div>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

* Update the views
```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    searchTerm = request.GET.get("searchMovie")
    return render(request, "home.html", {"searchTerm": searchTerm})

def about(request):
    return HttpResponse("<h1>Welcome to About Page</h1>")
```

* Add the searchTerm to the home.html
```html
<!-- movies/home.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
  <div class="container">
   <form action="">
  <div class="mb-3">
  <label class="form-label">Search for Movie:</label>
  <input type="text" name="searchMovie" class="form-control">
  </div>
  <button type="submit" class="btn btn-primary">Search</button>
</form>
Searching for {{ searchTerm }}
</div>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```


CH-4 / Sending a form to another page
========================================================

* Update the home.html
```html
<!-- movies/home.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
  <div class="container">
   <form action="">
  <div class="mb-3">
  <label class="form-label">Search for Movie:</label>
  <input type="text" name="searchMovie" class="form-control">
  </div>
  <button type="submit" class="btn btn-primary">Search</button>
</form>
Searching for {{ searchTerm }}
<br />
<br />
<h2>Join our mailing list:</h2>
<form action="{% url 'signup' %}">
  <div class="mb-3">
    <label for="email" class="form-label">
      Enter your email:
    </label>
    <input type="email" class="form-control" name="email">
  </div>
  <button type="submit" class="btn btn-primary">
Sign Up
  </button>

</form>

</div>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>

```

* Update the urls.py file within the moviereviews folder
```python
# moviereviews/urls.py

from django.contrib import admin
from django.urls import path
from movies import views as moviesViews

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", moviesViews.home),
    path("about/", moviesViews.about),
    path("signup/", moviesViews.signup, name="signup"), 
]
```

* Write the view
```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    searchTerm = request.GET.get("searchMovie")
    return render(request, "home.html", {"searchTerm": searchTerm})

def signup(request):
    email = request.GET.get("email")
    return render(request, "signup.html", {"email": email})

def about(request):
    return HttpResponse("<h1>Welcome to About Page</h1>")

```

* Create the signup.html file within the movies/templates folder
```html
<!-- movies/templates/signup.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
    <div class="container">
        <h2>Added {{ email }} to mailing list</h2>
    </div>
</body>
</html>
```


CH-4 / Creating a back link
========================================================

* Update the signup.html

```html
<!-- movies/templates/signup.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
    <div class="container">
        <h2>Added {{ email }} to mailing list</h2>
        <a href="{% url 'home' %}">Home</a>
    </div>
</body>
</html>
```

* Update the urls.py file within the moviereviews folder
```python
# moviereviews/urls.py

from django.contrib import admin
from django.urls import path
from movies import views as moviesViews

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", moviesViews.home, name="home"),
    path("about/", moviesViews.about, name="about"),
    path("signup/", moviesViews.signup, name="signup"), 
]
```
