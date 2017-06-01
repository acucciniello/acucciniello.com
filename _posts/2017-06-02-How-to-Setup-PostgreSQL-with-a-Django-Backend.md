---
layout: post
title:  "How to Setup up PostgreSQL with a Django Backend"
date:   2017-06-02 12:20:10 
---


## Introduction

Welcome! I know it is out of the ordinary for me, but lately I have experimenting with using Django on the backend for some of my projects.  For a database, I have been sticking with PostgreSQL.  The purpose of this tutorial is to teach you how to get started using Django with PostgreSQL for your database work. 

If you have used Django before you know that it's default relational database is SQLite.  In order to use another relational database you must complete a couple of steps.  Today we will demonstrate this with PostgreSQL.

## Requirements

The things you need installed for this tutorial are:

1. [PostgreSQL][psqlInstall] (9.5)
2. Python (2.7.1)
3. [Django][djangoInstall] (1.11)

I am expecting you to have a basic Django backend set up.  If you do not, check out this [link][djangoTut].

Now that you made sure your environment is set up let's add PostgreSQL.

 
## Edit settings.py

In your `settings.py` file (located in the directory `myproject/myproject/settings.py`), you must go down to where you see a section for your DATABASE settings. Instead of `django.db.backends.postgresql` you should see  `django.db.backends.sqlite3`.  

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': DB_NAME,
        'USER': DB_USER,
        'PASSWORD': DB_PASS,
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

The code I have placed above is what you should change to the entire DATABASE section to.

Now you are probably wondering, what are DB_NAME, DB_USER, and DB_PASS? 

At the top of your `settings.py` file, add this:

```
DB_NAME = os.environ['DB_NAME']
DB_USER = os.environ['DB_USER']
DB_PASS = os.environ['DB_PASS']
```

We do not want to commit our private information about our database to our source control, so we will be using environment variables to access them.

## Create a Secrets File

As mentioned in the previous section, we want to store our private information about the database when saving things in source control.  

Create a file in your project directory called `secrets.sh`: 

```
$ touch secrets.sh
```

Then you want to add the following to it:
```
export DB_NAME='databaseName'
export DB_USER='dbUserName'
export DB_PASS='dbPassWord'
```

Now in order for your environment to actually contain these variables, you must run the script we just wrote:

```
$ . secrets.sh
```

Just like that your environment variables are set.

## Install psycopg2

The last step here, is simply installing the package `psycopg2`:

```
$ pip install psycopg2
```

This package is simply a PostgreSQL adapter for Python.

## Left Overs

For sake of brevity with this tutorial, this is all you need to do differently for setting up PostgreSQL. If you are first starting out, I want to suggest that you do the following things:

1. Actually create a database to use and set DB_NAME equal to the name
2. Create a Table with rows
3. Create a Model to represent your data and place it in your `models.py` file.
4. Proceed with the regular things you need to do when setting up a database with Django - `migrate` and `makemigrations`.

## Conclusion

Many developers prefer to use PostgreSQL for their database, so do not be alarmed when you see that it does not come packaged with Django. 

Great job today! I know it was short, but I hope you got some value out of this!

Reach out and follow me on [twitter][twitter]!  Check out my [GitHub][github] for my open source projects!


[github]: https://github.com/acucciniello
[twitter]: https://twitter.com/antocucciniello
[psqlInstall]: http://postgresguide.com/setup/install.html
[djangoInstall]: https://docs.djangoproject.com/en/1.11/howto/
[djangoTut]: https://docs.djangoproject.com/en/1.11/intro/tutorial01/
