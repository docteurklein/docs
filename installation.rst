.. _installation:

PipelineDB Installation
===========================

Install PostgreSQL
---------------------------

Since PipelineDB runs as an extension to PostreSQL, begin by `installing PostgreSQL`_.

.. note:: PipelineDB currently supports PostgreSQL versions 10.1, 10.2, 10.3, 10.4, 10.5, and 11.0.

.. _`installing PostgreSQL`: https://www.postgresql.org/download/

Once you have PostgreSQL installed on your system, you just need to install the PipelineDB binaries and then create the PipelineDB extension within your PostgreSQL database. You can install binaries from our **apt** or **yum** repositories or you can download packages from our `release archives`_ and install them directly.

.. _`release archives`: https://github.com/pipelinedb/pipelinedb/releases

apt
------------

First, add our **apt** repository to your system (`inspect apt.sh`_):

.. _`inspect apt.sh`: http://download.pipelinedb.com/apt.sh

.. code-block:: sh

	curl -s http://download.pipelinedb.com/apt.sh | sudo bash

Now simply install the latest PipelineDB package:

.. code-block:: sh

  # PostgreSQL 10
  sudo apt-get install pipelinedb-postgresql-10

  # PostgreSQL 11
  sudo apt-get install pipelinedb-postgresql-11

yum
---------------

Add our **yum** repository to your system (`inspect yum.sh`_):

.. _`inspect yum.sh`: http://download.pipelinedb.com/yum.sh

.. code-block:: sh

	curl -s http://download.pipelinedb.com/yum.sh | sudo bash

Install the latest PipelineDB package:

.. code-block:: sh

 # PostgreSQL 10
 sudo yum install pipelinedb-postgresql-10
  
 # PostgreSQL 11
 sudo yum install pipelinedb-postgresql-11

.. note:: **apt** and **yum** repositories only need to be added to your system a single time. Once you've added them, you don't need to run these scripts again. You need only run the installation commands to get new versions of PipelineDB.

-------------------------

You may also download binary packages from our `release <https://github.com/pipelinedb/pipelinedb/releases>`_ archives and install them directly.

RPM Packages
--------------------

To install the PipelineDB RPM package, run:

.. code-block:: sh

	sudo rpm -ivh pipelinedb-postgresql-<pg version>_<pipelindb version>.rpm

Debian Packages
---------------------

To install the PipelineDB Debian package, run:

.. code-block:: sh

	sudo dpkg -i pipelinedb-postgresql-<pg version>_<pipelindb version>.deb

.. _creating-extension:

Creating the PipelineDB Extension
------------------------------------------

In order for PipelineDB to run, the :code:`shared_preload_libraries` configuration parameter must be set in :code:`postgresql.conf`, which can be found underneath your data directory. It's also a good idea to set :code:`max_worker_processes` to something reasonably high to give PipelineDB worker processes plenty of capacity:

.. code-block:: sh

	# At the bottom of <data directory>/postgresql.conf
	shared_preload_libraries = 'pipelinedb'
	max_worker_processes = 128
	
Running PostgreSQL
---------------------

To run the PostgreSQL server in the background, use the :code:`pg_ctl` driver and point it to your newly initialized data directory:

.. code-block:: sh

	pg_ctl -D <data directory> -l postgresql.log start

To connect to a running server using the default database, use PostgreSQL's standard client, `psql`_, which can be used to create the PipelineDB extension:

.. code-block:: sh

	psql -c "CREATE EXTENSION pipelinedb"

Once the PipelineDB extension has been created, you're ready to start using PipelineDB!

.. _`psql`:  https://www.postgresql.org/docs/current/static/app-psql.html

You can check out the :ref:`quickstart` section to start streaming data into PipelineDB right now.

Configuration
---------------------

By default, PostgreSQL is not configured to allow incoming connections from remote hosts. To enable incoming connections, first set the following line in :code:`postgresql.conf`:

.. code-block:: sh

    listen_addresses = '*'

And in :code:`pg_hba.conf`, add a line such as the following to allow incoming connections:

.. code-block:: sh

    host    all             all             <ip address>/<subnet>            md5


For example, to allow incoming connections from any host:

.. code-block:: sh

    host    all             all             0.0.0.0/0            md5

-------------

Docker
---------------------

PipelineDB is available as a Docker image, making it very easy to run on platforms that don't currently have official packages. The PipelineDB extension will automatically be created upon database initialization, so :ref:`creating-extension` is is not necessary with the Docker image.

You can run a PipelineDB Docker container via :code:`docker run`:

.. code-block:: sh
  
  docker run pipelinedb/pipelinedb-postgresql-{postgresql version}

The PipelineDB Docker image uses the `PostgreSQL image`_ as its parent, so all configuration and customization can be done via the interface that the `PostgreSQL image`_ provides.

.. _`PostgreSQL image`: https://hub.docker.com/_/postgres/

-----------------

Now you're ready to put PipelineDB to work! Check out the :ref:`continuous-views` or :ref:`quickstart` sections to get started.

