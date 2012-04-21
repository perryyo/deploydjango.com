Advanced
--------

This section covers more advanced PostgreSQL topics. Feel free to skip around
to whatever sections are of interest.


Creating Read Slaves
********************

Creating `read slaves
<http://en.wikipedia.org/wiki/Master/slave_(technology)>`_ is a popular way to
help scale read requests across a cluster of database servers. Luckily, Heroku
makes this process extremely simple with their `follow
<https://devcenter.heroku.com/articles/heroku-postgresql#follow_beta>`_
feature.

Let's assume your application currently has a single database,
``HEROKU_POSTGRESQL_GREEN``. In order to create a new read slave database, you
can use the ``--follow`` option when creating your new slave databse:

.. code-block:: console

    $ heroku addons:add heroku-postgresql:ronin --follow HEROKU_POSTGRESQL_GREEN
    ----> Adding heroku-postgresql:ronin to deploydjango... done, v7 ($200/mo)
          Attached as HEROKU_POSTGRESQL_AMBER
          Follower will become available for read-only queries when up-to-date
          Use `heroku pg:wait` to track status

The newly created database, ``HEROKU_POSTGRESQL_AMBER``, will now automatically
stay up-to-date with its master database, ``HEROKU_POSTGRESQL_GREEN``.

Using Heroku's ``follow`` feature, you can create as many read slaves as you like.

.. note::
    While Heroku ensures your new slave database will follow its master, there
    will be some replication delay. This means that newly written data to the
    master database may take several seconds to reach your slave database.


Creating a Duplicate Database
*****************************

In many situations, the ability to create a duplicate database can be extremely
useful:

- You want to run tests against your production data, but you don't want to
  expose your production database to your testing code.

- You want to test database migrations before applying them to production.

- You want a copy of a production database just incase something bad happens.

Duplicating a database using Heroku is really simple thanks to Heroku's
`fork <https://devcenter.heroku.com/articles/heroku-postgresql#fork_beta>`_
feature. To duplicate (fork) a copy of your production database,
``HEROKU_POSTGRESQL_GREEN``, you can create a new database using the ``--fork``
option:

.. code-block:: console

    $ heroku addons:add heroku-postgresql:ronin --fork HEROKU_POSTGRESQL_GREEN
    ----> Adding heroku-postgresql:ronin to deploydjango... done, v7 ($200/mo)
          Attached as HEROKU_POSTGRESQL_AMBER
          Database will become available after it completes forking
          Use `heroku pg:wait` to track status

Once the new database is up and running, you'll be able to use it like any
other master database--you can perform write queries, give it a read slave,
whatever you want.

.. note::

    Forked databases do **NOT** stay up-to-date with the database they were
    forked from.


Promoting a Slave Database to a Master Database
***********************************************

Let's say you're in a situation where you need to make one of your read slaves
writable (as a master). Heroku makes this process extremely simple using their
`unfollow <https://devcenter.heroku.com/articles/heroku-postgresql#unfollow>`_
command.

Let's assume you've got a read slave named ``HEROKU_POSTGRESQL_AMBER``. To make
it writable, all we do is run the ``pg:unfollow`` command, like so:

.. code-block:: console

    $ heroku pg:unfollow HEROKU_POSTGRESQL_AMBER
     !    HEROKU_POSTGRESQL_AMBER will become writable and no longer
     !    follow HEROKU_POSTGRESQL_BRONZE. This cannot be undone.

     !    WARNING: Potentially Destructive Action
     !    This command will affect the app: deploydjango
     !    To proceed, type "deploydjango" or re-run this command with --confirm deploydjango

    > deploydjango
    Unfollowing... done
