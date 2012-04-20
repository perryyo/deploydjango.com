PostgreSQL Configuration
========================

One of Heroku's oldest and most popular addons is their `PostgreSQL Addon
<http://devcenter.heroku.com/categories/heroku-postgres>`_. Did you know that
Heroku's hosted PostgreSQL service is "the largest and most reliable Postgres
service in the world" (`ref <https://postgres.heroku.com/>`_).


About
-----

Setup
-----

Best Practices
--------------

Resources
---------


PyPI Dependencies
-----------------

Before we can get Django working with Heroku's PostgreSQL service, we need to
install the `psycopg2 <http://initd.org/psycopg/>`_ PyPI module. The first
thing you'll want to do is install ``psycopg2`` locally into your project's
virtualenv:

.. code-block:: console

    $ workon myproject
    (myproject) $ pip install -U psycopg2
    Downloading/unpacking psycopg2
      Running setup.py egg_info for package psycopg2
        no previously-included directories found matching 'doc/src/_build'
    Installing collected packages: psycopg2
      Running setup.py install for psycopg2

        no previously-included directories found matching 'doc/src/_build'
    Successfully installed psycopg2
    Cleaning up...
    (myproject) $

.. note::
    If you're running Ubuntu, you'll need to install the ``libpq-dev`` package.

Once you've got ``psycopg2`` installed in your virtualenv, you'll want to run
``pip freeze`` to confirm it's installed and working as expected:

.. code-block:: console

    (myproject) $ pip freeze
    psycopg2==2.4.4
    wsgiref==0.1.2
    (myproject) $

Assuming you see something like ``psycopg2==2.4.4``, you'll then want to add
that line to your project's ``requirements.txt`` file:

.. code-block:: console

    (myproject) $ echo "psycopg2==2.4.4" >> requirements.txt
    (myproject) $
