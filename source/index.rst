Deploy Django
=============

*Useful patterns for deploying Django on Heroku.*

`Deploying Django <http://rdegges.com/deploying-django>`_ is hard work. No
matter how much experience you have, deployment is always a difficult task as
it involves so many factors. Among other things, you have to:

- Provision the proper infrastructure services (a database, caching server,
  search server, etc.).

- Ensure your site properly works with your infrastructure services in
  development **and** production environments.

- Know how to manage and maintain your various infrastructure services.

- Know how to monitor your site performance so you can find and fix issues
  quickly.

- Know how to scale your infrastructure services as load grows.

- Have a reliable way to quickly deploy and rollback site updates without
  causing downtime for your users.

Luckily for us, `Heroku <http://www.heroku.com/>`_ has built an incredibly
simple, elegant, and scalable platform that makes deploying your Django
websites as painless as possible.

The goal of this site is to **help you** get up and running as quickly as
possible, while teaching you the basics along the way. Our hope is that you'll
find this site useful, and that it will provide you with enough information to
not only deploy your Django sites, but also to maintain, grow, and scale them
to support millions of users.

.. note::
    This still is still under rapid development. Please send any suggestions
    (or requests) to rdegges@gmail.com.

Topics:

.. toctree::
    :maxdepth: 2

    postgresql/index.rst
