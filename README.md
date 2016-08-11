postgres
========
Installs `PostgreSQL` and performs other related actions:
- creates a database user
- creates a database for that user

Role Variables
--------------
| Variable | Description | Default value |
|----------|-------------|---------------|
|`postgres_db`| Database to be created | `{{ postgres_user }}` |
|`postgres_install_ppa` | Whether to install a PPA | `false` |
|`postgres_user` | User to be created | `none` |
|`postgres_version` | The version of PostgreSQL you want to install | `9.3`|

Dependencies
------------
none

License
-------
BSD
