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
