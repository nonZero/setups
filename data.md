# Moving Data Between Servers

## Exporting data from Django with `dumpdata`

To export all data from your project:

    python manage.py dumpdata --indent 2 > all_data.json

## Loading data with `loaddata`

Loading data is easy as:

    python manage.py loaddata all_data.json

However, this will not delete previously created data - so it will most
probably fail if you already have existing data in your db.