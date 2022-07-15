API (Under Design)
======

Current draft design of api v0

.. contents:: Table of contents
   :local:
   :backlinks: none
   :depth: 2


User Authentication
--------------------------------

APIs about user sign in/up activity.


Create Session Login Token
~~~~~~~~


.. http:post:: /api/v1/login/auth

    Trigger a new build for the ``version_slug`` version of this project.

    **Example request**:

     .. sourcecode:: bash

         $ curl \
           -X POST \
           -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/versions/latest/builds/


    **Example response**:

    .. sourcecode:: json

        {
            "build": "{BUILD}",
            "project": "{PROJECT}",
            "version": "{VERSION}"
        }

    :statuscode 202: the build was triggered


Create Session Via API Token
~~~~~~~~


.. http:post:: /api/v1/login/auth

    Trigger a new build for the ``version_slug`` version of this project.

    **Example request**:

     .. sourcecode:: bash

         $ curl \
           -X POST \
           -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/versions/latest/builds/


    **Example response**:

    .. sourcecode:: json

        {
            "build": "{BUILD}",
            "project": "{PROJECT}",
            "version": "{VERSION}"
        }

    :statuscode 202: the build was triggered



Create User
~~~~~~~~

.. http:post:: /api/v1/users

    Update a redirect for this project.

    **Example request**:

     .. sourcecode:: bash

         $ curl \
           -X PUT \
           -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/redirects/1/ \
           -H "Content-Type: application/json" \
           -d @body.json

    The content of ``body.json`` is like,

    .. sourcecode:: json

        {
            "from_url": "/docs/",
            "to_url": "/documentation.html",
            "type": "page"
        }

    **Example response**:

    `See Redirect details <#redirect-details>`_


.. note::

   This is an example note for future use.



Chat Messages
---------

This section shows all the apis about chat messages that are currently available in APIv3.



Post a message
~~~~~~~~

.. http:post:: /api/v1/messages

    Post a message into the chat room.

    **Example request**:

     .. sourcecode:: bash

         $ curl \
           -X PUT \
           -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/redirects/1/ \
           -H "Content-Type: application/json" \
           -d @body.json

    The content of ``body.json`` is like,

    .. sourcecode:: json

        {
            "from_url": "/docs/",
            "to_url": "/documentation.html",
            "type": "page"
        }

    **Example response**:

    `See Redirect details <#redirect-details>`_


.. note::

   This is an example note for future use.



Messages listing
~~~~~~~~

.. http:get:: /api/v3/messages

    Retrieve a list of all versions for a project.

    **Example request**:

    .. sourcecode:: bash

        $ curl -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/versions/


    **Example response**:

    .. sourcecode:: json

        {
            "count": 25,
            "next": "/api/v3/projects/pip/versions/?limit=10&offset=10",
            "previous": null,
            "results": ["VERSION"]
        }

    :query boolean active: return only active versions
    :query boolean built: return only built versions
    :query string privacy_level: return versions with specific privacy level (``public`` or ``private``)
    :query string slug: return versions with matching slug
    :query string type: return versions with specific type (``branch`` or ``tag``)
    :query string verbose_name: return versions with matching version name




