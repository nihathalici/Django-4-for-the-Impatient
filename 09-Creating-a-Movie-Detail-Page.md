CH-9 / Creating a Movie Detail Page
========================================================

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
    path("movies/", include("movies.urls")), 
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```


* Create another urls.py within the movies folder
```python
# movies/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path("<int:movie_id>", views.detail, name="detail"),
]
```

* Update the views
```python
# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse
from django.shortcuts import get_object_or_404

from .models import Movie

def home(request):
    searchTerm = request.GET.get("searchMovie")
    if searchTerm:
        movies = Movie.objects.filter(title__icontains=searchTerm)
    else:
        movies = Movie.objects.all()
    return render(request, "home.html", {"searchTerm": searchTerm, 'movies': movies})

def signup(request):
    email = request.GET.get("email")
    return render(request, "signup.html", {"email": email})

def about(request):
    return HttpResponse("<h1>Welcome to About Page</h1>")

def detail(request, movie_id):  # new
    movie = get_object_or_404(Movie, pk=movie_id)
    return render(request, "detail.html", {"movie": movie})
```

* Create the detail.html file within the movies/templates folder 
```html
<!-- movies/templates/detail.html -->

{% extends 'base.html' %}

{% block content %}
<div class="card mb-3">
  <div class="row g-0">
    <div class="col-md-4">
        <img src="{{ movie.image.url }}" alt=""
          class="img-fluid rounded-start">
    </div>
    <div class="col-md-8">
        <div class="card-body">
            <h5 class="card-title">{{ movie.title }}</h5>
            <p class="card-text">{{ movie.description }}</p>
            <p class="card-text">
              {% if movie.url %}
                <a href="{{ movie.url }}"
                  class="btn btn-primary">
                Movie Link</a>
              {% endif %}     
            </p>
        </div>
    </div>
  </div>
</div>    
{% endblock content %}   
```

* Check http://127.0.0.1:8000/movies/1


CH-9 / Implementing links to individual movie pages
========================================================

* Update home.html
```html
<!-- movies/templates/home.html -->

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
      <a href="{% url 'detail' movie.id %}">
      <h5 class="card-title fw-bold">{{ movie.title }}</h5>
    </a>
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

* Check http://127.0.0.1:8000/
