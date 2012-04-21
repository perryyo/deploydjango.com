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


View Slow Queries
*****************

A big part of writing good code is knowing when you do things wrong. Slow
database queries, in particular, are probably the greatest cause of poor site
performance.

Luckily for us, Heroku's logging system allows you to easily view a stream of
slow query logs directly from your console. Any query that takes longer than
50ms to execute will be dumped into the log output.

To view the streaming logs, you can simply run:
``heroku logs --tail --ps postgres``. If you'd like to just view the most
recent logs, you can run the same command without the optional ``--tail``
argument: ``heroku logs --ps postgres``.


Backing Up Your Database
************************

Backing up your database is incredibly important, but not always easy to do. To
combat the complexity of backing up your database, Heroku created their
extremely useful `pgbackups addon <https://addons.heroku.com/pgbackups>`_.

Incredibly, all of the pgbackups addon plans are completely free!

To get started, we'll use the largest available backup plan: auto-month. This
plan will:

- Automatically backup your ``DATABASE_URL`` database every night.
- Retain 7 daily backups.
- Retain 5 weekly backups.
- Retain 10 manual backups.

To get it going, install the addon:

.. code-block:: console

    $ heroku addons:add pgbackups:auto-month
    ----> Adding pgbackups:auto-month to deploydjango... done, v14 (free)
          You can now use "pgbackups" to backup your databases or import an external backup.

Once you've got the addon installed, Heroku will start automatically backing up
your primary database each day.

To view a list of your available backups, you can run the ``pgbackups``
command. Since we just installed the auto-month backup plan, however, we've got
no existing backups:

.. code-block:: console

    $ heroku pgbackups
     !    No backups. Capture one with `heroku pgbackups:capture`.

Let's fix that right now by forcing a manual backup:

.. code-block:: console

    $ heroku pgbackups:capture

    SHARED_DATABASE (DATABASE_URL)  ----backup--->  b001

    Capturing... done
    Storing... done

Now, if we run the ``pgbackups`` command again, we should see:

.. code-block:: console

    $ heroku pgbackups
    ID   | Backup Time         | Size       | Database
    -----+---------------------+------------+----------------
    b001 | 2012/04/20 23:09.50 | 918.0bytes | SHARED_DATABASE

As time progresses, and we gradually get more backups, they'll show up in the
``pgbackups`` listing.

.. note::
    In the backup example above, we backed up our default database
    (``DATABASE_URL``). If you'd like to backup another database, you can do so
    by specifying its name, for example: ``heroku pgbackups:capture
    HEROKU_POSTGRESQL_GREEN``.

As a quick note: all manually captured backups display with a ``b`` prefix in
the ``heroku pgbackups`` listing, while all automatically captured backups
display with an ``a`` prefix.


Downloading Your Backups
************************

So you've got a few database backups, and you'd like to download them--no
problem! Assuming you've got the following backup available:

.. code-block:: console

    $ heroku pgbackups
    ID   | Backup Time         | Size       | Database
    -----+---------------------+------------+----------------
    b001 | 2012/04/20 23:09.50 | 918.0bytes | SHARED_DATABASE

You can create a publicly available download URL from `Amazon S3
<http://aws.amazon.com/s3/>`_ (which is good for 10 minutes) by running:

.. code-block:: console

    $ heroku pgbackups:url b001
    "https://s3.amazonaws.com/hkpgbackups/app4444444@heroku.com/b001.dump?AWSAccessKeyId=test&Expires=1334989747&Signature=Juy%2FKGQvJ5zPLknUUOFSEAoEGyc%3D"

Which you can then download directly to your computer using ``wget``, ``curl``,
or any other standard tool.


Restoring From a Backup
***********************

While backing up your database is always a good idea, your backups will do you
no good unless you know how to restore from the backup when things go wrong.

Assuming you have the following backup available:

.. code-block:: console

    $ heroku pgbackups
    ID   | Backup Time         | Size       | Database
    -----+---------------------+------------+----------------
    b001 | 2012/04/20 23:09.50 | 918.0bytes | SHARED_DATABASE

You can restore your backup (``b001`` in this case) to *any* of your available
databases using the ``pgbackups:restore`` command:

.. code-block:: console

    $ heroku pgbackups:restore SHARED_DATABASE b001

    SHARED_DATABASE (DATABASE_URL)  <---restore---  b001
                                                    SHARED_DATABASE
                                                    2012/04/20 23:09.50
                                                    918.0bytes

     !    WARNING: Potentially Destructive Action
     !    This command will affect the app: deploydjango
     !    To proceed, type "deploydjango" or re-run this command with --confirm deploydjango

    > deploydjango

    Retrieving... done
    Restoring... done

.. note::
    The restoration process make take a few minutes if your backups are large.
