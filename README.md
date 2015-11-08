# django-breadcrumbs3

This app provides support for implementing breadcrumbs in Django's views. It
allows you to specify the breadcrumbs in your views, and then render them with
one line in your templates. It saves you from having to code ugly and repeated
markup for each and every page.

A full test suite is provided and it works with:

* Python 2.7, 3.3\*, 3.4, 3.5\*
* Django 1.7\*, 1.8, 1.9b1\*

\* Note: Python 3.3 is unsupported on Django 1.9b1, and Python 3.5 is unsupported on Django 1.7.

[![Build Status](https://travis-ci.org/sjkingo/django-breadcrumbs3.svg)](https://travis-ci.org/sjkingo/django-breadcrumbs3)

## Installation

1. Install from [PyPi](https://pypi.python.org/pypi/django-breadcrumbs3):
   
   ```
   $ pip install django-breadcrumbs3
   ```

2. Add `breadcrumbs3` to your `INSTALLED_APPS` setting (this is to provide access to the template tag):

   ```python
   INSTALLED_APPS = (
       ...
       'breadcrumbs3',
   )
   ```

3. Make sure the Django `request` context processor is added to the settings:

   * Django 1.7:
   
     ```python
     TEMPLATE_CONTEXT_PROCESSORS = (
         ...
         'django.core.context_processors.request',
     )
     ```

   * Django 1.8 and higher ([docs](https://docs.djangoproject.com/en/1.8/ref/templates/upgrading/#the-templates-settings)):

     ```python
     TEMPLATES = [
         {
             'OPTIONS': {
                 'context_processors': [
                     ...
                     'django.template.context_processors.request',
                 ],
                 ...
             },
             ...
         },
     ]
     ```
   
4. Add the breadcrumbs3 middleware:

   ```python
   MIDDLEWARE_CLASSES = [
       ...
       'breadcrumbs3.middleware.BreadcrumbMiddleware',
   ]
   ```

## Testing

You can run the tests through Django's test runner:

```
$ python manage.py test breadcrumbs3
```

## Using breadcrumbs3

There are two parts to generating breadcrumbs: adding each *crumb* in your
view, and rendering the breadcrumb list in a template.

An example view might be:

```python
def some_view(request):
    request.breadcrumbs('Some view title', request.path_info)
    ...
```

There are a few options for calling the `request.breadcrumbs` method:

* The first argument is always required and specifies the title.
* The second argument is optional, and if given is a URL to link
  this crumb to. You may like to use [`reverse`](https://docs.djangoproject.com/en/stable/ref/urlresolvers/#reverse) so
  you don't have to hardcode any URLs.
* If the second argument is None, no link will be provided for this crumb.

You can call `request.breadcrumbs()` as many times as needed. The order will be
preserved when the breadcrumbs are rendered.

You can then render the breadcrumbs using the `breadcrumbs` template tag:

`templates/some_template.html`:

```html
{% load breadcrumbs %}

<div class="breadcrumbs">
    {% breadcrumbs %}
</div>
```

Breadcrumbs are rendered as a list like so:

```html
<ul class="breadcrumbs-list">
    <li class="breadcrumb">...</li>
    ...
</ul>
```
