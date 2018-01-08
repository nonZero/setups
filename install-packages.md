# Server Setup

## Update Server Packages

Add and run the following fabric command to update your system:

    @task
    def apt_upgrade():
        sudo("apt-get update", pty=False)
        sudo("apt-get upgrade -y", pty=False)


and run:

    fab apt_upgrade

## Install More Packages

Let's install many great packages:

    APT_PACKAGES = [
        # generic system related packages
        'unattended-upgrades',  # for auto updating your system
        'ntp',  # To keep time synchromized
        'fail2ban',  # to secure against SSH/other attacks

        'postfix',  # mail server
        'opendkim',  # SSL for mail
        'opendkim-tools',

        # useful tools
        'git',
        'htop',
        'most',

        'python3',
        'virtualenvwrapper',  # for easily managing virtualenvs

        # required libraries for building some python packages
        'build-essential',
        'python3-dev',
        'libpq-dev',
        'libjpeg-dev',
        'libjpeg8',
        'zlib1g-dev',
        'libfreetype6',
        'libfreetype6-dev',
        'libgmp3-dev',

        # postgres database
        'postgresql',

        'nginx',  # a fast web server
        'uwsgi',  # runs python (django) apps via WSGI

        'rabbitmq-server',  # for offline tasks via celery
    ]

    @task
    def apt_install():
        pkgs = " ".join(APT_PACKAGES)
        sudo(f"DEBIAN_FRONTEND=noninteractive apt-get install -y -q {pkgs}", pty=False)

and run:

    fab apt_upgrade



## Setup postgres

When using postgres with linux, linux users can connect to postgres users without a password (This is called [Trust Authentication](https://www.postgresql.org/docs/current/static/auth-methods.html#AUTH-TRUST)).

On Ubuntu, postgres creates a `postgres` linux user which is a postgres-superuser - and can create databases and more postgres users.

To make things easier - let's make our `sysop` user a postgres superuser as well!

    @task
    def create_postgres_su():
        run("sudo -u postgres createuser -s sysop")

and run:

    fab create_postgres_su
