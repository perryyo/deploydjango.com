PostgreSQL Configuration
========================

One of Heroku's oldest and most popular addons is their `PostgreSQL Addon
<http://devcenter.heroku.com/categories/heroku-postgres>`_. Did you know that
Heroku's hosted PostgreSQL service is "the largest and most reliable Postgres
service in the world"?


About
-----

The `Heroku PostgreSQL service <https://addons.heroku.com/heroku-postgresql>`_
allows you to easily create, destroy, resize, backup, restore, and scale your
database infrastructure both from the command line (via the `Heroku CLI app
<https://toolbelt.heroku.com/>`_) as well as from the web (using `Heroku's
standalone site <http://postgres.heroku.com/>`_).

Heroku's PostgreSQL service is an excellent choice for you if:

- Your application requires a relational database for storing information,

- You don't want to worry about downtime or data loss (since Heroku fully
  manages your PostgreSQL instances),

- You want the ability to scale your PostgreSQL database by:

  - Creating read slaves on demand,
  - Creating database snapshots for experimental testing (eg: duplicate your
    production database and run tests against the clone),
  - Easily upgrading (or downgrading) the size of your database,
  - Paying only for what you use (by the second) so you can instantly scale up
    to support burst traffic--and down again to prevent bankruptcy :),

- You want the ability to instantly backup and restore your data to any
  PostgreSQL database (no vendor lock-in).

- Your application is running anywhere on the internet; Heroku's PostgreSQL
  service is a standalone service, it works just like any normal PostgreSQL
  server, so you don't need to make any application changes.


Setup
-----

Through the rest of this article, I'll assume that you've already got a working
Heroku application setup, and that you are using the `Heroku CLI tool
<https://toolbelt.heroku.com/>`_.


Bootstrap a Database
********************

To get started, let's bootstrap a new PostgreSQL server instance. In the
example below, we'll create a free (shared) PostgreSQL database:

.. code-block:: console

    $ heroku addons:add shared-database
    ----> Adding shared-database to deploydjango... done, v2 (free)

If we wanted to create a larger (paid) database, we could run the following command:

.. code-block:: console

    $ heroku addons:add heroku-postgresql:ronin
    ----> Adding heroku-postgresql:ronin to deploydjango... done, v3 ($200/mo)
          Attached as HEROKU_POSTGRESQL_IVORY
          The database should be available in 3-5 minutes
          Use `heroku pg:wait` to track status

To verify that our database(s) exist, we can now run the ``pg:info`` command:

.. code-block:: console

    $ heroku pg:info
    === SHARED_DATABASE (DATABASE_URL)
    Data Size    (empty)
    === HEROKU_POSTGRESQL_IVORY
    Plan         Ronin
    Status       preparing
    Data Size    -1 B
    Tables       -1
    PG Version   ?
    Created      2012-04-20 07:26 UTC
    Conn Info    "host=ec2-xx-xx-xx-xx.compute-1.amazonaws.com
                 port=5432 dbnamexxxxxxxx=
                 user=xxxx sslmode=require
                 password=xxxxxxxxxxxxxxx"
    Maintenance  not required

As you can see from the output above, our Heroku application now has two
databases defined, a free (shared) database, and a paid database.

.. note::
    You can find a complete list of the available paid Heroku databases on
    their database pricing page: https://postgres.heroku.com/pricing


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
