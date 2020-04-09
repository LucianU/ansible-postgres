# postgres

Installs `PostgreSQL` and performs other related actions:
- creates a database user
- creates a database for that user
- adds an entry to `pg_hba.conf` for a specified IP
- installs a `postgresql.conf` that:
  - enables listening on all interfaces
  - enables archiving
- enables backups performed with `pgbackrest` to an S3-compatible storage service (AWS S3, DigitalOcean Spaces)

The setup of `pgbackrest` is taken from its documentation:
https://pgbackrest.org/user-guide.html

## Role Variables
| Variable | Description | Default value |
|----------|-------------|---------------|
|`postgres_backups_repo_bucket_name`| The bucket name of the S3-compatible storage | `none` |
|`postgres_backups_repo_region`| The region of the S3-compatible storage service | `none` |
|`postgres_backups_repo_endpoint`| The URL of the S3-compatible storage service | `none` |
|`postgres_backups_repo_cipher_pass`| The password used for the encryption of the backup repo | `none` |
|`postgres_backups_repo_key`| The key used to access the S3-compatible storage service | `none` |
|`postgres_backups_repo_key_secret`| The secret associated with the key used to access the S3-compatible storage service | `none` |
|`postgres_cluster_name`| Name of the default cluster | `none` |
|`postgres_db`| Database to be created | `{{ postgres_user }}` |
|`postgres_enable_backups`| Enables the backup service which uses `pgbackrest` | `true` |
|`postgres_install_ppa` | Whether to install a PPA | `false` |
|`postgres_user` | User that will own the database we're creating | `none` |
|`postgres_user_password`| The user's password | `none` |
|`postgres_ubuntu_version_name` | The name of the Ubuntu version. Used for the PPA | `bionic` |
|`postgres_version` | The version of PostgreSQL you want to install | `11`|
|`postgres_whitelist_ip` | The IP to add to `pg_hba.conf` | `none`|

## Usage
* Example playbook `provision.yml`

```yml
---
- name: Setup PostgreSQL Machine
  hosts: all
  vars:
    ansible_python_interpreter: /usr/bin/python3
  roles:
    - role: ansible-postgres
      vars:
        postgres_backups_repo_bucket_name: maxwell-pg-backups
        postgres_backups_repo_region: fra1
        postgres_backups_repo_endpoint: fra1.digitaloceanspaces.com
        postgres_cluster_name: main
        postgres_enable_backups: true
        postgres_install_ppa: true
        postgres_user: maxwell
        postgres_version: 11
```
* Example run

```bash
ansible-playbook provision.yml \
  --extra-vars="postgres_user_password=${POSTGRES_USER_PASSWORD}" \
  --extra-vars="postgres_backups_repo_cipher_pass=${POSTGRES_BACKUPS_REPO_CIPHER_PASS}" \
  --extra-vars="postgres_backups_repo_key=${DIGITALOCEAN_SPACES_KEY}" \
  --extra-vars="postgres_backups_repo_key_secret=${DIGITALOCEAN_SPACES_KEY_SECRET}" \
  --extra-vars="postgres_whitelist_ip=[ip-of-web-app-machine]" \
  --inventory="[db-machine-ip]," # You need the comma
```

## Dependencies
none

## License
BSD
