## Clone project

Add this to `fabfile.py` to clone the project to your server:

    # ...

    env.project = "MyProject"
    env.code_dir = f"/home/sysop/{env.project}"
    env.clone_url = "https:///github.com/myuser/MyProject.git"

    @task
    def clone_project():
        run(f"git clone {env.clone_url} {env.code_dir}", pty=False)


run:

    fab clone_project

## Create a virtualenv

Add this to `fabfile.py` to create a virtualenv:

    # ...

    env.venv_name = "myproject"
    env.venvs = f"/home/sysop/.virtualenvs/"
    env.venv_path = f"{env.venvs}{env.venv_name}/"
    env.venv_command = f"source {env.venv_path}/bin/activate"

    @task
    def create_venv():
        run(f"mkdir -p {env.venvs}")
        run(f"virtualenv -p /usr/bin/python3 --prompt='({env.venv_name}) ' {env.venv_path}")

and run:

    fab create_venv

## Running commands in your virtualenv

We will create a context manager to run commands in our virtualenv:

    from contextlib import contextmanager

    @contextmanager
    def virtualenv():
        with cd(env.code_dir):
            with prefix(env.venv_command):
                yield

Example usage:

    @task
    def upgrade_pip():
        with virtualenv():
            run("pip install --upgrade pip", pty=False)

To run:

    fab upgrade_pip


## Install requirements

Assuming your project has an updated `requirements.txt` (created with `pipenv lock -r > requirements.txt`):

    @task
    def pip_install():
        with virtualenv():
            run("pip install -r requirements.txt", pty=False)

run:

    fab pip_install

## Create a `local_settings` file

We will now create a settings file for your project.

This settings file will be created automatically from a template or manually with an editor.  Let's try this time to do it manually.

It should look something like this:

    ALLOWED_HOSTS = ["myproject.mydomain.com"]

    SECRET_KEY = ''

    # Send error reports to:
    ADMINS = (
        ('Your Name', 'your.name@gmail.com'),
        ('John Doe', 'john.doe@gmail.com'),
    )

    # send some other reports to...
    MANAGERS = ADMINS

Add a secret key to the text above.  If your project has `django-extensions`, to generate a secret key, run locally :

    python manage.py generate_secret_key

Or use an [online tool](https://www.miniwebtool.com/django-secret-key-generator/).

Connect to your server and use either `nano` or `vim` to edit `MyProject/myproject/local_settings.py`:

    ssh sysop@myproject.mydomain.com
    nano MyProject/myproject/local_settings.py

Paste the file content there, save and exit (<kbd>Ctrl</kbd>+<kbd>X</kbd> with nano).

Use <kbd>Ctrl</kbd>+<kbd>D</kbd> to logout from ssh.

To check if your project is set up correctly use `check`:

    @task
    def m(cmd):
        with virtualenv():
            run("./manage.py {cmd}".format(cmd), pty=False)

    @task
    def check():
        m('check')

    @task
    def send_test_mail():
        m('sendtestemail --admin')

run:

    fab check
    fab send_test_mail

(Note: The mail will most probably get into your spam folder.  We will later configure your server to be whitelisted by popular mail servers.)