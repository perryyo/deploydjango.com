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
    If you aren't familiar with requirements files, you should read through
    `Heroku's guide <https://devcenter.heroku.com/articles/python-pip>`_ to
    managing Python dependencies with pip.

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

Now that you know why we need one, let's do it!


Step 1 - Think Modular
^^^^^^^^^^^^^^^^^^^^^^

Now, having a top-level ``requirements.txt`` file is mandatory, but so is
keeping our dependencies easy to manage.

What does this mean? Well, in all likelihood, your development environment
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
    $ touch requirements/{common.txt,dev.txt,prod.txt,test.txt}
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


Step 2 - Define Your Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

So, now that we know why modular requirements rock, let's see what they
actually look like in practice.

Below I've specified 4 requirements files taken from actual projects I've
worked on. I'll explain them as we go.

First up, our ``requirements/common.txt`` file. This file holds all of our
**shared** requirements--basically, any program needed in all of your
environments (development, production, testing, whatever):

.. code-block:: text

    # requirements/common.txt
    Django==1.4
    django-cache-machine==0.6
    django-celery==2.5.5
    django-dajaxice==0.2
    django-guardian==1.0.4
    django-kombu==0.9.4
    django-pagination==1.0.7
    django-sorting==0.1
    django-tastypie==0.9.11
    Fabric==1.4.1
    lxml==2.3.4
    pyst2==0.4
    South==0.7.4
    Sphinx==1.1.3

As you can see, the stuff I've included is all stuff that is essentially
required for my Django site to run, no matter what. If I don't have any of
these, my site will not work.

The next file below is my ``requirements/dev.txt`` file, which holds all of my
development only dependencies. These are things that I need, but only when I'm
developing code locally:

.. code-block:: text

    # requirements/dev.txt
    -r common.txt
    django-debug-toolbar==0.9.4

In my development environment, I typically use a simple SQLite3 database (so I
have no reason to include any special drivers), and the excellent
``django-debug-toolbar`` package which allows me to inspect DB queries,
performance issues, etc.

And incase you're wondering--the first line, ``-r common.txt`` tells ``pip`` to
include all of my common dependencies in addition to the dependencies I have
listed below.

This allows me to run ``pip install -r requirements/dev.txt`` from the command
line to install all of my development requirements:

.. code-block:: console

    $ pip install -r requirements/dev.txt
    Downloading/unpacking Django==1.4 (from -r requirements/common.txt (line 1))
      Downloading Django-1.4.tar.gz (7.6Mb): 7.6Mb downloaded
      Running setup.py egg_info for package Django

    Downloading/unpacking django-cache-machine==0.6 (from -r requirements/common.txt (line 2))
      Downloading django-cache-machine-0.6.tar.gz
      Running setup.py egg_info for package django-cache-machine

    ... snipped for brevity ...

As you can see above, it actually works! When we ``pip`` install our
``requirements/dev.txt`` file, it successfully intalls not only our development
dependencies (eg: ``django-debug-toolbar``), but also our ``common.txt``
dependencies! Beautiful!

Below is a sample ``requirements/prod.txt`` requirements file which specifies
all production dependencies **and** common dependencies:

.. code-block:: text

    # requirements/prod.txt
    -r common.txt
    boto==2.1.1
    cssmin==0.1.4
    django-compressor==1.1.2
    django-htmlmin==0.5.1
    django-pylibmc-sasl==0.2.4
    django-storages==1.1.3
    gunicorn==0.14.1
    newrelic==1.2.0.246
    psycopg2==2.4.5
    pylibmc==1.2.2
    raven==1.3.5
    slimit==0.6

And lastly, here is one of my old ``requirements/test.txt`` files, which
provides test specific dependencies. These packages are only used for running
unit tests against my project:

.. code-block:: text

    # requirements/test.txt
    -r common.txt
    django-coverage==1.2.2
    django-nose==0.1.3
    mock==0.8.0
    nosexcover==1.0.7

- If I was going to run my code locally for development, I'd install my
  ``requirements/dev.txt`` dependencies.

- If I was going to run my code in production, I'd install my
  ``requirements/prod.txt`` dependencies.

- If I was going to do some testing on my code, I'd install my
  ``requirements/test.txt`` dependencies.

You get the idea. The main point here is that breaking up your dependencies is:

- Easy.
- Powerful.
- Intuitive.


Step 3 - Heroku Best Practices
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now that we've got our requirements files all modularized, I bet you're
thinking: **Why even bother with a top-level ``requirements.txt`` file?**

So, here's the deal:

- Having a top-level ``requirements.txt`` is a standard.
- Heroku automatically reads your top-level ``requirements.txt`` file everytime
  you deploy your project (which we'll talk about in the next
  chapter) and installs any dependencies listed in it.

Since Heroku will install whatever is defined in your ``requirements.txt``
file, this gives you a few choices:

- Make Heroku install *all* of your dependencies: ``common.txt``, ``dev.txt``,
  ``prod.txt``, etc...
- Have Heroku *only* install the dependencies it needs.

Since we're only going to use Heroku for deploying our production quality
(live) website, it's best to have Heroku install *only* what it needs.

This will make future deployments faster, since Heroku has less things to do.

Open up your top-level ``requirements.txt`` file and enter the following:

.. code-block:: text

    # Install all of our production dependencies only.
    -r requirements/prod.txt

This way, Heroku will do exactly what we want it to: install our production software.
