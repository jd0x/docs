.. _hasura_cluster_create:

Hasura CLI: hasura cluster create
---------------------------------

Create a Hasura cluster. Read more about Hasura clusters :ref:`here <hasura_cluster_doc>`.

Synopsis
~~~~~~~~


Create new Hasura clusters of various kinds and add it to current project

::

  hasura cluster create [flags]

Examples
~~~~~~~~

::

    # Create a free-tier Hasura cluster
    # use infrastructure config code 'free':
    $ hasura cluster create -i free

    # Create a pro-tier Hasura cluster:
    # visit https://hasura.io/pricing/ and select a config
    # get the infrastructure config code and execute the command
    $ hasura cluster create -i [code]

    # NOTE: If you execute this command within a Hasura project, it will
    # add the cluster to the project with alias 'hasura' and
    # add your SSH key to the cluster.
    # If a cluster with alias 'hasura' already exists in the project,
    # the command will prompt for a confirmation on whether to overwrite it or not.

    # More examples (can only be executed within a Hasura project):

    # Create a Hasura cluster using infrastructure config that is already present
    # in project's 'clusters.yaml' file, having some alias:
    $ hasura cluster create -c [cluster-alias]

    # Create a Hasura cluster, but don't add it to the project:
    $ hasura cluster create -i [code] --no-add

    # Create a Hasura cluster and add it to the project with a different alias
    # other than the default 'hasura' (say 'staging'):
    $ hasura cluster create -i [code] --add-as staging

    # Create a Hasura cluster and add it to the project with a different alias
    # other than the default 'hasura' (say 'staging')
    # If a cluster with alias already exists in the project,
    # over-write it with this cluster
    $ hasura cluster create -i [code] --add-as staging --overwrite


Options
~~~~~~~

::

      --add-as string       cluster will be added to the project with this alias (default "hasura")
  -c, --cluster string      take infrastructure configuration from cluster in clusters.yaml with this alias
  -h, --help                help for create
  -i, --infra string        infrastructure configuration code for the cluster to be created
      --no-add              set this flag if cluster should not be added to the project after creation
      --overwrite           over-write existing cluster with same alias on conflict
      --skip-confirmation   skip confirmation prompt to continue with cluster creation

Options inherited from parent commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

      --project string   hasura project directory where the commands should be executed. (default: current directory)

SEE ALSO
~~~~~~~~

* :ref:`hasura cluster <hasura_cluster>` 	 - Manage hasura clusters

*Auto generated by spf13/cobra*
