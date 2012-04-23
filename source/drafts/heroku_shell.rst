Accessing Your Remote Shell
***************************

One of the greatest things about Heroku is that their platform is completely
transparent. This means that you can easily inspect the way things work, and
not worry about ever being *locked out* of your application.

There are certain times, for example, when it would be incredibly useful to
connect to your Heroku enviornment remotely (via a bash shell, let's say) to
take a look what your application environment and get a feel for how things work.

Using the ``run`` command, you can run any command remotely in an isolated
environment. This means, for instance, that you can do the following:

.. code-block:: console

    $ heroku run ls
    Running ls attached to terminal... up, run.1
    Makefile  Procfile  requirements.txt  source

    $ heroku run bash
    Running bash attached to terminal... up, run.1
    ~ $ cd source/
    ~/source $ ls
    conf.py  heroku  index.rst  postgresql  quickstart.rst  _static  _templates

What I just did was first run a remote ``ls`` command to list the files in the
root of my application directory (for my ``deploydjango`` project) on Heroku!
Since I've already got some stuff there (
