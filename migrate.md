## Create a Database

To create the database and the database user we will use `django-extensions`' `sqlcreate` command:

    @task
    def create_db():
        with virtualenv():
            run("./manage.py sqlcreate | psql", pty=False)


run:

    fab create_db

and then:

    fab m:migrate

## Create a Django Superuser (Optional)

If you would like, you can create a superuser for your project:

    fab m:createsuperuser
