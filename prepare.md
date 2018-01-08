# Preparing your Django Project

## Add some settings to `settings.py`:

### Static Files
Configure a folder to collect all static files from all apps (`/app*/static/...`) into.  This allows offloading static file serving to a fast webserver.  Add the following below `STATIC_URL`:

    STATIC_ROOT = os.path.join(BASE_DIR, "collected_static")

### Configure outgoing emails:

    # For various emails (might be sent to users)
    DEFAULT_FROM_EMAIL = "no.relpy@myproject.com"

    # For emails sent to admins:
    # Default **from** address:
    SERVER_EMAIL = "admin@myproject.com"
    # Subject-line prefix. Make sure to include the trailing space.
    EMAIL_SUBJECT_PREFIX = "[myproject] "

## Customize the Default Error Views

Add an `404.html` file to a templates folder (`app1/tempaltes`).  For example:

    {% extends "base.html" %}
    {% load i18n %}

    {% block page_title %}
        {% trans "404 Not Found" %}
    {% endblock %}

    {% block content %}
        <h1>{% trans "404 Not Found" %}</h1>
    {% endblock %}


## External Resources
* [Deployment checklist - Django Documentation](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/)




