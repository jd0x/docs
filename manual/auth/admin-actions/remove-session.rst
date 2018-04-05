Create user session (as admin)
==============================

To create a session to a user, make a request to the admin endpoint : ``/v1/admin/user/create-session``

.. code-block:: http

   POST auth.<cluster-name>.hasura-app.io/v1/admin/user/create-session HTTP/1.1
   Content-Type: application/json

   {
      "hasura_id": 3,
      "expiry_time": 89371
   }

* ``expiry_time`` is an integer field in seconds. But it is optional.

If the request is successful, the API will return response with user details.

Typical response of the ``/v1/admin/user/create-session`` request is :

.. code-block:: http

   HTTP/1.1 200 OK
   Content-Type: application/json

   {
      "hasura_id": 3,
      "hasura_roles": [
        "user", "merchant", "something"
      ],
      "auth_token": "Asjowjj21oid32on54mcuwoeADSjsah78fiSjseoi28",
      "email": "something@email.com",
      "mobile": "919999999999",
      "username": "something123",
      "created": "2014-11-05T08:15:30-05:00",
      "last_login": "2014-11-05T08:15:30-05:00",
      "is_active": true,
   }


* ``auth_token``  is the authentication token of the user for the current session.

* ``hasura_roles``  is a list of all roles assigned to the user.

* ``hasura_id``  is the hasura identifier of the user.

* ``is_active``  gives status of the user.

