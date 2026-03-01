apt_package_setup
=================

Ansible role to set up apt packages and repositories on Debian-based systems. It can add GPG-signed repositories, PPA repositories, run apt upgrades, and install packages.

Requirements
------------

- A Debian-based target system (e.g. Ubuntu, Debian).
- Ansible >= 2.9

Role Variables
--------------

All configuration is nested under the `apt` dictionary. A `default_apt` dictionary with the same structure is also defined as a fallback.

### `apt.repos`

A list of additional apt repositories to add, including their GPG keys. Each item is a dictionary with the following keys:

| Key | Type | Description |
|---|---|---|
| `name` | string | A unique name used as the filename for the GPG key in `/etc/apt/trusted.gpg.d/`. |
| `key_url` | string | URL to download the repository's GPG signing key from. |
| `key_type` | string | File extension for the key (e.g. `asc`, `gpg`). |
| `repo` | string | The apt repository line (e.g. `deb https://example.com/repo stable main`). |

Default: `[]`

### `apt.ppa_repos`

A list of PPA repositories to add. Each item is a dictionary with the following key:

| Key | Type | Description |
|---|---|---|
| `repo` | string | The PPA repository string (e.g. `ppa:user/ppa-name`). |

Default: `[]`

### `apt.packages`

A list of apt package names to install.

Default: `[]`

### `apt.do_upgrade`

Whether to run `apt upgrade` on the target system.

| Type | Default |
|---|---|
| bool | `true` |

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: apt_package_setup
      vars:
        apt:
          repos:
            - name: docker
              key_url: https://download.docker.com/linux/ubuntu/gpg
              key_type: asc
              repo: "deb https://download.docker.com/linux/ubuntu jammy stable"
          ppa_repos:
            - repo: ppa:deadsnakes/ppa
          packages:
            - docker-ce
            - python3.11
            - htop
          do_upgrade: true
```

License
-------

MIT

Author Information
------------------

Kilian Kreibich, FabLab Ansbach e.V.
