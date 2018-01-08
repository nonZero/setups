# Django Setup and Deployment

## Prerequisites
This guide assumes the following:

* Python 3.6
* Django 2.0
* git
* Production/deployment to a new Ubuntu 17.10 server.
* Development machine: Linux, Windows or Mac.


## Preparing Your Django Project:
- [Project structure + using pipenv](./project-structure.md)
- [Setting up postgres on develoment machines](./postgres-setup.md)
- [Configuring Django to use postgres](./django-postgres.md)
- [Making a modular `settings.py`
 for your project](./settings.md)
- [Preparing your project for deployment](./prepare.md)

## Interacting with the server
- [Interacting with the server with SSH](./using-ssh.md)
- [Creating a `sysop` user](./sysop.md)
- [Using fabric for automating your remote tasks](./fab.md)
- [Update Server and Install Services](./install-packages.md)
- [Create a postgres sysop user](./postgres-sysop.md)
### Setting up a Django Project
- [Clone project and create a virtualenv](./clone.md)
- [Create a production local_settings file ](./prod-settings.md)
- [Create a database and tables](./migrate.md)
- [Setup `uwsgi` and `nginx`](./uwsgi.md)
- [Automating upgrades](./upgrade.md)

