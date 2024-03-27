CH-11 / Letting Users Create, Read, Update, and Delete Movie Reviews
========================================================

* Update the models.py within the movies folder and add the Review model

```python

# movies/models.py


from django.contrib.auth.models import User  # new

...

class Review(models.Model):
    text = models.CharField(max_length=100)
    date = models.DateTimeField(auto_now_add=True)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    movie = models.ForeignKey(Movie, on_delete=models.CASCADE)
    watchAgain = models.BooleanField()

    def __str__(self):
        return self.text

```

* Register the new model

```python

# movies/admin.py

from django.contrib import admin
from .models import Movie, Review  # new

admin.site.register(Movie)
admin.site.register(Review)  # new

```

* Make the migrations
```shell
python manage.py makemigrations
python manage.py migrate

```

CH-11 / Creating a review
========================================================

* Update the urls.py within the movies folder

```python
# movies/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path("<int:movie_id>", views.detail, name="detail"),
    path("<int:movie_id>/create", views.createreview, name="createreview"),
]
```

* Update the views and add the new function createreview
```python

# movies/views.py

from django.shortcuts import render
from django.http import HttpResponse
from django.shortcuts import get_object_or_404, redirect  # new

from .models import Movie, Review  # new
from .forms import ReviewForm  # new


def createreview(request, movie_id):  # new
    movie = get_object_or_404(Movie, pk=movie_id)
    if request.method == "GET":
        return render(request, "createreview.html", {"form": ReviewForm(), 
                                                     "movie": movie})
    else:
        try:
            form = ReviewForm(request.POST)
            newReview = form.save(commit=False)
            newReview.user = request.user
            newReview.movie = movie
            newReview.save()
            return redirect("detail",
                            newReview.movie.id)
        except ValueError:
            return render(request,
                          "createreview.html",
                          {"form": ReviewForm(), 
                           "error": "Bad data passed in"})

```

* Create the createreview.html file within the movies/templates folder

```html
<!-- movies/templates/createreview.html -->

{% extends 'base.html' %}

{% block content %}
  <div class="card mb-3">
    <div class="row g-0">
        <div>
            <div class="card-body">
                <h5 class="card-title">Addd Review for {{ movie.title }}</h5>
                
                {% if error %}
                  <div class="alert alert-danger mt-3" role="alert">{{ error }}</div>    
                {% endif %}
                 <p class="card-text">
                    <form action="" method="post">
                        {% csrf_token %}
                        {{ form.as_p }}
                        <button type="submit" class="btn btn-primary">Add Review</button>
                    </form>
                 </p>   
            </div>
        </div>
    </div>
</div>    
{% endblock content %}    
```

* Create the forms.py within the movies folder

```python
# movies/forms.py

from django.forms import ModelForm, Textarea
from .models import Review

class ReviewForm(ModelForm):
    def __init__(self, *args, **kwargs):
        super(ModelForm, self).__init__(*args, **kwargs)
        self.fields["text"].widget.attrs.update({"class": "form-control"})
        self.fields["watchAgain"].widget.attrs.update({"class": "form-check-input"})
    
    class Meta:
        model = Review
        fields = ["text", "watchAgain"]
        labels = {"watchAgain": ("Watch Again")}
        widgets = {"text": Textarea(attrs={"rows": 4})}
```

* Update the detail.html within the movies/templates folder

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
              
              {% if user.is_authenticated %}
              <a href="{% url 'createreview' movie.id %}" class="btn btn-primary">Add Review</a>     
              {% endif %}       
            </p>
        </div>
    </div>
  </div>
</div>    
{% endblock content %}    
```

* Check http://127.0.0.1:8000/ Login, and try to add a review



CH-11 / Listing reviews
========================================================
