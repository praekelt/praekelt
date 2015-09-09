Our Platforms
=============

Praekelt uses two main platforms for the bulk of engineering work:

1. Django and Jmbo

   We use Django for websites, mobi sites, responsive sites, mobi HTML5 apps. Jmbo
   is our own lightweight publishing framework built on top of Django.

2. Twisted

   Projects that require large concurrency work better in a Twisted environment.

Use of any other platform must be approved by our engineering management, and may
need to be hosted separately from our usual environments, so please get this
sorted out prior to commencing development on a project.

Django and Jmbo
---------------

You should use the latest stable release of Jmbo and / or Django *as of the start of the project*
unless otherwise specified. Pin that version in your requirements so that the
project won't break by accidently being deployed on a newer version without
testing.

You must use jmbo-skeleton_ as a starting point for
Django projects, as it has the layout and configurations we use, so that deployment
will be smooth.


We deploy Django in the following stack:

- Ubuntu Server (current LTS release)
- nginx_
- gunicorn_
- supervisord_
- postgresql_
- device-proxy_

You may simplify this for development but we recommend you keep your
dev environment as close to this as you can.

Notes:

- We manage hostnames in nginx because there may be multiple QA and live hostnames,
  so don't use Django's ALLOWED_HOSTS.
- Make use of pip and virtualenv.
- There are specific requirements for logging. Please see the Logging_ section
  for more information.

.. _django-skeleton: https://github.com/praekelt/jmbo-skeleton/#jmbo-skeleton
.. _nginx: http://nginx.org/
.. _gunicorn: http://gunicorn.org/
.. _supervisord: http://supervisord.org/
.. _postgresql: http://www.postgresql.org/
.. _device-proxy: https://github.com/praekelt/device-proxy/#device-proxy

Twisted
----

Twisted_ is a scalable...

.. _Twisted: http://twistedmatrix.org/

Logging
-------

All logging must happen to a standardized path, under */var/praekelt/log*. If your app
has a large amount of files (celery, worker, and app, for instance), write each of
these to a named path under */var/praekelt/log/appname/foo.log*, otherwise just 
*/var/praekelt/log/foo.log* is sufficient.

Logs always end with the extension *.log*. *.err* is not valid. If you need to write out
error logs, either use the name *foo-error.log*, or write your supervisord_ configuration
with the *redirect_stderr* option.

The basis for this requirement is to ease debugging (hunting logs in 7 different
directories is never fun, and causes issues when under time pressure), simplifies log
rotation, and allows them to easily fit within our automatic log collection and
indexing system.
