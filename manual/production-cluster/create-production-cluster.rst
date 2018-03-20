Creating a production cluster
=============================

This section takes your over the process of creating a cluster on Hasura's Production tier.

.. note::

   Having an active billing account is a mandatory to create Production tier clusters.

Creating a cluster
------------------

Choose cluster configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You can generate your cluster's configuration using the `Pricing Calculator
<https://hasura.io/pricing>`_.

Create the cluster
^^^^^^^^^^^^^^^^^^
After the above step, follow the ``hasura`` CLI commands to create a cluster of
that configuration.

It is usually of the form:

.. code-block:: bash

   $ hasura cluster create --infra ZXPBVF

Following the above will create a cluster and add its configuration to the
``clusters.yaml`` file, with a default the cluster alias ``hasura``.

To add your own alias, run:

.. code-block:: bash

   $ hasura cluster create --infra ZXPBVF --add-as prod

``prod`` will be set as the alias for the created cluster.


Add the cluster to a Hasura project
-----------------------------------
CLI will add by default if you created the cluster while in a project directory.

If you executed, ``cluster create`` outside a project directory, you have add
this cluster to an existing project.

To add this cluster to a Hasura project, ``cd`` into the Hasura project
directory, and run:

.. code-block:: bash

   $ hasura cluster add <cluster-name> -c <cluster-alias>
   $ hasura ssh-key add -c <cluster-alias>
   $ git push <cluster-alias> master

where ``<cluster-name>`` is the newly created name of your cluster, and
``<cluster-alias>`` is the cluster alias.


Example
-------
Let's assume the infra code generated from the Pricing Calculator is ``ZXPBVF``,
and assume we want alias of this cluster to be ``prod``.

Then the flow to create a cluster and adding it to a project is:

.. code-block:: bash

   $ hasura cluster create --infra ZXPBVF --add-as prod
   # say cluster name is: ambitious93
   # cd into the project directory
   $ hasura cluster add ambitious93 -c prod
   $ hasura ssh-key add -c prod
   # git push the project to this cluster
   $ git push prod master


Enable billing on your account
------------------------------
Log in to `Hasura Dashboard <https://dashboard.hasura.io/>`_, go to the
`Billing` tab, and add payment details to enable billing on your account. You
can refer to the documentation :doc:`here <./manage-billing>` on how to do so.


Advanced
--------
You can also write declarative configuration of your cluster. The file
``clusters.yaml`` (at the top-level of a Hasura project directory) can contain
cluster configuration. Check out the :doc:`reference documentation for
clusters.yaml <./reference-clusters-yaml>` and some :doc:`sample cluster
configurations <./sample-cluster-configs>`.

Once you have added your cluster configuration in ``clusters.yaml``, run the
following command to create a cluster.

.. code-block:: bash

   $ hasura cluster create --cluster <cluster-alias>


where ``<cluster-alias>`` is the cluster alias in the ``clusters.yaml``.

After this, you should add the cluster to the project. For reference, see
:doc:`this <../hasuractl/hasura_cluster_add>`.
