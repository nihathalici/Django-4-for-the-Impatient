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
