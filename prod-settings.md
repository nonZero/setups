## Create a Production `local_settings` file

We will now create a settings file for your project, on the server.

Let's try this time to do it manually, with an editor.

The settings file should look something like this:

    ALLOWED_HOSTS = ["myproject.mydomain.com"]

    SECRET_KEY = ''

    # Send error reports to:
    ADMINS = (
        ('Your Name', 'your.name@gmail.com'),
        ('John Doe', 'john.doe@gmail.com'),
    )

    # send some other reports to...
    MANAGERS = ADMINS

Edit this file locally in an open editor.  Add a secret key to the text above.  If your project has `django-extensions`, to generate a secret key, run locally :

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
