# cron Setup

Role that setup the [cron](https://linux.die.net/man/8/cron) daemon using the security guidelines established by the [CIS Benchmark for RHEL 7](http://cisecurity.org) over the **5.1 Configure cron** section.

It also allows to manage a list of authorised users to run `cron` jobs.

Should be use to setup any `cron` jobs to be setup in the machines.

## Requirements

This role was created using [Ansible 2.9](https://docs.ansible.com/ansible/2.9/) for macOS and tested using the [centos/7](https://app.vagrantup.com/centos/boxes/7) boxes for [Vagrant v.2.2.6](https://www.vagrantup.com/docs/index.html) with [VirtualBox](https://www.virtualbox.org/) as a Provider.

The [Ansible modules](https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html) used in the role are:

- [include_tasks](https://docs.ansible.com/ansible/2.9/modules/include_tasks_module.html#include-tasks-module) - new in version 2.4, previously [include](https://docs.ansible.com/ansible/2.9/modules/include_module.html#include-module) was used;
- [service](https://docs.ansible.com/ansible/2.9/modules/service_module.html#service-module)
- [file](https://docs.ansible.com/ansible/2.9/modules/file_module.html#file-module)
- [template](https://docs.ansible.com/ansible/2.9/modules/template_module.html#template-module)
- [yum](https://docs.ansible.com/ansible/2.9/modules/yum_module.html#yum-module) - requires Python 2 and `yum` (for Python 3 see [dnf](https://docs.ansible.com/ansible/2.9/modules/dnf_module.html#dnf-module));
- [command](https://docs.ansible.com/ansible/2.9/modules/command_module.html#command-module) - keep this in mind when running check mode
- [cron](https://docs.ansible.com/ansible/2.9/modules/cron_module.html#cron-module)

> We are using the [loop](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html?highlight=loop) statement which was added in Ansible 2.5 and also [blocks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_blocks.html?highlight=block).

## Role Tasks

The role is organised, at the moment, in two tasks files:

- [setup](./tasks/setup.yml) - ensure all guidelines from CIS are enforced on the hosts and maintain a list of authorised users to run `cron`;
- [aide](./tasks/aide.yml) - ensure [AIDE](https://aide.github.io) is setup following the **1.3 Filesystem Integrity Checking** chapter of CIS and a `cron` job is scheduled.

> The goal is to append new tasks according to the `cron` jobs that needs to be in place.

## Role Variables

As for the moment no variable is defined. If a list of users should be provided to populate the `cron.allow` file it should be define as a list named `cron_users`, example:

```yaml
cron_users:
  - app_user01
  - app_user02
```

## Dependencies

This role doesn't have any dependencies.

## Example Playbook

A working example using Vagrant and Virtual Box is setup under [tests](./tests/).

