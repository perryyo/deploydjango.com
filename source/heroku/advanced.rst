Advanced
--------

This section contains some slightly more advanced topics that didn't fit into
the Setup section. Feel free to skip around to sections below that interest you.


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
