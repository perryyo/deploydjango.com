Advanced
--------

This section contains some slightly more advanced topics that didn't fit into
the Setup section. Feel free to skip around to sections below that interest you.


Adding Collaborators to Your Heroku Application
***********************************************

At some point, you'll most likely want to give another developer access to your
project. Coding by yourself is great and all, but two heads are (often) better
than one :)

To add a collaborator to your project, you can use the ``sharing:add`` command:

.. code-block:: console

    $ heroku sharing:add yourfriend@blah.com
    yourfriend@blah.com added as a collaborator on deploydjango.

Collaborators will be able to push new code to your application, but won't be
able to add new infrastructure components. For example: if I'm a collaborator
on your application, I can push new features live, but I can't add a new
PostgreSQL database (only the account owner can do that).


Removing Collaborators
**********************

If you need to remove a collaborator from your account (maybe you and your
co-founder weren't getting along), you can use the ``sharing:remove`` command
to immediately revoke their access:

.. code-block:: console

    $ heroku sharing:remove yourfriend@blah.com
    Collaborator removed.


Destroying Applications
***********************

Sometimes you'll want to get rid of an application that already exists. Sure,
you can do this through Heroku's `web interface
<https://api.heroku.com/myapps>`_, but that's no fun.

To destroy an application via the command line, you can run the
``apps:destroy`` command:

.. code-block:: console

    $ heroku apps:destroy deploydjango

     !    WARNING: Potentially Destructive Action
     !    This command will destroy deploydjango (including all add-ons).
     !    To proceed, type "deploydjango" or re-run this command with --confirm deploydjango

    > deploydjango
    Destroying deploydjango (including all add-ons)... done
