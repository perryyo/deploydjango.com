Setup
-----

Getting started with Heroku is simple. There are four major things you'll need
to do:

1. Create an account on http://www.heroku.com/.
2. Install the `Heroku toolbelt <https://toolbelt.heroku.com/>`_.
3. Install the `heroku-accounts <https://github.com/ddollar/heroku-accounts>`_
   plugin.
4. Have a Django 1.4 site (version controlled with Git) that you'd like to
   deploy to Heroku.

Through the rest of this site--you'll be expected to have the above working and
ready to go. I'll walk you through steps 2 and 3, but steps 1 and 4 are up to
you.

Before you go any further, sign up for an account on Heroku:
https://api.heroku.com/signup.


Installing the Heroku Toolbelt
******************************

The `Heroku toolbelt <https://toolbelt.heroku.com/>`_ is a set of software
Heroku officially supports which provides:

- The ``heroku`` command line utility.
- The ``foreman`` command line utility.
- The ``git`` command line utility.

The ``heroku`` CLI app is a great tool (maintained by Heroku) that allows you
to fully manage your Heroku applications from the command line. You can use it
to:

- Create new Heroku applications.
- Scale web applications.
- Add infrastructure components.
- Resize infrastructure components.
- Remove infrastructure components.
- etc.

To get started, visit: https://toolbelt.heroku.com and follow their
installation instructions for your specific system.

.. note::
    Don't create any applications or anything else just yet--we'll get to that
    in a bit.


Installing heroku-accounts - The Best Tool for Managing Multiple Herkou Accounts
********************************************************************************

We're all programmers here, so I'm going to assume that you probably work on
code both for fun, as well as for profit. Seeing as how this is most likely the
case, this step will make your life much easier later on.

The `heroku-accounts plugin <https://github.com/ddollar/heroku-accounts>`_
allows you to easily switch between Heroku accounts on the command line. This
will allow you to do things like:

- Create new Heroku applications on your work account.
- Work on private applications on your personal account.
- Add new Heroku accounts in a flash.
- Easily switch back and fourth amongst your accounts.
- Handle multiple SSH keys for your accounts *almost* transparently.

To install ``heroku-accounts``, simply run the following command:

.. code-block:: console

    $ heroku plugins:install git://github.com/ddollar/heroku-accounts.git
    heroku-accounts installed

Once you've got ``heroku-accounts`` installed, you should be able to run
``heroku help accounts`` to see the usage information:

.. code-block:: console

    $ heroku help accounts
    Usage: heroku accounts

     list all known accounts

    Additional commands, type "heroku help COMMAND" for more details:

      accounts:add      #  accounts:add
      accounts:default  #  accounts:default
      accounts:remove   #  accounts:remove
      accounts:set      #  accounts:set

As you can see, ``heroku-accounts`` provides several useful commands, ``add``,
``default``, ``remove``, and ``set``.

Let's quickly add our personal account:

.. code-block:: console

    $ heroku accounts:add rdegges --auto
    Enter your Heroku credentials.
    Email: rdegges@gmail.com
    Password (typing will be hidden):
    Generating new SSH key
    Generating public/private rsa key pair.
    Created directory '/root/.ssh'.
    Your identification has been saved in /root/.ssh/identity.heroku.rdegges.
    Your public key has been saved in /root/.ssh/identity.heroku.rdegges.pub.
    The key fingerprint is:
    c3:b5:59:7f:4b:b5:c7:fb:59:eb:c6:c8:af:ac:7f:da root@rtest
    The key's randomart image is:
    +--[ RSA 2048]----+
    |                 |
    |                 |
    |          . .   .|
    |       . . + . .o|
    |        S o   .o+|
    |         .    ..+|
    |            . oo.|
    |            .o.+=|
    |           .o=BE.|
    +-----------------+
    Adding entry to ~/.ssh/config
    Adding public key to Heroku account: rdegges@gmail.com

What happened here was that I created a new Heroku account (locally), and gave
it the name ``rdegges``. Since this is my personal account, naming it
``rdegges`` makes sense for me. If you've got multiple Heroku accounts, create
them now.

.. note::
    When you add an account using ``heroku-accounts``, ``heroku-accounts`` will
    automatically generate and upload a new SSH key to your Heroku account.
    This way, you'll be able to push code to any applications you may have created.
