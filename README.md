# Ansible role for MetricBeat

[![Build Status](https://travis-ci.org/orhan89/ansible-role-packetbeat.svg)](https://travis-ci.org/orhan89/ansible-role-packetbeat)

An Ansible Role that installs PacketBeat on Red Hat/CentOS or Debian/Ubuntu.

## Tested On

  * EL / Centos (6 / 7)
  * Debian (Wheezy / Jessie)
  * Ubuntu (Trusty)
  * Solaris

## Role Variables

Available variables are listed below, along with their default values as
definied in `defaults/main.yml`.

PacketBeat user and group. If you run PacketBeat with a user other than root make
sure your logs are readable by the PacketBeat user. Add the PacketBeat user to a
privileged group, with access to your logs.

On Ubuntu you would add the user to the `adm` group. On CentOS you can adjust
the permissions with the `setfacl` command, e.g. `sudo setfacl -m g:packetbeat:r
<path>`.

    packebeat_user: root
    packetbeat_group: root

Create the `packetbeat` user and group.

    packetbeat_create_user: true

PacketBeat version to use.

    packetbeat_version: 1.1.1

Make use of the PacketBeat apt repo.

On Debian-based systems, you may use a URL to install a specific `.deb`.
To do so, change `packetbeat_use_apt_repo` value to `false`, then (optionally)
adjust the value of `packetbeat_deb_baseurl` (which has a default value set for you).

    packetbeat_use_apt_repo: true

PacketBeat `.deb` base URL for package download if `packetbeat_use_apt_repo: false`

    packetbeat_deb_baseurl: "https://download.elastic.co/beats/packetbeat"

Start PacketBeat at boot time.

    packetbeat_start_at_boot: true

PacketBeat version upgrade. This option allows package upgrades.

    packetbeat_upgrade: false

PacketBeat configuration file.

    packetbeat_config_file: /etc/packetbeat/packetbeat.yml

PacketBeat registry file.

    packetbeat_config_registry_file: /var/lib/packetbeat/registry

The PacketBeat configuration is built based on the variable `packetbeat_config`.
For easier management of the contents, the `packetbeat_config` variable is made
up of multiple other variables:

* `packetbeat_config_interfaces`
* `packetbeat_config_protocols`
* `packetbeat_config_output`
* `packetbeat_config_shipper`
* `packetbeat_config_logging`

```yaml
packetbeat_config_interfaces: |
  interfaces:
    devices: any
packetbeat_config_protocols: |
  protocols:
    dns:
      ports: [53]

      include_authorities: true
      include_additionals: true
packetbeat_config_output: |
  output:
    elasticsearch:
      hosts: [ 'localhost:9200' ]
packetbeat_config_shipper: |
  shipper:
packetbeat_config_logging: |
  logging:
    files:
      rotateeverybytes: 10485760 # = 10MB
packetbeat_config: |
  {{packetbeat_config_output}}
  {{packetbeat_config_shipper}}
  {{packetbeat_config_logging}}
```

PacketBeat templates (a list of templates to install).
These templates will be copied to the /etc/packetbeat directory
and can be used in the elasticsearch output for example.

https://www.elastic.co/guide/en/beats/packetbeat/current/elasticsearch-output.html#_template

    packetbeat_templates: []

## Usage
```yaml
    - hosts: logging
      roles:
        - { role: orhan89.packetbeat }
```

## License

See [License](LICENSE)

## Author Information

This role was created in 2016 by Ricky Hariady. It is based on the FileBeat role by Emiliano Castagnari: https://github.com/torian/ansible-role-filebeat
