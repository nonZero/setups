# Project structure and Managing Dependencies

* We assume the following project structure:

            - MyProject/
                - .git/
                - myproject/
                    - settings.py
                    - wsgi.py
                - myapp1/
                    - static/
                    - app.py
                    - models.py
                    - ...
                - myapp2/
                    - ...
                - myapp3/
                    - ...
                - uploads/
                    - # uploaded files stored here
                - .gitignore
                - manage.py

    **Notice:** we use `MyProject` vs `myproject` for simplicity.

* The project is published as a public git repo online, for example via github:

    https://github.com/myuser/MyProject.git

## Managing python requirements with `pipenv`

### Install `pipenv`

Make sure [`pipenv`](http://pipenv.readthedocs.io/en/latest/) is installed globally on your development machine.  To install `pipenv`:

* On Windows: Run the following (when no virtualenv is activated!!! You might need to run this as an administrator.)

        pip install pipenv

* On other platforms: <https://docs.pipenv.org/install/#installing-pipenv>


### Create a virtualenv and install dependencies with `pipenv`

* Install some dependencies with `pipenv install` . Some recommended packages are:
    * `django`: 😎 (required).
    * `psycopg2`: for using postgres (required).
    * `ipython`: a great python shell (Highly recommended).
    * `django-extensions`: for example for `shell_plus` (Highly recommended).
        <br/>(To install, make sure to add `"django_extensions",` to `INSTALLED_APPS`).
    * `Pillow`: for image manipulation (optional - for working with uploaded images).
    * `djangorestframework`: for building REST APIs (optional).

    * For example:

            pipenv install django psycopg2 ipython django-extensions Pillow djangorestframework

* (**Note:**  If you already have an existing `requirements.txt` file, just run `pipenv install` to magically convert it to a `Pipfile`!.   Make sure to edit the `Pipfile` to change version numbers to `"*"`)

* This command...
    * ... will create a virtualenv (it will display its location on the screen).
    * ... will create a `Pipfile`, similar to:

            [[source]]

            url = "https://pypi.python.org/simple"
            verify_ssl = true
            name = "pypi"


            [packages]

            django = "*"
            psycopg2 = "*"
            pytz = "*"
            pillow = "*"
            djangorestframework = "*"
            ipython = "*"
            django-extensions = "*"


            [dev-packages]

    * ...will create a `Pipfile.lock` file with information on current versions installed.

* Use the following command to create a `requirements.txt` file that can be easily used later by `pip` (without `pipenv`) on your server:

        pipenv lock -r > requirements.txt

* Add these files to your git repo: `git add Pipfile Pipflie.lock requirements.txt`
