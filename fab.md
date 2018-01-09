# Using `fabric` for Automating Remote Tasks

To automate deployment tasks, we will use `fabric` which allows running commands on remote hosts from python.

On your **development machine** run:

    pipenv install --dev fabric3

(and NOT **fabric** which is not compatible with python 3).

## Defining and running tasks with fabric

Fabric uses a `fabfile` to store reusable tasks.

Create a new `MyProject/fabfile.py` with:


```python
from fabric.api import *

@task
def foo():
    print("FOO!!!!")


@task
def bar():
    print("BAR!!")
```

Now try running the tasks with:

    fab foo
    fab bar
    fab foo bar bar foo

## Executing Commands On Remote Hosts

Use `run()` to execute commands on a remote host:

Change your `fabfile.py` to include:

```python
from fabric.api import *

env.user = "sysop"
env.hosts = ["myproject.mydomain.com"]


@task
def host_type():
    run("uname -a")


@task
def uptime():
    run("uptime")
```

and run::

    fab host_type uptime

## Tips
To execute a command as root from a `fabfile` with `sudo` use one of:

```python
sudo("cmd")
run("sudo cmd")
run("cmd", use_sudo=True)
```

Refer to fabric's [docs](http://www.fabfile.org/) for more info.
