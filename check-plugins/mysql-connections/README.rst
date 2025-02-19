Check mysql-connections
=======================

Overview
--------

Checks the connection usage rate, the rate of aborted connections and if name resolution is active for new connections on MySQL/MariaDB. Logic is taken from `MySQLTuner script <https://github.com/major/MySQLTuner-perl>`_:mysql_stats(), v1.9.8.

Hints:

* On RHEL 7+, one way to install the Python MySQL Connector is via ``pip install pymysql``
* Compared to check_mysql / MySQLTuner this check currently:

    * supports only simple login with username/password (not via SSL/TLS)
    * does not support a connection via socket


Fact Sheet
----------

.. csv-table::
    :widths: 30, 70
    
    "Check Plugin Download",                "https://github.com/Linuxfabrik/monitoring-plugins/tree/main/check-plugins/mysql-connections"
    "Check Interval Recommendation",        "Once a minute"
    "Can be called without parameters",     "Yes"
    "Available for",                        "Python 3, Windows"
    "Requirements",                         "Python module ``pymysql``; User with no privileges, locked down to ``127.0.0.1`` - for example ``monitoring@127.0.0.1``. Usernames in MySQL/MariaDB are limited to 16 chars in specific versions."


Help
----

.. code-block:: text

    usage: mysql-connections [-h] [-V] [--always-ok] [-H HOSTNAME]
                             [--ignore-name-resolution] [-p PASSWORD]
                             [--port PORT] [--timeout TIMEOUT] [-u USERNAME]

    Checks the connection usage rate, the rate of aborted connections and if name
    resolution is active for new connections on MySQL/MariaDB.

    options:
      -h, --help            show this help message and exit
      -V, --version         show program's version number and exit
      --always-ok           Always returns OK.
      -H HOSTNAME, --hostname HOSTNAME
                            MySQL/MariaDB hostname. Default: 127.0.0.1
      --ignore-name-resolution
                            Do not check if name resolution is active. Default:
                            False
      -p PASSWORD, --password PASSWORD
                            Use the indicated password to authenticate the
                            connection. Default:
      --port PORT           MySQL/MariaDB port. Default: 3306
      --timeout TIMEOUT     Network timeout in seconds. Default: 3 (seconds)
      -u USERNAME, --username USERNAME
                            MySQL/MariaDB username. Default: root


Usage Examples
--------------

.. code-block:: bash

    ./mysql-connections --hostname localhost --username root --password mypassword

Output:

.. code-block:: text

    36.4% used (4/11); 0.0% aborted connections (0.0/1.3K); Name resolution is active: A reverse name resolution is made for each new connection and can reduce performance [WARNING]. Configure your accounts with ip or subnets only, then update your configuration with skip-name-resolve=ON.


States
------

* WARN if the number of connections is more than 85% of the maximum possible number of simultaneous client connections (setting ``max_connections``).
* WARN if the number of aborted connections is more than 3% of all client connections.
* WARN if name resolution is active (if `--ignore-name-resolution` is ommitted).


Perfdata / Metrics
------------------

.. csv-table::
    :widths: 25, 15, 60
    :header-rows: 1
    
    Name,                                       Type,               Description
    mysql_aborted_connects,                     Continous Counter,  "Number of failed server connection attempts. This can be due to a client using an incorrect password, a client not having privileges to connect to a database, a connection packet not containing the correct information, or if it takes more than connect_timeout seconds to get a connect packet."
    mysql_connections,                          Continous Counter,  "Number of all connection attempts (both successful and unsuccessful)."
    mysql_interactive_timeout,                  Seconds,            "Time in seconds that the server waits for an interactive connection (one that connects with the mysql_real_connect() CLIENT_INTERACTIVE option) to become active before closing it."
    mysql_max_connections,                      Number,             "The maximum number of simultaneous client connections."
    mysql_max_used_connections,                 Number,             "Max number of connections ever open at the same time."
    mysql_pct_connections_aborted,              Percentage,         Aborted_connects / Connections \* 100
    mysql_pct_connections_used,                 Percentage,         Max_used_connections / max_connections \* 100
    mysql_wait_timeout,                         Seconds,            "Time in seconds that the server waits for a connection to become active before closing it."


Credits, License
----------------

* Authors: `Linuxfabrik GmbH, Zurich <https://www.linuxfabrik.ch>`_
* License: The Unlicense, see `LICENSE file <https://unlicense.org/>`_.
* Credits:

    * heavily inspired by MySQLTuner (https://github.com/major/MySQLTuner-perl)
