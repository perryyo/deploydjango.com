Heroku Django Template
######################

With Django's 1.4 Release there is support for project templates. This allows 
users to easily create templates structured towards their preferred project 
styles and make bootstrapping even faster. With this support we've created a 
Django specific template which includes many of the other references you'll find
here. This should allow you to immediately create a project and push it to 
Heroku. 

Quickstart
----------

.. code-block:: console

    django-admin.py startproject --template=https://github.com/craigkerstiens/heroku-template/zipball/master -n Procfile project_name

Now you should be able to immediately take your application and create a heroku and
deploy it. 

.. code-block:: console

    heroku create -s cedar
    git push heroku master
    heroku open

Or you can run your new project locally

.. code-block:: console

    foreman start


