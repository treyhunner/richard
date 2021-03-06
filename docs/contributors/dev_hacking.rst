=========================================
 Installing, running and testing richard
=========================================

This covers how to clone richard and set it up for easy hacking.

.. Note::

   richard is pretty new and is under heavy development. As such, the
   documentation for it sucks and the installation guide may have as
   much of a chance of helping you install richard as it does helping
   you make a quiche.

   I'm really sorry about that, but I'm still bootstrapping the
   project.


.. contents::
   :local:


richard requires a bunch of stuff to run. I'm going to talk about this
stuff in two groups:

1. stuff that you should install with your package manager
2. Python packages that should get installed in a virtual environment


Install things with your package manager
========================================

You need the following things all of which should be provided by your
system/package manager:

* Python 2.7
* pip
* virtualenv


On Debian, this translates to::

    $ apt-get install \
          python \
          python-pip \
          python-virtualenv


Get richard
===========

Clone the repository from github::

    $ git clone git://github.com/willkg/richard.git


Install Python requirements
===========================

Now you need to install some other things all of which are specified
in the requirements files provided.

Create a virtual environment::

    $ cd richard
    $ virtualenv ./venv/


Use pip to install the development requirements::

    $ ./venv/bin/pip install -r requirements/development.txt


Make sure to activate the virtual environment every time you go to use
richard things. You can do that like this::

    $ ./venv/bin/activate


.. Note::

   If you want to use virtualenvwrapper or want to set things up differently,
   feel free to do so!


Configure
=========

You will need to override some of those settings for your
instance. To do that:

1. ``cp richard/settings_local.py-dist richard/settings_local.py``
2. Edit ``richard/settings_local.py``


Make sure to do at least the following:

1. Set a ``SECRET_KEY``. Make it unique! Don't share it with anyone!

TODO: Finish this up


Set up the database
===================

sqlite3
-------

If you're a contributor and not working on db-related bits, then using
sqlite3 might work fine. It's certainly the easiest to set up

Uncomment the sqlite3 bits in ``richard/settings_local.py`` and
comment out the postgresql bits.


postgresql
----------

If you're working on db-related bits or have postgres set up already,
then do the following:

1. ``pip install psycopg2``
2. set up a postgres database and add the relevant configuration bits
   to the ``richard/settings_local.py`` file.


Set up database schema and create admin user
============================================

To set up the database schema and create the admin user, run::

    $ ./manage.py syncdb --migrate

The admin user account you create here can be used to log into the richard
admin section.


Set up sample data (optional)
=============================

If you want to set up some initial data, do::

    $ ./manage.py generatedata

This is useful to see how the site works.


Run the tests
=============

Richard uses ``django-nose`` to discover tests.

Activate the virtual environment, then run the tests::

    $ ./manage.py test --nologcapture --nocapture


Run the server
==============

Run the server like this::

    $ ./manage.py runserver --traceback


Then point your browser at ``http://localhost:8000/``.


Troubleshooting
===============

I can't log in
--------------

First, make sure your administrator account has an email address
associated with it. This is the email address you will log in with
Persona.

Second, if you're seeing a "Misconfigured" kind of error, make sure
the ``SITE_URL`` in your ``settings_local.py`` file matches the domain
and port that the server is running on. If it doesn't match, then
django-browserid won't work.

See `the django-browserid troubleshooting docs
<https://django-browserid.readthedocs.org/en/latest/details/troubleshooting.html>`_
for more details.
