=======================
One-to-one relationship
=======================

One-to-One (1-1) relationship is defined as the relationship between two tables where both the tables should be associated with each other based on only one matching row.

Lets consider two sample tables: ``person`` and ``voter``

``person``

+---------+-----------------------------+
| column  | type                        |
+=========+=============================+
| id      | serial NOT NULL primary key |
+---------+-----------------------------+
| name    | text NOT NULL               |
+---------+-----------------------------+
| age     | int4 NOT NULL               |
+---------+-----------------------------+
| gender  | text NOT NULL               |
+---------+-----------------------------+


``voter``

+-----------+-----------------------------+
| column    | type                        |
+===========+=============================+
| voter_id  | serial NOT NULL primary key |
+-----------+-----------------------------+
| person_id | int4 NOT NULL               |
+-----------+-----------------------------+


In the above schema, a ``person`` can have only one ``voter_id`` and every ``voter_id`` can identify only one person.

Lets add a one-to-one relationship between the ``person`` and ``voter`` tables.

Creating a one-to-one relationship
----------------------------------

To create a one-to-one relationship, we will add a uniqueness constraint over a column each in both tables and then add a relationship using SQL. In this case, we will add a uniqueness constraint over the ``person_id`` column in the ``voter`` table. In the case of ``person`` table, ``id`` is a primary key and therefore already unique.



#. Open the API-Console

#. Add uniqueness constraint:

   #. Choose the ``SQL`` section in the left panel.
   #. Add the following SQL command to create a uniqueness constraint on the ``person_id`` column of the ``voter`` table.

      .. code-block:: sql

         CREATE UNIQUE INDEX ON voter (person_id)
  
   #. Check the ``This is a migration`` checkbox so that a :doc:`migration <../data-migration>` is created in the ``/migrations`` directory.
   #. Hit ``Run``.

#. Create an :doc:`object relationship <../../api-reference/data/query/relationship>` from ``person`` to ``voter``.

   #. Go to the ``API-Explorer`` section.
   #. Choose ``JSON Raw`` on the left panel.
   #. Make the following query to create the relationship using the admin token:

      .. code-block:: http

        POST data.<cluster-name>.hasura-app.io/v1/query HTTP/1.1
        Content-Type: application/json
        Authorization: Bearer <auth-token>
        X-Hasura-Role: admin

    
        {
            "type": "create_object_relationship",
            "args": {
                "table": "person",
                "name": "voter_info",
                "using": {
                    "manual_configuration" : {
                        "remote_table" : "voter",
                        "column_mapping" : {
                            "id" : "person_id"
                        }
                    }
                }
            }
        }

#. Similarly create an :doc:`object relationship <../../api-reference/data/query/relationship>` from ``voter`` to ``person``.

   #. Go to the ``API-Explorer`` section.
   #. Choose ``JSON Raw`` on the left panel.
   #. Make the following query to create the relationship using the admin token:

      .. code-block:: http

        POST data.<cluster-name>.hasura-app.io/v1/query HTTP/1.1
        Content-Type: application/json
        Authorization: Bearer <auth-token>
        X-Hasura-Role: admin

        {
            "type": "create_object_relationship",
            "args": {
                "table": "voter",
                "name": "person_info",
                "using": {
                    "manual_configuration" : {
                        "remote_table" : "person",
                        "column_mapping" : {
                            "person_id" : "id"
                        }
                    }
                }
            }
        }


Fetching over a one-to-one relationship
---------------------------------------

To fetch the list of all entries from the ``person`` table along with their ``voter_id``:

.. rst-class:: api_tabs
.. tabs::

   .. tab:: GraphQL

      .. code-block:: none

        query fetch_person {
            person {
                name
                age
                gender
                voter_info {
                    voter_id
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
                "table" : "person",
                "columns": [
                    "*",
                    {
                        "name": "voter_info",
                        "columns": ["voter_id"]
                    }
                ]
            }
        }


Similarly, to fetch all the entries from the ``voter`` table along with the associated ``person`` info:

.. rst-class:: api_tabs
.. tabs::

   .. tab:: GraphQL

      .. code-block:: none

         query fetch_voter {
            voter {
                id 
                person_info {
                   name
                   age
                   gender 
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
                "table" : "voter",
                "columns": [
                    "*",
                    {
                        "name": "person_info",
                        "columns": ["*"]
                    }
                ]
            }
        }
