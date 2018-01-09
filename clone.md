## Clone project

Add this to `fabfile.py` to clone the project to your server:

```python
# ...

env.project = "MyProject"
env.code_dir = f"/home/sysop/{env.project}"
env.clone_url = "https:///github.com/myuser/MyProject.git"

@task
def clone_project():
    run(f"git clone {env.clone_url} {env.code_dir}", pty=False)
```

run:

    fab clone_project

## Create a virtualenv

Add this to `fabfile.py` to create a virtualenv:

```python
# ...

env.venv_name = "myproject"
env.venvs = f"/home/sysop/.virtualenvs/"
env.venv_path = f"{env.venvs}{env.venv_name}/"
env.venv_command = f"source {env.venv_path}/bin/activate"

@task
def create_venv():
    run(f"mkdir -p {env.venvs}")
    run(f"virtualenv -p /usr/bin/python3 --prompt='({env.venv_name}) ' {env.venv_path}")
```

and run:

    fab create_venv

## Running commands in your virtualenv

We will create a context manager to run commands in our virtualenv:

```python
from contextlib import contextmanager

@contextmanager
def virtualenv():
    with cd(env.code_dir):
        with prefix(env.venv_command):
            yield
```

Example usage:

```python

    @task
    def upgrade_pip():
        with virtualenv():
            run("pip install --upgrade pip", pty=False)
```

To run:

    fab upgrade_pip


## Install requirements

Assuming your project has an updated `requirements.txt` (created with `pipenv lock -r > requirements.txt`):

```python
@task
def pip_install():
    with virtualenv():
        run("pip install -r requirements.txt", pty=False)
```

run:

    fab pip_install
