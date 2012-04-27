Deploy Django
=============

*The definitive guide to deploying Django applications on Heroku.*


Welcome
-------

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
possible, while teaching you the basics along the way. My hope is that you'll
find this site useful, and that it will provide you with enough information to
not only deploy your Django sites, but also to maintain, grow, and scale them
to support millions of users.

.. note::
    This site is still under rapid development. Please send any suggestions
    (or requests) to rdegges@gmail.com.


Who This Book is For
--------------------

Unfortunately, since deployment is such a broad topic, this book isn't suited
for everyone. To keep things reasonably simple throughout this book, I'm going
to make the following assumptions:

- You are familiar with Django and Python. You don't have to be great, but you
  should be familiar with them, and have written at least one Django website
  before.

- You are comfortable using the command line, and you've used Linux before. I'm
  not going to cover any GUI tools in this book--we'll be dealing stricly with
  command line stuff.

- You are willing to get your hands dirty--the best way to learn about
  deployment is to actually deploy something you've written. While this book
  has a lot of great information, it will be useless to you unless you follow
  along with the examples as we go :)

If you're a new Python or Django programmer, and you haven't yet written a
website of your own, please read through the following books (in order) and
then return to this one, as you'll be a whole lot more knowledgeable and will
be able to fully understand the concepts explained here:

- `Learn Python the Hard Way <http://learnpythonthehardway.org/>`_
- `Django 1.0 Web Site Development <http://www.amazon.com/gp/product/1847196780/ref=as_li_ss_tl?ie=UTF8&tag=rdegges-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=1847196780>`_
- `Practical Django Projects <http://www.amazon.com/gp/product/1430219386/ref=as_li_ss_tl?ie=UTF8&tag=rdegges-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=1430219386>`_
- `Pro Django <http://www.amazon.com/gp/product/1430210478/ref=as_li_ss_tl?ie=UTF8&tag=rdegges-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=1430210478>`_
- `Django 1.1 Testing and Debugging <http://www.amazon.com/gp/product/1847197566/ref=as_li_ss_tl?ie=UTF8&tag=rdegges-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=1847197566>`_

Reading through those books in order is enough to take any programmer
(regardless of skillset) from beginner to pro.

Now that we've got that out of the way, let's deploy some Django!


Topics
------

.. toctree::
    :maxdepth: 3

    heroku/index.rst
    django_project_structure/index.rst
    postgresql/index.rst
