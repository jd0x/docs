.. _data-graphql:

GraphQL API reference
=====================

GraphQL endpoints
-----------------

The data microservice exposes the GraphQL interface at ``/v1alpha1/graphql``. So the publicly accessible URL will be
``https://data.<project-name>.hasura-app.io/v1alpha1/graphql``. It accepts a ``POST`` request as follows:

.. code-block:: http

   POST data.<project-name>.hasura-app.io/v1alpha1/graphql HTTP/1.1
   Content-Type: application/json
   Authorization: Bearer <auth-token> # optional if cookie is set
   X-Hasura-Role: <role>  # optional. Pass if only specific user role has access

   {
       "operationName" : "some-operation-name",
       "query" : "a graphql query",
       "variables" : {
           "variable-name" : "variable-value"
       }
   }

This is the standard interface that any GraphQL library expects, so if you configure your GraphQL client library with
the endpoint, everything should work as expected (with the limitations mentioned below).

The schema is exposed separately at this endpoint (as introspection is not yet supported): ``/v1alpha1/graphql/schema``.
This is only accessible by an admin.

.. code-block:: http

   GET data.<project-name>.hasura-app.io/v1alpha1/graphql/schema HTTP/1.1
   Authorization: Bearer <auth-token> # optional if cookie is set
   X-Hasura-Role: <role>  # optional. Pass if only specific user role has access

Queries and mutations
---------------------

All tracked tables in the ``public`` schema of ``hasuradb`` database can be queried and modified over the GraphQL
endpoint. The top level query type is ``query_root`` whose fields are the table names. The top level mutation type
is ``mutation_root`` whose fields are ``insert_<table-name>``, ``update_<table-name>``, ``delete_<table-name>``
for all tables.

For example, if you have an ``author`` table defined, the GraphQL schema for ``query_root`` looks as follows:

.. code-block:: none

   type query_root {

     author(
       where: author_bool_exp
       limit: Int
       offset: Int
       order_by: [String]
     ): [author]

   }

Examples:

- Fetch all the authors with their id and name fields.

  .. code-block:: none

     query {
       author {
         id
         name
       }
     }

- Fetches the author whose name is ``"maya"``.

  .. code-block:: none

     query {
       author (where: {name: {_eq: "maya"}}) {
         id
         name
       }
     }

- ... and their articles (an array relationship to article table)

  .. code-block:: none

     query {
       author (where: {name: {_eq: "maya"}}) {
         id
         name
         articles {
           title
           rating
         }
       }
     }

- ... and fetch only the top 5 sorted by rating

  .. code-block:: none

     query {
       author (where: {name: {_eq: "maya"}}) {
         id
         name
         articles (order_by: ["+rating"] limit: 5) {
           title
           rating
         }
       }
     }

The ``mutation_root`` will be as follows:

.. code-block:: none

   type mutation_root {

     insert_author(
       objects: [author_input!]!
     ): author_mutation_response

     update_author(
       where: author_bool_exp! _set: author_input!
     ): author_mutation_response

     delete_author(
       where: author_bool_exp!
     ): author_mutation_response

   }

Examples:

- Insert an author returing the id.

  .. code-block:: none

     mutation {
       insert_author (
         objects: [{name: "srishti"}]
       ) {
         returning {
           id
         }
       }
     }

- Update the name of the author named "srishti" to "shukra", returning the number of affected rows.

  .. code-block:: none

     mutation {
       update_author (
         where: { name: {_eq: "srishti"} }
         _set: { name: "shukra" }
       ) {
         affected_rows
       }
     }

- Delete author named "shukra" returning id of the deleted author and the number of affected rows.

  .. code-block:: none

     mutation {
       delete_author (
         where: { name: {_eq: "shukra"} }
       ) {
         affected_rows
         returning {
           id
         }
       }
     }

Permissions
-----------

Permissions that are added through the api-console are enforced for every GraphQL query. By default (unless a
permission is added), only users with admin role can query/modify a table.

.. _generate-schema-json:

Generating schema.json
----------------------

As we don't yet support introspection over the graphql endpoint, the standard tooling
(`apollo-codegen <https://github.com/apollographql/apollo-codegen>`_) to generate ``schema.json`` will not work out
of the box. You'll need to run an additional command to fetch the schema as follows:

.. code-block:: Bash

   $ curl -H 'Authorization: Bearer <auth-token>' 'https://data.<cluster-name>.hasura-app.io/v1alpha1/graphql/schema' | jq -r '.schema' > schema.graphql

Now that you have the GraphQL schema, you can generate ``schema.json`` as follows:

.. code-block:: Bash

   $ apollo-codegen introspect-schema schema.graphql --output schema.json

Current limitations
-------------------

1. No support for fragments.
2. No support for introspection. However, you can fetch the GrahpQL schema at a different endpoint (just not through
   the introspection query). This schema can be used in various client libraries. See :ref:`generate-schema-json` for
   detailed instructions.
3. Error messages may not point to the exact location of syntax error.
