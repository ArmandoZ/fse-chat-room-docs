API (Under Design)
======

Current draft design of api v0

.. contents:: Table of contents
   :local:
   :backlinks: none
   :depth: 2


Authentication and authorization
--------------------------------

Requests to the Read the Docs public API are for public and private information.
All endpoints require authentication.


Token
~~~~~

The ``Authorization`` HTTP header can be specified with ``Token <your-access-token>``
to authenticate as a user and have the same permissions that the user itself.


Resources
---------

This section shows all the resources that are currently available in APIv3.
There are some URL attributes that applies to all of these resources:

:?fields=:

   Specify which fields are going to be returned in the response.

:?omit=:

   Specify which fields are going to be omitted from the response.

:?expand=:

   Some resources allow to expand/add extra fields on their responses (see `Project details <#project-details>`__ for example).


.. note::

   If you are using :doc:`Read the Docs for Business </commercial/index>` take into account that you will need to replace
   https://readthedocs.org/ by https://readthedocs.com/ in all the URLs used in the following examples.


Projects
~~~~~~~~


Build triggering
++++++++++++++++


.. http:post:: /api/v3/projects/(string:project_slug)/versions/(string:version_slug)/builds/

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


Redirect update
+++++++++++++++

.. http:put:: /api/v3/projects/(str:project_slug)/redirects/(int:redirect_id)/

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



Project details
+++++++++++++++

.. http:get:: /api/v3/projects/(string:project_slug)/

    Retrieve details of a single project.

    **Example request**:


    .. sourcecode:: bash

       $ curl -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/


    **Example response**:

    .. sourcecode:: json

        {
            "id": 12345,
            "name": "Pip",
            "slug": "pip",
            "created": "2010-10-23T18:12:31+00:00",
            "modified": "2018-12-11T07:21:11+00:00",
            "language": {
                "code": "en",
                "name": "English"
            },
            "programming_language": {
                "code": "py",
                "name": "Python"
            },
            "repository": {
                "url": "https://github.com/pypa/pip",
                "type": "git"
            },
            "default_version": "stable",
            "default_branch": "master",
            "subproject_of": null,
            "translation_of": null,
            "urls": {
                "documentation": "http://pip.pypa.io/en/stable/",
                "home": "https://pip.pypa.io/"
            },
            "tags": [
                "distutils",
                "easy_install",
                "egg",
                "setuptools",
                "virtualenv"
            ],
            "users": [
                {
                    "username": "dstufft"
                }
            ],
            "active_versions": {
                "stable": "{VERSION}",
                "latest": "{VERSION}",
                "19.0.2": "{VERSION}"
            },
            "_links": {
                "_self": "/api/v3/projects/pip/",
                "versions": "/api/v3/projects/pip/versions/",
                "builds": "/api/v3/projects/pip/builds/",
                "subprojects": "/api/v3/projects/pip/subprojects/",
                "superproject": "/api/v3/projects/pip/superproject/",
                "redirects": "/api/v3/projects/pip/redirects/",
                "translations": "/api/v3/projects/pip/translations/"
            }
        }

    :query string expand: allows to add/expand some extra fields in the response.
                          Allowed values are ``active_versions``, ``active_versions.last_build`` and
                          ``active_versions.last_build.config``. Multiple fields can be passed separated by commas.

    .. note::

       .. FIXME: we can't use :query string: here because it doesn't render properly

      :doc:`Read the Docs for Business </commercial/index>`, also accepts

      :Query Parameters:

         * **expand** (*string*) -- with ``organization`` and ``teams``.



Versions listing
++++++++++++++++

.. http:get:: /api/v3/projects/(string:project_slug)/versions/

    Retrieve a list of all versions for a project.

    **Example request**:

    .. tabs::

        .. code-tab:: bash

            $ curl -H "Authorization: Token <token>" https://readthedocs.org/api/v3/projects/pip/versions/

        .. code-tab:: python

            import requests
            URL = 'https://readthedocs.org/api/v3/projects/pip/versions/'
            TOKEN = '<token>'
            HEADERS = {'Authorization': f'token {TOKEN}'}
            response = requests.get(URL, headers=HEADERS)
            print(response.json())

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




