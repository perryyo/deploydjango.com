Setup
-----

Through the rest of this article, I'll assume you've already got a working
Heroku application setup, and that you're using the `Heroku CLI tool
<https://toolbelt.heroku.com/>`_.


Bootstraping a Database
***********************

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
    === HEROKU_POSTGRESQL_GREEN_URL
    Plan         Ronin
    Status       preparing
    Data Size    -1 B
    Tables       -1
    PG Version   ?
    Created      2012-04-20 07:26 UTC
    Conn Info    "host=ec2-yy-yy-yy-yy.compute-1.amazonaws.com
                 port=5432 dbname=yyyyy
                 user=yyyyy sslmode=require
                 password=yyyyy"
    Maintenance  not required

As you can see from the output above, our Heroku application now has two
databases defined, a free (shared) database, and a paid database.

.. note::
    You can find a complete list of available Heroku databases plans on their
    `pricing page <https://postgres.heroku.com/pricing>`_.

As you can also see--there are obviously some differences between the shared
database we created, and the paid database:

- The shared database is created insantly (there is no delay), while the paid
  database is provisioned on the fly, and takes a minute or so to become available.

- The paid database lists connection info while the shared database does not.
  This is because paid databases allow you to directly connect to them using
  the `psql <http://www.postgresql.org/docs/8.4/static/app-psql.html>`_ tool
  like you would with any normal PostgreSQL database.


Configuring Django to Use PostgreSQL
************************************

Now that we've got a database running, let's configure our Django site to use
it!

Like all other Heroku addons--when we created our database in the previous
section, Heroku added a couple environment variables to our application, which
specify our newly created database information. Let's quickly take a look:

.. code-block:: console

    $ heroku config
    DATABASE_URL                => postgres://xxxxx:xxxxx@ec2-xx-xx-xx-xx.compute-1.amazonaws.com/xxxxx
    HEROKU_POSTGRESQL_GREEN_URL => postgres://yyyyy:yyyyy@ec2-yy-yy-yy-yy.compute-1.amazonaws.com:5432/yyyyy
    SHARED_DATABASE_URL         => postgres://xxxxx:xxxxx@ec2-xx-xx-xx-xx.compute-1.amazonaws.com/xxxxx

As you can see, we've now got 3 environment variables defined. One for each our
our databases, and one extra variable, ``DATABASE_URL``. The ``DATABASE_URL``
variable is a special variable, in that its only purpose is to provide a
standardized 'default' database for your application.

At the moment, it looks like our shared database (``SHARED_DATABASE_URL``) is
set as the default database (``DATABASE_URL``). If we wanted to set our larger
(and more performant) database (``HEROKU_POSTGRESQL_GREEN_URL``) as the
default, we could do so by running the ``pg:promote`` command:

.. code-block:: console

    $ heroku pg:promote HEROKU_POSTGRESQL_GREEN
    -----> Promoting HEROKU_POSTGRESQL_GREEN to DATABASE_URL... done

    $ heroku config
    DATABASE_URL                => postgres://yyyyy:yyyyy@ec2-yy-yy-yy-yy.compute-1.amazonaws.com:5432/yyyyy
    HEROKU_POSTGRESQL_GREEN_URL => postgres://yyyyy:yyyyy@ec2-yy-yy-yy-yy.compute-1.amazonaws.com:5432/yyyyy
    SHARED_DATABASE_URL         => postgres://xxxxx:xxxxx@ec2-xx-xx-xx-xx.compute-1.amazonaws.com/xxxxx

Now our ronin database is the default!

The next thing we need to do is tell Django to use our new Heroku database.
What we're going to do is tell Django to use our ``DATABASE_URL`` database just
like we would any other database, with one exception: instead of hard-coding in
our database credentials--we'll simply grab them from the environment!

.. code-block:: python

    # settings.py
    from os import environ
    from urlparse import urlparse

    if environ.has_key('DATABASE_URL'):
        url = urlparse(environ['DATABASE_URL'])
        DATABASES['default'] = {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': url.path[1:],
            'USER': url.username,
            'PASSWORD': url.password,
            'HOST': url.hostname,
            'PORT': url.port,
        }

Also: don't forget to add ``psycopg2`` to your ``requirements.txt`` file, since
the ``psycopg2`` library is required for Django to interface with PostgreSQL:

.. code-block:: none

    # requirements.txt
    psycopg2==2.4.5
    ...

Now that we've got Django configured to use whichever database is currently set
to ``DATABASE_URL``, we can easily switch our primary database without changing
a single line of code!


Destroying a Database
*********************

If you'd like to remove a database that you've already provisioned, you can do
so via the ``addons:remove`` command:

.. code-block:: console

    $ heroku addons:remove HEROKU_POSTGRESQL_GREEN
     !    WARNING: Potentially Destructive Action
     !    This command will affect the app: deploydjango
     !    To proceed, type "deploydjango" or re-run this command with --confirm deploydjango

    > deploydjango
    ----> Removing HEROKU_POSTGRESQL_GREEN from deploydjango... done, v9 ($200/mo)

Heroku bills for database usage by the second, so as soon as your database has
been removed, you'll stop being charged.
