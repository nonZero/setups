# Installing And Setting Up Postgres On Development Machines
## Ubuntu Linux
* To install run:

        sudo apt-get update
        sudo apt-get install postgresql postgresql-contrib

* **Tip:** Create a superuser role for your linux user:

        sudo -u postgres createuser -s $USER

## Windows
* Download and install postgres 10.1 **64 bit** from here: <https://www.postgresql.org/download/> . During installation, remember the password supplied to user `postgres`.
* **Important** Add your postgres `bin` folder to your user's PATH environment variable.  It is probably located in `C:\Program Files\PostgreSQL\10\bin` but it could by somewhere else.  Make sure your PATH string ends in a semicolon (`;`) !
* In a new cmd windows you shoule be able to connect to the server by running:

        psql --username=postgres

* **Tip:** Create a superuser role for your windows user:

        createuser --username=postgres -s %USERNAME%

