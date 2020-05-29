# omsagent

Install Microsoft omsagent (Log Analytics agent) on your system.

|Travis|GitHub|Quality|Downloads|
|------|------|-------|---------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-omsagent.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-omsagent)|[![github](https://github.com/robertdebock/ansible-role-omsagent/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-omsagent/actions)|[![quality](https://img.shields.io/ansible/quality/43607)](https://galaxy.ansible.com/robertdebock/omsagent)|[![downloads](https://img.shields.io/ansible/role/d/43607)](https://galaxy.ansible.com/robertdebock/omsagent)|

## Example Playbook

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.omsagent
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.core_dependencies
    - role: robertdebock.users
      users_group_list:
        - name: omiusers
      users_user_list:
        - name: omsagent
          group: omiusers
    - role: robertdebock.auditd
      auditd_local_events: "no"
    - role: robertdebock.cron
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: check if connection still works
      ping:
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## Role Variables

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for omsagent
# Extra documentation on Log Analytic Agent is available on:
# https://docs.microsoft.com/en-us/azure/azure-monitor/platfrom/logs-analytics-agent

omsagent_version: 1.12.15-0

# omsagent_tmp directory is where the installer script is placed.
# The installer downloads a large file (125MB) to this directory.
omsagent_tmp: /tmp

# Set the user and group owning the directory.
omsagent_owner: omsagent
omsagent_group: omiusers

# Use workspace ID for automatic onboarding.
omsagent_id: []

# Use as the shared key for automatic onboarding.
omsagent_shared_key: []

# Use as the OMS domain for onboarding.
# For azure monitoring log Analytics workspace in Goverment cloud use:
# omsagent_domain: opinsights.azure.command
# leave emapy to use scripts default (omsagent_domain: opinsights.azure.com).
omsagent_domain: []

# Use [protocol://][user:password@]proxyhost[:port] as the proxy configuration.
# omsagent_proxy: "https://username:password@proxyserver:proxyport"
```

## Requirements

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.auditd
- robertdebock.bootstrap
- robertdebock.core_dependencies
- robertdebock.cron
- robertdebock.users

```

## Context

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/omsagent.png "Dependency")

## Compatibility

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|el|7|
|opensuse|all|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

## Exceptions

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| alpine | Only RPM and DPKG is supported by Microsoft. |
| amazonlinux | Dependency not available: ansible-role-auditd |
| archlinux | Only RPM and DPKG is supported by Microsoft. |
| centos:8 | Python is not configured or Python does not support ctypes on this system. |
| ubuntu | Python is not configured or Python does not support ctypes on this system, installation cannot continue. |
| debian | Python is not configured or Python does not support ctypes on this system, installation cannot continue. |

## Included version(s)

This role [refers to a version](https://github.com/robertdebock/ansible-role-omsagent/blob/master/defaults/main.yml) released by Microsoft. Check the released version(s) here:
- [OMS Agent for Linux GA](https://github.com/microsoft/OMS-Agent-for-Linux/releases).

This version reference means a role may get outdated. Monthly tests occur to see if [bit-rot](https://en.wikipedia.org/wiki/Software_rot) occured. If you however find a problem, please create an issue, I'll get on it as soon as possible.
## Testing

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-omsagent) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-omsagent/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## License

Apache-2.0


## Author Information

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
