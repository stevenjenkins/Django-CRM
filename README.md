# Django-CRM

This is a fork of [Django-CRM](https://github.com/MicroPyramid/Django-CRM), to do the following:

* Enhance installation instructions in general
* Add MacOS instructions
* Add admin support (go to <http:127.0.0.1:8000/admin>)

<http://django-crm.readthedocs.io> for latest documentation

# TODOs

* [ ] Fix and document token authentication
* [ ] Enhance to use either session or token authentication for the `api/` namespace
* [ ] Detailed walkthrough of the `env.md`setup
* [ ] Detailed description of the components needed (whether for stand-alone or Docker)
* [ ] JWT conversion to supported JWT (or other token-management)

# Enhancements

## `admin/` support

Django Admin support has been enabled.  The upstream codebase had some support (e.g., the
`*/admin.py` files), but those were not exposed.  My fork exposes those via the standard
mechanisms.

## show_urls

I find it convenient to be able to see the routes, and added support for the following:
```
  python manage.py show_urls
```

# Installation Notes

* SASS: the official documentation says to install ruby, ruby-dev, and gem.  However, you do
  not need that if you install SASS in another way (e.g., `npm install sass`)

* There is no `python manage.py create_blog_user` task; however, the standard 
  `python manage.py createsuperuser` is enabled.


# Usage Notes

Note in `crm/urls.py` that there is swagger, and `api/` support; however, the UI of Django-CRM
is not in the codebase.

