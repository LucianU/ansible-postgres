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
|`postgres_user` | User to be crated | `none` |

Dependencies
------------
none

License
-------
BSD
