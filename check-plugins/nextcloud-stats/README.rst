Check nextcloud-stats
=====================

Overview
--------

This plugin lets you track the number of active users over time, the number of shares in various categories and some storage statistics against a Nextcloud server. Might take up to 30 seconds for the first time; after that, still takes a few seconds.

To access the serverinfo API you will need the credentials of an admin user. It is recommended to create an app password (at https://cloud.example.com/index.php/settings/user/security) or a separate user for that purpose.

Hints:

* If you simply want to check the availability of the Nextcloud web frontend, you have to use other checks.
* If a Nextcloud App leads to a "500 Internal Server Error", the Nextcloud API often still remains intact (so this check can't report this).

Tested with Nextcloud 15+.


Fact Sheet
----------

.. csv-table::
    :widths: 30, 70
    
    "Check Plugin Download",                "https://github.com/Linuxfabrik/monitoring-plugins/tree/main/check-plugins/nextcloud-stats"
    "Check Interval Recommendation",        "Every 15 minutes"
    "Can be called without parameters",     "No"
    "Available for",                        "Python 2, Python 3, Windows"
    "Requirements",                         "Nextcloud App 'serverinfo'"


Help
----

.. code-block:: text

    usage: nextcloud-stats [-h] [-V] [--always-ok] [--insecure] [--no-proxy]
                           --password PASSWORD [--url URL] [--username USERNAME]

    This plugin lets you track the number of active users over time, the number of
    shares in various categories and some storage statistics against a Nextcloud
    server.

    options:
      -h, --help           show this help message and exit
      -V, --version        show program's version number and exit
      --insecure           This option explicitly allows to perform "insecure" SSL
                           connections. Default: False
      --no-proxy           Do not use a proxy. Default: False
      --password PASSWORD  Nextcloud API password.
      --url URL            Nextcloud API URL. Default: http://localhost/nextcloud/
                           ocs/v2.php/apps/serverinfo/api/v1/info
      --username USERNAME  Nextcloud API username. Default: admin


Usage Examples
--------------

.. code-block:: bash

    ./nextcloud-stats --username nextcloud-stats --password mypassword --url http://localhost/nextcloud/ocs/v2.php/apps/serverinfo/api/v1/info
    
Output:

.. code-block:: text

    8 users (3/5/7 in the last 5min/1h/24h), 116.4K files, 50 apps, v24.0.8.3
    * Shares: 142 (18 groups, 57 links [56 w/o password], 0 mails, 0 rooms, 14 users, 0 federated sent)
    * Federated Shares: 0 received
    * Storages: 14 (0 home, 14 other, 0 local)
    * PHP: v8.1.13, upload_max_filesize=9.8GiB, max_execution_time=3600s, memory_limit=1.0GiB
    * DB: mysql v10.6.11, size=148.7MiB
    * Web: Apache, local memcache: Memcache\Redis, locking memcache: Memcache\Redis


States
------

* Always returns OK.


Perfdata / Metrics
------------------

* nc_active_users_last5min
* nc_active_users_last1h
* nc_active_users_last24h
* nc_server_database_size
* nc_shares_num_fed_shares_received
* nc_shares_num_fed_shares_sent
* nc_shares_num_shares
* nc_shares_num_shares_groups
* nc_shares_num_shares_link
* nc_shares_num_shares_link_no_password
* nc_shares_num_shares_mail
* nc_shares_num_shares_room
* nc_shares_num_shares_user
* nc_storage_num_files
* nc_storage_num_storages
* nc_storage_num_storages_home
* nc_storage_num_storages_local
* nc_storage_num_storages_other
* nc_storage_num_users: For num_users, the Nexctloud serverinfo app (NC21) returns the number of users that have ever existed, and not those that are enabled. See https://github.com/nextcloud/serverinfo/issues/311
* nc_system_apps_num_installed


Troubleshooting
---------------

Unknown error while fetching http://localhost/nextcloud/ocs/v2.php/apps/serverinfo/api/v1/info?format=json, maybe timeout or error on webserver
    Check the Nextcloud API endpoint URL. Maybe change from http(s)://localhost to http(s)://127.0.0.1.

HTTP error "401 Unauthorized" while fetching http://...
    Password is correct? Maybe you enabled 2FA. Use an app password for your monitoring server.


Credits, License
----------------

* Authors: `Linuxfabrik GmbH, Zurich <https://www.linuxfabrik.ch>`_
* License: The Unlicense, see `LICENSE file <https://unlicense.org/>`_.
* Credits: Inspired by: https://github.com/BornToBeRoot/check_nextcloud
