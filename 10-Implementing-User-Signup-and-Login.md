CH-10 / Implementing User Signup and Login
========================================================

* Create the accounts app

```shell
python manage.py startapp accounts
```


* Update the settings
```python
# moviereviews/settings.py

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
    "accounts",
]

```

* Update the urls at the project level
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
    path("accounts/", include("accounts.urls")), 
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

```

* Create the urls.py file within the accounts folder
```python
# accounts/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path("signupaccount/", views.signupaccount, name="signupaccount"),
]

```

* Write the view
```python
# accounts/views.py

from django.shortcuts import render
from django.contrib.auth.forms import UserCreationForm

def signupaccount(request):
    return render(request, "signupaccount.html", {"form": UserCreationForm})


```

* Create the templates folder within the accounts directory
* Create the signupaccount.html file within the accounts/templates folder

```html
<!-- accounts/templates/signupaccount.html -->

{% extends 'base.html' %}

{% block content %}
  <div class="card mb-3">
    <div class="row g-0">
        <div>
            <div class="card-body">
                <h5 class="card-title">Sign Up</h5>
                <p class="card-text">
                    <form action="" method="post">
                       {% csrf_token %}
                       {{ form.as_p }}
                       <button type="submit" 
                         class="btn btn-primary">
                         Sign Up
                       </button>
                    </form>
                </p>
            </div>
        </div>
    </div>
  </div>    
{% endblock content %}
    
```

* Check http://127.0.0.1:8000/accounts/signupaccount/ 


CH-10 / Creating a user
========================================================

* Update the views.py

```python
# accounts/views.py

from django.shortcuts import render
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User
from django.contrib.auth import login
from django.shortcuts import redirect

def signupaccount(request):
    if request.method == "GET":
        return render(request, "signupaccount.html", {"form": UserCreationForm})
    else:
        if request.POST["password1"] == request.POST["password2"]:
            user = User.objects.create_user(request.POST["username"], password=request.POST["passwoord1"])
            user.save()
            login(request, user)
            return redirect("home")

```

* Check http://127.0.0.1:8000/accounts/signupaccount/ . Create a new user


CH-10 / Handling user creation errors
========================================================


* Update the views.py

```python
# accounts/views.py

# accounts/views.py

from django.shortcuts import render
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User
from django.contrib.auth import login
from django.shortcuts import redirect

def signupaccount(request):
    if request.method == "GET":
        return render(request, "signupaccount.html", {"form": UserCreationForm})
    else:
        if request.POST["password1"] == request.POST["password2"]:
            user = User.objects.create_user(request.POST["username"], password=request.POST["password1"])
            user.save()
            login(request, user)
            return redirect("home")
        else:  
            return render(request, "signupaccount.html", {"form": UserCreationForm, "error": "Passwords do not match"})
    
```

* Update the signupaccount.html file within the accounts/templates folder

```html
<!-- accounts/templates/signupaccount.html -->

{% extends 'base.html' %}

{% block content %}
  <div class="card mb-3">
    <div class="row g-0">
        <div>
            <div class="card-body">
                <h5 class="card-title">Sign Up</h5>
                
                {% if error %}
                <div class="alert alert-danger mt-3" role="alert">
                  {{ error }}
                </div>   
                {% endif %}
                <p class="card-text">
                    <form action="" method="post">
                       {% csrf_token %}
                       {{ form.as_p }}
                       <button type="submit" 
                         class="btn btn-primary">
                         Sign Up
                       </button>
                    </form>
                </p>
            </div>
        </div>
    </div>
  </div>    
{% endblock content %}

```
