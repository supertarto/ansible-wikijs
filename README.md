# Ansible Wiki.js
[![CI](https://github.com/supertarto/ansible-wikijs/actions/workflows/ci.yml/badge.svg)](https://github.com/supertarto/ansible-wikijs/actions/workflows/ci.yml)

Install and configure Wiki.js with Ansible.


## Requirements

Wiki.js require nodejs and a database such as PostrgeSQL. You can also use MariaDB or SQLite, but Wiki.js recommand PostgreSQL for futur update.
You can use supertarto.nodejs and supertarto.postgresql

## Tested plateform

* Debian 10 (Buster)
* Debian 11 (Bullseye)
* Debian 12 (Bookworm)

## Role variables

Force install/update.

```yml
wikijs_update: false
```

Wikijs Version, download url and destination on your server.

```yml
wikijs_version: "2.5..300"
wikijs_download_url: "https://github.com/Requarks/wiki/releases/download/{{ wikijs_version }}/wiki-js.tar.gz"
wikijs_download_dest: /usr/local/wikijs
```

System user and group that should be created and used for the running service. wikijs_user_additional_groups allows specifying additional groups for the system user.

```yml
wikijs_system_user: wikijs
wikijs_system_group: wikijs
wikijs_user_additional_groups: ""
```

Port used to connect to your Wiki.js instance.

```yml
wikijs_config_port: 3000
```

Database informations. Postgres is recommanded.

```yml
# postgres, mysql, mariadb, mssql, sqlite
wikijs_config_db_type: postgres
wikijs_config_db_host: localhost
wikijs_config_db_port: 5432
wikijs_config_db_user: wikijs
wikijs_config_db_pass: wikijsrocks
wikijs_config_db_db: wiki
wikijs_config_db_ssl: false
```

Configuration of the SSL connection to the database.
Ignored if `auto` is `true`.

```yml
wikijs_config_ssl_options:
  auto: true
  # Use the fields you need when 'auto' is false.
  # rejectUnauthorized: true
  # ca: path/to/ca.crt
  # cert: path/to/cert.crt
  # key: path/to/key.pem
  # pfx: path/to/cert.pfx
  # passphrase: xyz123
```

Only used if sqlite is selected

```yml
wikijs_config_sqlite_storage: path/to/database.sqlite
```

Informations about you SSL certificate, if you need to use SSL.

```yml
wikijs_config_ssl_enabled: false
wikijs_config_ssl_port: 3443
# custom, letsencrypt
wikijs_config_ssl_provider: custom

# For custom only
# pem, pfx
wikijs_config_ssl_format: pem
# only for pem
wikijs_config_ssl_key: path/to/key.pem
wikijs_config_ssl_cert: path/to/cert.pem
wikijs_config_ssl_pfx: path/to/cert.pfx
wikijs_config_ssl_passphrase: null
wikijs_config_ssl_dhparam: null

# For letsencrypt only
wikijs_config_ssl_domain: wiki.yourdomain.com
wikijs_config_ssl_subscriberEmail: admin@example.com

wikijs_config_bindIP: "0.0.0.0"
wikijs_config_logLevel: info
wikijs_config_offline: false
wikijs_config_ha: false
wikijs_config_dataPath: ./data
```

## Examples

```yml
---
- hosts: all
  roles:
    - role: supertarto.nodejs
    - role: supertarto.postgresql
    - role: supertarto.wikijs

  vars:
    postgresql_databases:
      - name: wiki
    postgresql_users:
      - name: wikijs
        password: wikijsrocks
```

## Installation

```bash
ansible-galaxy role install supertarto.wikijs
```

## License
GPL V3.0
