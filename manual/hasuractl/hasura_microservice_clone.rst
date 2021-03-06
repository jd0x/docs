.. _hasura_microservice_clone:

Hasura CLI: hasura microservice clone
-------------------------------------

Clone microservice from a project on Hasura Hub

Synopsis
~~~~~~~~


Clone just a microservice from a project on https://hasura.io/hub

::

  hasura microservice clone [[microservice-1] [microservice-2]...] --from [hub-user/hub-project-name] [flags]

Examples
~~~~~~~~

::

    # Clone microservice 'app' from 'hasura/hello-python-flask':
    $ hasura microservice clone app --from hasura/hello-python-flask

    # Clone all microservices from 'hasura/hello-react'
    $ hasura microservice clone --from hasura/hello-react

    # Clone microservices 'api' and 'ui' from 'hasura/hello-react'
    $ hasura microservice clone api ui --from hasura/hello-react


Options
~~~~~~~

::

      --from string   hub project to the clone the microservice from
  -h, --help          help for clone

Options inherited from parent commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

      --project string   hasura project directory where the commands should be executed. (default: current directory)

SEE ALSO
~~~~~~~~

* :ref:`hasura microservice <hasura_microservice>` 	 - Manage microservices on hasura

*Auto generated by spf13/cobra*
