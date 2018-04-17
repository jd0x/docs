=========================
Many-to-one relationships
=========================

In a many-to-one relationship one entity contains values that refer to another entity that has unique values.  In the Hasura data APIs, many-to-one relationships are referred to as ``object relationships``.

For example, in an ``article`` ``author`` schema, multiple ``articles`` can have the same unique ``author``.

Creating a many-to-one relationship
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add a many-to-one relationship from ``article`` to ``author``,

#. :doc:`Create a foreign key constraint on the "article" table <adding-foreign-key-constraint>`.
#. Open the API console and go to the ``article`` table.
#. Open the relationships tab.
#. Add the suggested object relationship and give it a desired name.

.. image:: many-to-one-rel.png

Fetching over a many-to-one relationship
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To fetch the list of all ``articles`` along with the name of each of their ``author``, use a query like so:

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json
   Authorization: Bearer <auth-token> # optional if cookie is set
   X-Hasura-Role: <role>  # optional. Required if request needs particular user role

   

.. rst-class:: api_tabs
.. tabs::

   .. tab:: GraphQL

      .. code-block:: none

         query fetch_article {
            article {
                title
                author {
                   name 
                }
            }
         } 

   .. tab:: JSON API

      .. code-block:: http
        :emphasize-lines: 14

        POST data.<cluster-name>.hasura-app.io/v1/query HTTP/1.1
        Content-Type: application/json
        Authorization: Bearer <auth-token> # optional if cookie is set
        X-Hasura-Role: admin

        {
            "type" : "select",
            "args" : {
                "table" : "article",
                "columns": [
                    "title",
                    {
                        "name": "author",
                        "columns": ["name"]
                    }
                ]
            }
        }