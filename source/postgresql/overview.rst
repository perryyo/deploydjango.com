Overview
--------

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
  service is a standalone service and works just like any normal PostgreSQL
  server, so you don't need to make any application changes.
