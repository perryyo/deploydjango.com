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


Installing Heroku Accounts - The Best Tool for Managing Multiple Herkou Accounts
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
