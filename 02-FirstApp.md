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