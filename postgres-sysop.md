# Setup A postgres `sysop` User

When using postgres with linux, linux users can connect to postgres users without a password (This is called [Trust Authentication](https://www.postgresql.org/docs/current/static/auth-methods.html#AUTH-TRUST)).

On Ubuntu, postgres creates a `postgres` linux user which is a postgres-superuser - and can create databases and more postgres users.

To make things easier - let's make our `sysop` user a postgres superuser as well!

    @task
    def create_postgres_su():
        run("sudo -u postgres createuser -s sysop")

and run:

    fab create_postgres_su
