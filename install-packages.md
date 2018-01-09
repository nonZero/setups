# Server Setup

## Update Server Packages

Add and run the following fabric command to update your system:

```python
@task
def apt_upgrade():
    sudo("apt-get update", pty=False)
    sudo("apt-get upgrade -y", pty=False)
```


and run:

    fab apt_upgrade

## Install More Packages

Let's install many great packages:

```python
APT_PACKAGES = [
    # generic system related packages
    'unattended-upgrades',  # for auto updating your system
    'ntp',  # To keep time synchromized
    'fail2ban',  # to secure against SSH/other attacks

    'mailutils',  # postfix mail server and goodies
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

    # nginx - a fast web server
    'nginx',

    # uwsgi: runs python (django) apps via WSGI
    'uwsgi',
    'uwsgi-plugin-python3',

    'rabbitmq-server',  # for offline tasks via celery
]

@task
def apt_install():
    host = env.hosts[0]
    sudo(f'''debconf-set-selections <<< "postfix postfix/mailname string {host}"''')
    sudo(f'''debconf-set-selections <<< "postfix postfix/main_mailer_type string 'Internet Site'"''')

    pkgs = " ".join(APT_PACKAGES)
    sudo(f"DEBIAN_FRONTEND=noninteractive apt-get install -y -q {pkgs}", pty=False)
```

and run:

    fab apt_install

Since we have installed nginx, a default web page should appear when accesing your server in your web browser:  <http://myproject.mydomain.com> .
