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
