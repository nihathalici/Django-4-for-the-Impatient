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

CH-10 / Checking if a username already exists
========================================================


* Update the views.py

```python
# accounts/views.py

from django.shortcuts import render
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User
from django.contrib.auth import login
from django.shortcuts import redirect
from django.db import IntegrityError

def signupaccount(request):
    if request.method == "GET":
        return render(request, "signupaccount.html", {"form": UserCreationForm})
    else:
        if request.POST["password1"] == request.POST["password2"]:
            try:
                user = User.objects.create_user(request.POST["username"], password=request.POST["password1"])
                user.save()
                login(request, user)
                return redirect("home")
            except IntegrityError:
                return render(request,
                              "signupaccount.html",
                              {"form":UserCreationForm,
                               "error": "Username already taken. Choose new username."})
        else:
            return render(request, "signupaccount.html", {"form": UserCreationForm, "error": "Passwords do not match"})

```


CH-10 / Customizing UserCreationForm
========================================================

* Create the forms.py file within the accounts folder
```python
# accounts/forms.py

from django.contrib.auth.forms import UserCreationForm

class UserCreateForm(UserCreationForm):
    def __init__(self, *args, **kwargs):
        super(UserCreateForm, self).__init__(*args, **kwargs)

        for fieldname in ["username", "password1", "password2"]:
            self.fields[fieldname].help_text = None
            self.fields[fieldname].widget.attrs.update({"class":"form-control"})

```

CH-10 / Using UserCreateForm
========================================================

* Update the views.py

```python

# accounts/views.py

from django.shortcuts import render
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User
from django.contrib.auth import login
from django.shortcuts import redirect
from django.db import IntegrityError

from .forms import UserCreateForm

def signupaccount(request):
    if request.method == "GET":
        return render(request, "signupaccount.html", {"form": UserCreateForm})
    else:
        if request.POST["password1"] == request.POST["password2"]:
            try:
                user = User.objects.create_user(request.POST["username"], password=request.POST["password1"])
                user.save()
                login(request, user)
                return redirect("home")
            except IntegrityError:
                return render(request,
                              "signupaccount.html",
                              {"form":UserCreateForm,
                               "error": "Username already taken. Choose new username."})
        else:
            return render(request, "signupaccount.html", {"form": UserCreateForm, "error": "Passwords do not match"})

```    

* Check http://127.0.0.1:8000/accounts/signupaccount/ 


CH-10 / Showing whether a user is logged in
========================================================

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
      
      {% if user.is_authenticated %}
        <a class="nav-link" href="#">Logout ({{ user.username }})</a>  
      {% else %}
        <a class="nav-link" href="#">Login</a>
        <a class="nav-link" href="#">Sign Up</a>
      {% endif %}     
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

CH-10 / Implementing the logout functionality
========================================================

* Update the urls.py file within the accounts folder

```python
# accounts/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path("signupaccount/", views.signupaccount, name="signupaccount"),
    path("logout/", views.logoutaccount, name="logoutaccount"),
]
```

* Update the views
```python

from django.contrib.auth import login, logout   # new

def logoutaccount(request):  # new
    logout(request)
    return redirect("home")

```

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
      
      {% if user.is_authenticated %}
        <a class="nav-link" href="{% url 'logoutaccount' %}">Logout ({{ user.username }})</a>  
      {% else %}
        <a class="nav-link" href="#">Login</a>
        <a class="nav-link" href="{% url 'signupaccount' %}">Sign Up</a>
      {% endif %}     
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

CH-10 / Implementing the login functionality
========================================================

* Update the urls.py file within the accounts folder

```python

# accounts/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path("signupaccount/", views.signupaccount, name="signupaccount"),
    path("logout/", views.logoutaccount, name="logoutaccount"),
    path("login/", views.loginaccount, name="loginaccount"),  # new
]
```

* Update the views
```python
# accounts/views.py

from django.shortcuts import render
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User
from django.contrib.auth.forms import AuthenticationForm  # new
from django.contrib.auth import login, logout, authenticate # new
from django.shortcuts import redirect
from django.db import IntegrityError

from .forms import UserCreateForm

def signupaccount(request):
    if request.method == "GET":
        return render(request, "signupaccount.html", {"form": UserCreateForm})
    else:
        if request.POST["password1"] == request.POST["password2"]:
            try:
                user = User.objects.create_user(request.POST["username"], password=request.POST["password1"])
                user.save()
                login(request, user)
                return redirect("home")
            except IntegrityError:
                return render(request,
                              "signupaccount.html",
                              {"form":UserCreateForm,
                               "error": "Username already taken. Choose new username."})
        else:
            return render(request, "signupaccount.html", {"form": UserCreateForm, "error": "Passwords do not match"})

    
def logoutaccount(request):
    logout(request)
    return redirect("home")

def loginaccount(request):  # new
    if request.method == "GET":
        return render(request, "loginaccount.html", {"form":AuthenticationForm})
    else:
        user = authenticate(request, username=request.POST["username"], 
                            password=request.POST["password"])
        if user is None:
            return render(request, "loginaccount.html",
                          {"form": AuthenticationForm(),
                           "error": "username and password do not match"})
        else:
            login(request, user)
            return redirect("home")
```

* Create the loginaccount.html file within the accounts/templates folder
```html
<!-- accounts/templates/loginaccount.html -->

{% extends 'base.html' %}

{% block content %}
  <div class="card mb-3">
    <div class="row g-0">
        <div>
            <div class="card-body">
                <h5 class="card-title">Login</h5>
                
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
                         Login
                       </button>
                    </form>
                </p>
            </div>
        </div>
    </div>
  </div>    
{% endblock content %}
    
```

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
      
      {% if user.is_authenticated %}
        <a class="nav-link" href="{% url 'logoutaccount' %}">Logout ({{ user.username }})</a>  
      {% else %}
        <a class="nav-link" href="{% url 'loginaccount' %}">Login</a>
        <a class="nav-link" href="{% url 'signupaccount' %}">Sign Up</a>
      {% endif %}     
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

* Check http://127.0.0.1:8000/accounts/login/  and http://127.0.0.1:8000/accounts/signupaccount/
