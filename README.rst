===================
Wagtail FontAwesome
===================
.. image:: https://img.shields.io/pypi/l/wagtailfontawesome.svg
    :target: https://gitlab.com/alexgleason/wagtailfontawesome/blob/master/LICENSE

Add `FontAwesome 4.7 <https://fontawesome.com/v4.7.0/>`_ icons to your Wagtail project.

.. image:: https://gitlab.com/alexgleason/wagtailfontawesome/raw/master/screenshot.png
    :alt: Screenshot

Install
=======

.. code-block:: console

    pip install wagtailfontawesome


Then add ``wagtailfontawesome`` to your installed apps.

Usage
=====

StreamField
-----------

Add FontAwesome icons to StreamField `the regular way <http://docs.wagtail.io/en/latest/topics/streamfield.html#basic-block-types>`_, just set `icon="fa-something"`. Reference `the full list <https://fontawesome.com/v4.7.0/icons/>`_.

For example, using ``fa-exclamation-triangle`` on a block that's a class:

.. code-block:: python

    class NoticeBlock(StructBlock):
        message = RichTextBlock()
        indicator = ChoiceBlock()

        class Meta:
            icon = 'fa-exclamation-triangle'

The same block, but inline:

.. code-block:: python

    notice = StructBlock([
      ('message', RichTextBlock()),
      ('indicator', ChoiceBlock())
    ], icon='fa-exclamation-triangle')

Using IconBlock
---------------

Wagtail FontAwesome contains a dropdown chooser you can use to select a block from the available options. For example,

.. code-block:: python

    from wagtailfontawesome.blocks import IconBlock

    class CardBlock(StructBlock):
        icon = IconBlock()
        title = CharBlock()

You are responsible for including the FontAwesome CSS yourself somewhere on the page. See the section "On the front-end" below for a way to do that with Wagtail FontAwesome.

ModelAdmin
-----------------

`ModelAdmin <http://docs.wagtail.io/en/latest/reference/contrib/modeladmin/>`_ is supported if you're using Wagtail 1.5 or above. Similar to StreamField, just set `icon="fa-something"` on your menu item.

Other parts of the admin
------------------------

You can include icons anywhere in the admin with:

.. code-block:: html+django

    <i class="icon icon-fa-something"></i>

In Wagtail 1.3.x and below you can only use icons on the page editor screen.

On the front-end
----------------

You can also include the CSS on the front end, and follow FontAwesome's documentation.

.. code-block:: html+django

    {% load wagtailfontawesome %}

    {% fontawesome_css %}

This will generate equivalent markup to:

.. code-block:: html+django

    <link rel="stylesheet" href="{% static 'wagtailfontawesome/css/fontawesome.css' %}">

Then include icons anywhere on the front-end with:

.. code-block:: html+django

    <i class="fa fa-something"></i>

Using wagtailfontawesome as an optional dependency
--------------------------------------------------

If you want to distribute a Wagtail plugin with FontAwesome icons, you can use this package as an optional dependency by checking if it's installed in Django, and falling back otherwise.

.. code-block:: python

    from django.apps import apps
    try:
        from wagtail.core.blocks import StructBlock
    except ImportError:  # fallback for Wagtail <2.0
        from wagtail.wagtailcore.blocks import StructBlock


    class BlockquoteBlock(StructBlock):
        quote = TextBlock()
        author = TextBlock()

        class Meta:
            if apps.is_installed('wagtailfontawesome'):
                icon = 'fa-quote-left'

(in this case, the fallback is to do nothing)
