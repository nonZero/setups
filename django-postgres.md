# Use Postgres as your Database
## Prerequisites
- [Install postgres](./postgres-setup.md).
- Make sure your user is a postgres superuser, and you can access postgres from the command line by running `psql`.
- Install `psycopg2`, the package used by Django to use postgres.  In your virtualenv run:

        pipenv install psycopg2

## Choosing Simple Configuration Options
Assuming our project name is `myproject`, we will choose the following defaults:

- The postgres server will be running on the same machine as the django project.
- A postgres database called `myproject`
- A postgres user called `myproject`
- The user's password would be `myproject`

In your settings file, modify `DATABASES` to:

            DATABASES = {
                'default': {
                    'ENGINE': 'django.db.backends.postgresql',
                    'NAME': 'myproject',
                    'USER': 'myproject',
                    'PASSWORD': 'myproject',
                    'HOST': '127.0.0.1',
                    'PORT': '5432',
                }
            }


## Create A Postgres User And Database
- Assuming you have `django-extensions` installed, use the following command to generate commands to create a user and a db for your project:

           python manage.py sqlcreate

  This will **output** something simlar to:

        CREATE USER myproject WITH ENCRYPTED PASSWORD 'myproject' CREATEDB;
        CREATE DATABASE myproject WITH ENCODING 'UTF-8' OWNER "myproject";
        GRANT ALL PRIVILEGES ON DATABASE myproject TO myproject;

- To execute the code above and create your DB, use `psql` (assuming your user is a postgres superuser):

           python manage.py sqlcreate | psql

## Migrate!

- To create the tables, indexes and relationships in your DB, run:

           python manage.py migrate



## Notes
* This setup does not fit more complicated setups, where the database is running on a different host.  It also assumes the access to your postgres is restricted and  knowledge of the user/password would not help an attacker to access your db.

* See also <https://github.com/kennethreitz/dj-database-url> for easily configuring DB access from environment varibales.


