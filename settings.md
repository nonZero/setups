# Creating A Modular `settings.py` File
- To allow per-host modification to your `settings.py` file, we will use the following structure to store your settings:

            - MyProject/
                - myproject/
                    - settings.py
                    - base_settings.py
                    - local_settings.py  # DO NOT ADD TO GIT!

- To switch to this structure:
    - Add to `MyProject/.gitignore` the line `local_settings.py` to prevent git from adding it to the repo.
    - Rename your current `myproject/settings.py` to `myproject/base_settings.py` .
    - Create a new `myproject/settings.py` file with the following content:

        ```python
        from .base_settings import *
        from .local_settings import *
        ```

    - Create a new `local_settings.py` file with the follwoing content:

        ```python
        DEBUG = True
        EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
        ```

        Remember: **Don't add `local_settings.py` to git!**

    - In `base_settings.py`:
        - Change `DEBUG = True` to `DEBUG = False`.
        - Move the line strating with `SECRET_KEY =` to the end of `local_settings.py` .

Make sure your project still runs OK :-)

## Tips for maintaining settings files.
* Generally, try keeping all settings in your `base_settings.py` file.
* If different settings are needed for production and dev, keep the production value as a default in `base_settings` and override locally in `local_settings.py`.
* Secret/Security related settings should be kept out of git.  They can either be stored on a `local_settings.py` file on the server or injected via environment variables.
