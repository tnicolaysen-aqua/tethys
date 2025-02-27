.. _recipes_button_gizmo:

************
Add a Button
************

**Last Updated:** February 2025

This is how to add a button to your app.

.. code-block:: python

    from tethys_sdk.gizmos import Button

    # Single Buttons
    map_button = Button(
        display_text='',
        name='map-button',
        icon='globe',
        style='info',
        attributes={
            'data-toggle':'tooltip',
            'data-placement':'top',
            'title':'Map'
        }
    )

.. tip::

    For more details on how to use button gizmos see :ref:`gizmo_button`

Heading 1
---------

this is a list

* step 1
* step 2

Heading 2
=========

Numbered list

1. Item
2. Item

Heading 3
/////////

.. figure:: ../../images/tutorial/gee/gee_intro.png
    :width: 800px
    :align: center

