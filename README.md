# Django Setup and Deployment

## Prerequisites
This guide assumes the following:

* Python 3.6
* Django 2.0
* git
* Production/deployment to a new Ubuntu 17.10 server.
* Development machine: Linux, Windows or Mac.


## On Your Development Machine:
- [Project structure + using pipenv](./project-structure.md)
- [Setting up postgres on develoment machines](./postgres-setup.md)
- [Configuring Django to use postgres](./django-postgres.md)
- [Making a modular `settings.py`
 for your project](./settings.md)
- [Preparing your project for deployment](./prepare.md)

## On The Production Machine:
### Interacting with the server
- Getting a server
- [Connecting to your Server and Creating a `sysop` user](./connect.md)
- [Using fabric for automating your remote tasks](./fab.md)
- [Basic Server Setup: Install services and more](./install-packages.md)
### Setting up a django project
- [Clone project and create a virtualenv](./clone.md)
