.. _hasura_microservice_open:

Hasura CLI: hasura microservice open
------------------------------------

Open a microservice url in browser

Synopsis
~~~~~~~~


Open the external url for a microservice in browser

::

  hasura microservice open [flags]

Examples
~~~~~~~~

::

    # Open external url for microservice 'api'
    $ hasura microservice open api

    # Open /ui on auth microservice (hasura)
    $ hasura microservice open auth -n hasura -p ui

Options
~~~~~~~

::

  -c, --cluster string     cluster alias on which the command has to be executed
  -h, --help               help for open
  -n, --namespace string   use --namespace=hasura for hasura microservices (default "user")
  -p, --path string        path to open, appended to the URL

Options inherited from parent commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

      --project string   hasura project directory where the commands should be executed. (default: current directory)

SEE ALSO
~~~~~~~~

* :ref:`hasura microservice <hasura_microservice>` 	 - Manage microservices on hasura

*Auto generated by spf13/cobra*
