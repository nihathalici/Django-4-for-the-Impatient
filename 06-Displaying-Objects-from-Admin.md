CH-6 / Displaying Objects from Admin
========================================================

* Update the views

```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse

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