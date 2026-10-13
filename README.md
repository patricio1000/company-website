# Company Website

A small Django project for Lab 5, following Chapter 4 of *Django for Beginners*.

## Versions

- Python **3.13.15**
- Django **5.2.17**, as required by the assignment (rather than the book's Django version)

The exact installed dependencies are recorded in `requirements.txt`.

## Run locally

First install Python 3.13.15. From this directory, create and activate a virtual environment.

macOS/Linux:

```sh
python3.13 -m venv .venv
source .venv/bin/activate
```

Windows PowerShell:

```powershell
py -3.13 -m venv .venv
.venv\Scripts\Activate.ps1
```

Check that `python --version` reports `Python 3.13.15`, then run:

```sh
python -m pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

Open http://127.0.0.1:8000/ for Home and http://127.0.0.1:8000/about/ for About.
Use the Home and About links in the shared header to move between them.

This is a local learning project. `DEBUG=True` and the clearly labeled development-only
secret key are not production settings. No deployment is required for this lab.

## Test

```sh
python manage.py check
python manage.py test
```

The test suite contains eight `SimpleTestCase` tests: four for each page.
Each set checks the direct URL, named URL, template, and heading. These tests do
not require a test database because the pages do not query any models.

## How it works

- `django_project/settings.py` registers `pages` and adds the root `templates/`
  directory to `TEMPLATES[0]["DIRS"]`. Django searches these configured directories
  before looking in installed apps' template directories with `APP_DIRS=True`.
- `django_project/urls.py` includes `pages.urls`. The app names its routes `home`
  and `about`, so templates can use the `{% url %}` tag instead of hardcoded URLs.
- `home_page_view` is a function-based view. It explicitly builds a context
  dictionary with three inventory items and a greeting, then calls `render()`.
  This style makes a small custom request-handling flow easy to follow.
- `AboutPageView` subclasses `TemplateView`. Its `get_context_data()` calls
  `super()` and adds the sample address and phone number. `as_view()` connects
  the class to the URL dispatcher. This style reuses Django's common
  template-rendering behavior and can be extended with inheritance.
- `base.html` owns the shared navigation and content block. Both page templates
  extend it, avoiding repeated header markup.
- `home.html` demonstrates variables, the `now` and `for` tags, the `length` and
  `title` filters, and a template comment. The greeting renders as
  "Thank You For Visiting." The about page displays the assignment's fictional
  contact details.

## Repository contents

The virtual environment, SQLite database, Python caches, and local `.env` files
are excluded by `.gitignore`. Recreate the virtual environment and database with
the setup commands above after cloning.
