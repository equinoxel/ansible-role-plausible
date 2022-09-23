# laurivan.plausible

This is a role to install [Plausible](https://plausible.io/) via Docker.

## Requirements

You need a machine with Docker installed.

## Role Variables

Plausible depends on several components (SMTP, Clickhouse, PostgreSQL) and this role is designed to install them as a package. All variables are listed below (see also `defaults/main.yml`).

# Plausible parameters

`plausible_port` defined the exposed port for the web site (including JS delivery and API):

```yaml
plausible_port: 8000
```
You need an admin user to be able to access the site:

```yaml
plausible_admin_user_email: changeme@example.com
plausible_admin_user_name: change-me
plausible_admin_user_pwd: change-me
```

The hosting URL of the server, used for URL generation must be specified:

```yaml
plausible_base_url: change-me
```

Please define a long (>= 64 characters) secret key, which will be used by your instance to encrypt data:

```yaml
plausible_secret_key_base: change-me
```

# Email

Plausible can send various emails; for this it needs a SMTP relay. Following variables define the SMTP account from which emails will be sent:

```yaml
plausible_smtp_host:
plausible_smtp_port: 587
plausible_smtp_username:
plausible_smtp_password:
```

If you don't define any, no emails will be sent and you'll need to look through the logs :)

The email on behalf of which messages are sent is defined by:

```yaml
plausible_smtp_email: "changeme@example.com"
```

## Database

Plausible depends on PostgreSQL and Clickhouse. 

For PostgreSQL, you need to specify following variables:

```yaml
plausible_db_host: "plausible_db"
plausible_db: "plausible"
plausible_db_password: "change_me"
```
If you want to expose the database port outside (e.g. for backup), please define `plausible_db_port`


For Clickhouse, you only need to define the version used:

```yaml
plausible_clickhouse_version: "21.3.2.5"
```

### Volumes

The role will create following paths used by the docker containers as volumes:

```yaml
plausible_volume_base: "/mnt/plausible"
plausible_volume_config: "{{ plausible_volume_base }}/config"
plausible_volume_db: "{{ plausible_volume_base }}/db_data"
plausible_volume_events: "{{ plausible_volume_base }}/event_data"
plausible_volume_geoip: "{{ plausible_volume_base }}/geoip_data"
```

The `plausible_volume_geoip` is necessary to access MaxMind GeoIP data (see below).

### GeoIP

Plausible defaults to DBIP, but can be configured to use MaxMind GeoIP. If you want to use MaxMind, you need to define:

```yaml
plausible_geoip_db: "GeoLite2-Country.mmdb"
```

## Dependencies

None

## Example Playbook

```yaml
---
- name: Create unifi volume
  hosts: services
  vars:
    plausible_port: "8100"
    plausible_base_url: "https://metrics.example.com"
    plausible_admin_user_email: "admin@example.com"
    plausible_admin_user_name: "admin"
    plausible_admin_user_pwd: "change-me"
  roles:
    - 'laurivan.plausible'
```

## License

MIT

## Author Information

This role was created in 2022 by [Laur Ivan](https://www.laurivan.com).
