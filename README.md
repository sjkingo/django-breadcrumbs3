# django-breadcrumbs3

A small app to provide ready-to-go breadcrumb support in Django by removing
the generation of breadcrumbs out of the templates.

It has been tested on Django 1.8 and Python 3.4, but should work on lower
versions and Pytrhon 2.7 as well.

## Installation

1. `$ pip install django-breadcrumbs3`
2. Add `breadcrumbs3` to your `INSTALLED_APPS` setting (this is to provide access to the template tag).
3. Make sure `django.template.context_processors.request` is present in your
   `TEMPLATE_CONTEXT_PROCESSORS` (pre-Django 1.8) or `TEMPLATES.OPTIONS.context_processors` (1.8+) setting.
4. Add `breadcrumbs3.middleware.BreadcrumbMiddleware` to `MIDDLEWARE_CLASSES`.

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
