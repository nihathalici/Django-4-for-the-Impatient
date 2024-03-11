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
