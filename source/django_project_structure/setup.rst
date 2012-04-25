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

.. note::
    If you aren't familiar with requirements files, you should read through the
    official `pip <http://www.pip-installer.org/en/latest/>`_ documentation, as
    pip is the best practices way to install your Python packages.

This should include stuff like:

.. code-block:: text

    Django==1.4
    psycopg2==2.4.5
    South==0.7.3
    gunicorn==0.14.1
    newrelic==1.2.0.246
    django-celery==2.4.2

The idea of having a ``requirements.txt`` file is that any developer, as soon
as they clone your project's Git repository--should be able to immediately look
at your ``requirements.txt`` file and install all the packages listed therein.
This way they can run your site locally, make changes, etc., without having to
guess which versions of which packages are required for things to run smoothly.

Now that you know why we need one, let's do it! Be sure to follow along with
your own project as we go.


Step 1 - Think Modular
^^^^^^^^^^^^^^^^^^^^^^

Now, having a top-level ``requirements.txt`` file is mandatory, but so is
keeping our dependencies easy to manage.

What does this mean? Well, in all likelihood, your develpment environment
requires different dependencies than your production environment. Sure, you
*could* throw both your development **and** production requirements into a
single ``requirements.txt`` file, but that makes things difficult to maintain
over time.

Instead, it's best to organize your requirements by the environment in which
they're used.

Here's what we'll do (inside the root of our ``djangolicious`` directory):

.. code-block:: console

    $ ls
    djangolicious  manage.py
    $ touch requirements.txt
    $ mkdir requirements
    $ touch requirements/{common.txt,dev.txt,prod,txt,test.txt}
    $ tree .
    .
    ├── djangolicious
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── manage.py
    ├── requirements
    │   ├── common.txt
    │   ├── dev.txt
    │   ├── prod.txt
    │   └── test.txt
    └── requirements.txt

    2 directories, 10 files

As you can see, I created a new top-level directory, ``requirements``, which
holds a variable amount requirement files: one for each environment.

If your app only has a development environment, then only include a ``dev.txt``
file. If your app has development, production, testing, tom, and rudy--then
create a single ``.txt`` file for each of them.

.. note::
    The special file ``common.txt`` is meant to hold all dependencies that are
    commonly shared between all other environments. For example, Django. Since
    Django is needed in **all** of your environments, regardless of whether or
    not you're in development or production, you'd put it here.

Having our requirements files separate means that if I'm a developer working on
the project in my local environment **only**, I can simply install the
``requirements/dev.txt`` dependencies, and avoid installing the others (for
production, staging, testing, whatever).

*But why do I care how many requirements I have to install? Why don't I just
install them all?*

- Installing requirements can take a long time. In big projects, this equates
  to large chunks of time (30 minutes plus).

- Many requirements depend on external software and libraries to be installed
  on your local system in order to build. This means that avoiding installing
  libraries can not only save you time, but also save you massive headaches,
  like figuring out which version of ``libxml2`` and ``libpq-dev`` you need
  installed for your equivalent Python libraries to build.

- It lowers the barrier to entry for new project developers. If you've got a
  new developer working on your project, attempting to submit code, it is a
  lot simpler for them to install a few things and get working right away than
  to install **everything** and have trouble getting *anything* working.


Step 2 - The Heroku Pattern
^^^^^^^^^^^^^^^^^^^^^^^^^^^

*Now that we've established our modular requirements, exactly how do we tell
our top-level ``requirements.txt`` file to include all of our others?*

Thankfully, `pip <http://www.pip-installer.org/en/latest/>`_ already has this
problem solved.

Open up your main ``requirements.txt`` file and enter the following:

.. code-block:: text

    # Install all of our production dependencies only.
    -r requirements/prod.txt

.. note::
    The ``-r`` flag tells pip that the following relative path is another
    requirments file to parse.

The way requirements work on Heroku is that each time you push your code to
Heroku, Heroku will analyze your top-level ``requirements.txt`` file and
install whatever dependencies are listed.

Since we're only going to use Heroku to deploy our production software (we'll
do our development and testing locally throughout this book), I'm **only**
including the ``requirements/prod.txt`` file, so that Heroku **only** installs
our production dependencies.
