<img src="http://www.elao.com/images/corpo/logo_red_small.png"/>

[![Ansible Role](https://img.shields.io/ansible/role/5542.svg?style=plastic)](https://galaxy.ansible.com/list#/roles/5542) [![Platforms](https://img.shields.io/badge/platforms-debian-lightgrey.svg?style=plastic)](#) [![License](http://img.shields.io/:license-mit-lightgrey.svg?style=plastic)](#)

# Ansible Role: OhMyZsh (see: [https://github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh))

This role will assume the following configuration:
- Install ohmyzsh globally
- Setup a local zshrc file

It's part of the Manala <a href="http://www.manala.io" target="_blank">Ansible stack</a> but can be used as a stand alone component.

## Requirements

None.

## Dependencies

None.

## Installation

### Ansible 2+

Using ansible galaxy cli:

```bash
ansible-galaxy install manala.ohmyzsh,2.0
```

Using ansible galaxy requirements file:

```yaml
- src:     manala.ohmyzsh
  version: 2.0
```

### Ansible 1 (no longer maintained)

Using ansible galaxy cli:

```bash
ansible-galaxy install manala.ohmyzsh,1.0
```

Using ansible galaxy requirements file:

```yaml
- src:     manala.ohmyzsh
  version: 1.0
```

## Role Handlers

None

## Role Variables

| Name                            | Default                    | Type      |Description                                                      |
|-------------------------------- |--------------------------- |---------- |---------------------------------------------------------------- |
| `manala_ohmyzsh_dir`            | /usr/local/share/oh-my-zsh | String    | ohMyZsh installation directory                                  |
| `manala_ohmyzsh_update`         | false                      | Boolean   | Whether or not we should auto retrieve new revision of ohMyZsh  |
| `manala_ohmyzsh_users_template` | users/base.j2              | String    | User config template                                            |
| `manala_ohmyzsh_users`          | []                         | Array     | Collection of users with ohMyZsh custom configurations.         |

### Oh My Zsh configuration

The `manala_ohmyzsh_users_template` key will allow you to use differents main configuration templates. The role is shipped with basic templates :

- base (Simple template with common configuration)
- dev (Dev configuration, provide a different OhMyZsh theme than production template)
- empty ("Let me handle this" template, no default configuration inside.)
- prod (For production purpose.)

#### Example

```
manala_ohmyzsh_users_template: users/base.j2
```

The `manala_ohmyzsh_dir` key is used to specify the path where to checkout oh-my-zsh

#### Example

```
manala_ohmyzsh_dir: /usr/local/share/oh-my-zsh
```

The `manala_ohmyzsh_update` option will allow Oh My Zsh to retrieve new revisions from the repository.

#### Example

```
manala_ohmyzsh_update: false
```

### User configuration

This part allow you, with the key `manala_ohmyzsh_users`, to configure each user account as following:

| Name      | Default                      | Type       | Description                               |
|-----------|----------------------------- |----------- |------------------------------------------ |
| `user`    | ~ (required)                 | String     | User account name                         |
| `home`    | root or '/home/' ~ item.user | String     | User account home directory               |
| `template`| ~ (required)                 | String     | Template used for Oh My Zsh configuration |
| `config`  | []                           | Array      | List of Oh My Zsh options                 |

```
---

_env:        prod

manala_ohmyzsh_users:
  - user:     root
    template: users/{{ _env }}.j2
    config:
      - ZSH_THEME: manala-prod
      - plugins: (git debian common-aliases history history-substring-search)
  - user:     foo
    group:    root # Default to user primary group, but can be overriden
    template: users/{{ _env }}.j2
    config:
      - ZSH_THEME: manala-prod
      - plugins: (git debian common-aliases history history-substring-search)
```

## Example playbook

    - hosts: servers
      roles:
         - { role: manala.ohmyzsh }

# Licence

MIT

# Author information

Manala [**(http://www.manala.io/)**](http://www.manala.io)
