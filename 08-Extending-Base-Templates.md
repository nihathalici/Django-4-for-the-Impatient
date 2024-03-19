CH-8 / Extending Base Templates
========================================================

* Update the home.html within the movies/templates folder

```html
<!-- movies/home.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
<meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light mb-3">
  <div class="container">
    <a class="navbar-brand" href="#">Movies</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" 
    data-bs-target="#navbarNavAltMarkup"
    aria-controls="navbarNavAltMarkup"
    aria-expanded="false"
    aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
    <div class="navbar-nav ms-auto">
      <a class="nav-link" href="#">News</a>
      <a class="nav-link" href="#">Login</a>
      <a class="nav-link" href="#">Sign Up</a>
    </div>
  </div>
</div>
</nav>
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

* Create the templates folder within the main directory moviereviews. 
* Create the base.html file within the moviereviews/templates folder.

```html
<!-- moviereviews/templates/base.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
<meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light mb-3">
  <div class="container">
    <a class="navbar-brand" href="#">Movies</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" 
    data-bs-target="#navbarNavAltMarkup"
    aria-controls="navbarNavAltMarkup"
    aria-expanded="false"
    aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
    <div class="navbar-nav ms-auto">
      <a class="nav-link" href="#">News</a>
      <a class="nav-link" href="#">Login</a>
      <a class="nav-link" href="#">Sign Up</a>
    </div>
  </div>
</div>
</nav>

<div class="container">

{% block content %}
    
{% endblock content %}
    
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

* Update the settings
```python
# moviereviews/settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'moviereviews/templates')],
        'APP_DIRS': True,
        ...
    } 
]
```

* Update the home.html
```html
<!-- movies/home.html -->

{% extends 'base.html' %}

{% block content %}
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
{% endblock content %}
```

* Update the news.html within the news/template folder
```html
<!-- news/templates/news.html -->
{% extends 'base.html' %}

{% block content %}    
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
{% endblock content %}

``` 

CH-8 / Making the links work
========================================================

* Update the base.html
```html 
<!-- moviereviews/templates/base.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
<meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light mb-3">
  <div class="container">
    <a class="navbar-brand" href="{% url 'home' %}">Movies</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" 
    data-bs-target="#navbarNavAltMarkup"
    aria-controls="navbarNavAltMarkup"
    aria-expanded="false"
    aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
    <div class="navbar-nav ms-auto">
      <a class="nav-link" href="{% url 'news' %}">News</a>
      <a class="nav-link" href="#">Login</a>
      <a class="nav-link" href="#">Sign Up</a>
    </div>
  </div>
</div>
</nav>

<div class="container">

{% block content %}
    
{% endblock content %}
    
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
``` 

CH-8 / Adding a footer section
========================================================

* Update the base.html
```html 
<!-- moviereviews/templates/base.html -->

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
<meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light mb-3">
  <div class="container">
    <a class="navbar-brand" href="{% url 'home' %}">Movies</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" 
    data-bs-target="#navbarNavAltMarkup"
    aria-controls="navbarNavAltMarkup"
    aria-expanded="false"
    aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
    <div class="navbar-nav ms-auto">
      <a class="nav-link" href="{% url 'news' %}">News</a>
      <a class="nav-link" href="#">Login</a>
      <a class="nav-link" href="#">Sign Up</a>
    </div>
  </div>
</div>
</nav>

<div class="container">

{% block content %}
    
{% endblock content %}
</div>

<footer class="text-center text-lg-start bg-light text-muted mt-4">
  <div class="text-center p-4">
    © Copyright - 
    <a class="text-reset fw-bold text-decoration-none" href="#" target="_blank">Movie Reviews</a>  
  </div>
</footer>


<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
``` 

CH-8 / Serving static files
========================================================

* Create new folder named static within the moviereviews directory. Within the folder static another folder named images

* Save the image file movie.png within the static/images folder

* Update the base.html
```html
<!-- moviereviews/templates/base.html -->
{% load static %}

<!DOCTYPE html>
<html>
<head>
<title>Movies App</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
<meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light mb-3">
  <div class="container">
    <a class="navbar-brand" href="{% url 'home' %}">
    <img src="{% static 'images/movie.png' %}" alt="" width="30" height="24"
    class="d-inline-block align-text-top" />Movies</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" 
    data-bs-target="#navbarNavAltMarkup"
    aria-controls="navbarNavAltMarkup"
    aria-expanded="false"
    aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
    <div class="navbar-nav ms-auto">
      <a class="nav-link" href="{% url 'news' %}">News</a>
      <a class="nav-link" href="#">Login</a>
      <a class="nav-link" href="#">Sign Up</a>
    </div>
  </div>
</div>
</nav>

<div class="container">

{% block content %}
    
{% endblock content %}
</div>

<footer class="text-center text-lg-start bg-light text-muted mt-4">
  <div class="text-center p-4">
    © Copyright - 
    <a class="text-reset fw-bold text-decoration-none" href="#" target="_blank">Movie Reviews</a>  
  </div>
</footer>


<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

* Update the settings. Add this new line to the end of the file
```python
# moviereviews/settings.py

...
STATICFILES_DIRS = [BASE_DIR / "moviereviews/static/"]
...

```