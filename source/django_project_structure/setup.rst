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
