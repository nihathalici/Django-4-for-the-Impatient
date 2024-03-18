CH-7 / Understanding-the-Database
========================================================

* Update the models.py within the news folder

```python
# news/models.py

from django.db import models

class News(models.Model):
    headline = models.CharField(max_length=200)
    body = models.TextField()
    date = models.DateField()

    def __str__(self):
        return self.headline

```

CH-7 / Switching to a MySQL database
========================================================

* If you don't have XAMPP installed, go to https://www.apachefriends.org/download.html, download it, and install it.

* Start MySql and Apache Web Server

* Create a new database named moviereviews


CH-7 / Configuring our project to use the MySQL database
========================================================

* Install the pymysql package

```shell
pip install pymysql
```

* Update the __init__.py within the moviereviews folder
```python
import pymysql
pymysql.install_as_MySQLdb()
```

* Run the migrations
```shell
python manage.py migrate
```

* **DON'T WORK FOR ME!**
