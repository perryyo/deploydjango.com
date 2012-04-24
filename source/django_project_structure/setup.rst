Setup
-----

In this section I'm going to walk you through organizing a new Django project.
As we go along, you should update your project to match the layout described
below.

This section describes the *best practices* way of organizing your Django
projects for maximum maintainability and clarity.


The Basics - A Default Django Project
*************************************

Before we go any further, let's create a brand new Django 1.4 project:

.. code-block:: console

    $ django-admin.py startproject djangolicious
    $ cd djangolicious
    $ tree .
    .
    ├── djangolicious
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── manage.py

    1 directory, 5 files

In our top-level folder, ``djangolicious``, we've got:

- Our ``djangolicious`` project directory.
- Our ``manage.py`` script which we'll use to manage our Django site.

Inside our ``djangolicious`` project directory, we've got:

- A ``settings.py`` file which holds all of our project settings.
- A ``urls.py`` file which holds our root URL configuration.
- A ``wsgi.py`` file which holds our WSGI application configuration for
  Django's built in ``runserver`` command.
- An ``__init__.py`` file (which tells Python that this directory is a Python
  module).

Now that we've seen a basic skeleton project, let's start making improvements.


Managing Project Dependencies
*****************************

The first improvement we're going to make is adding a ``requirements.txt``
file to our project. **Every** Django project should have a top-level
``requirements.txt`` file which fully lists **all** Python packages used in the
project.

This should include stuff like:

- Django
- South
- django-celery
- django-storages
- etc.

The idea of having a ``requirements.txt`` file is that any developer, as soon
as they clone your project's Git repository--should be able to immediately look
at your ``requirements.txt`` file and install all the packages listed therein.
This way they can run your site locally, make changes, etc., without having to
guess which versions of which packages are required for things to run smoothly.
