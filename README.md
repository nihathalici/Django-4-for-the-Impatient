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


CH-5 / Working with Models 
========================================================

* Update the models.py within the movies folder 

```python

# movies/models.py

from django.db import models

class Movie(models.Model):
    title = models.CharField(max_length=100)
    description = models.CharField(max_length=250)
    image = models.ImageField(upload_to="movies/images/")
    url = models.URLField(blank=True)

```

* Install pillow
```shell
pip install pillow
```

* Generate the migrations
```shell
python manage.py migrate
```

* Create the migrations for the movies app.
```shell
python manage.py makemigrations movies
python manage.py migrate
```

* Create the superuser
```shell
python manage.py createsuperuser
```

CH-5 / Configuring for images
========================================================

* Update the settings
```python
# moviereviews/settings.py
import os

...

MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'

```

* Update the urls.py file within the moviereviews.py file
```python
# moviereviews/urls.py

from django.contrib import admin
from django.urls import path
from movies import views as moviesViews
from django.conf.urls.static import static
from django.conf import settings

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", moviesViews.home, name="home"),
    path("about/", moviesViews.about, name="about"),
    path("signup/", moviesViews.signup, name="signup"), 
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

* Register the model
```python
# movies/admin.py

from django.contrib import admin
from .models import Movie

admin.site.register(Movie)

```

* Check http://127.0.0.1:8000/admin/

* Make new entries for movies 


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



CH-6 / Displaying Objects from Admin
========================================================

* Update the views

```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse
a
from .models import Movie

def home(request):
    searchTerm = request.GET.get("searchMovie")
    movies = Movie.objects.all()
    return render(request, "home.html", {"searchTerm": searchTerm, 'movies': movies})

def signup(request):
    email = request.GET.get("email")
    return render(request, "signup.html", {"email": email})

def about(request):
    return HttpResponse("<h1>Welcome to About Page</h1>")

```

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
<p>Searching for: {{ searchTerm }}</p>

{% for movie in movies %}
  <h2>{{ movie.title }}</h2>
  <h3>{{ movie.description }}</h3>
  <img src="{{ movie.image.url }}">
  
  {% if movie.url %}
    <a href="{{ movie.url }}">Movie Link</a>   
  {% endif %}  
{% endfor %}
  
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

* Check http://127.0.0.1:8000/


CH-6 / Using the card component
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
<p>Searching for: {{ searchTerm }}</p>
<div class="row row-cols-1 row-cols-md-3 g-4">
{% for movie in movies %}
<div v-for="movie in movies" class="col">
  <div class="card">
    <img class="card-img-top" src="{{ movie.image.url }}">
    <div class="card-body">
      <h5 class="card-title fw-bold">{{ movie.title }}</h5>
      <p class="card-text">{{ movie.description }}</p>  
  {% if movie.url %}
    <a href="{{ movie.url }}" class="btn btn-primary">Movie Link</a>   
  {% endif %}
  </div>
</div>
</div>  
{% endfor %}
</div>  
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

* Update the home function in the views.py
```python
def home(request):
    searchTerm = request.GET.get("searchMovie")
    if searchTerm:
        movies = Movie.objects.filter(title__icontains=searchTerm)
    else:
        movies = Movie.objects.all()
    return render(request, "home.html", {"searchTerm": searchTerm, 'movies': movies})
```

CH-6 / Adding a news app
========================================================

* Add the new app to the project
```shell
python manage.py startapp news
```

* Update the settings
```python
# moviereviews/settings.py

...

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Local
    "movies",
    "news",
]

...

```

* Update the urls.py file within the moviereviews folder

```python
# moviereviews/urls.py

from django.contrib import admin
from django.urls import path, include
from movies import views as moviesViews
from django.conf.urls.static import static
from django.conf import settings

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", moviesViews.home, name="home"),
    path("about/", moviesViews.about, name="about"),
    path("signup/", moviesViews.signup, name="signup"),
    path("news/", include("news.urls")), 
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

```

* Within the news folder create urls.py
```python
# news/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path("", views.news, name="news"),
]
```

* Update the views.py file within the news folder
```python
# news/views.py

from django.shortcuts import render

def news(request):
    return render(request, "news.html")
```

CH-6 / News model
========================================================

* Update the models.py file within the news folder
```python
# news/models.py

from django.db import models

class News(models.Model):
    headline = models.CharField(max_length=200)
    body = models.TextField()
    date = models.DateField()
```

* Update the database
```shell
python manage.py makemigrations news
python manage.py migrate
```

* Register the new model

```python
# news/admin.py

from django.contrib import admin

from .models import News

admin.site.register(News)
```

* Check http://127.0.0.1:8000/admin/news/news/  Add some news


CH-6 / Listing news
========================================================

* Update the views.py file within the news folder

```python
# news/views.py

from django.shortcuts import render
from .models import News

def news(request):
    newss = News.objects.all()
    return render(request, "news.html", {"newss": newss})
```


* Within the news folder create the templates folder
* Within the templates folder create the news.html
```html
<!-- news/templates/news.html -->
<!DOCTYPE html>
<html>
  <head>
    <title>Movies App</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  </head>
  <body>
    <div class="container">
        
        {% for news in newss %}
        <h2>{{ news.headline }}</h2>
        <h5>{{ news.date }}</h5>
        <p>{{ news.body }}</p>    
        {% endfor %}        
    </div>
  </body>    
</html>
```

* Update the views.py file within the news folder
```python
# news/views.py

from django.shortcuts import render
from .models import News

def news(request):
    newss = News.objects.all().order_by('-date')
    return render(request, "news.html", {"newss": newss})
``` 

* Update the news.html and add the bootstrap/card look
```html
<!-- news/templates/news.html -->
<!DOCTYPE html>
<html>
  <head>
    <title>Movies App</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  </head>
  <body>
    <div class="container">
        
        {% for news in newss %}
          <div class="card mb-3">
            <div class="row g-0">
                <div>
                    <div class="card-body">
                        <h5 class="card-title">{{ news.headline }}</h5>
                        <p class="card-text">{{ news.body }}</p>
                        <p class="card-text"><small class="text-muted">{{ news.date }}</small></p>
                    </div>
                </div>
            </div>
          </div>
        {% endfor %}       
    </div>
  </body>    
</html>
```

* Check http://127.0.0.1:8000/news/
