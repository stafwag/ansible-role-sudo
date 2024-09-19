# Ansible Role: sudo

An ansible role to manage sudo

## Requirements

sudo

### Supported platforms

* Archlinux
* Debian
* FreeBSD
* NetBSD
* OpenBSD
* RedHat
* Suse

## Installation

### Ansible galaxy

The role is available on [Ansible Galaxy](https://galaxy.ansible.com/ui/standalone/roles/stafwag/package_update/).

To install the role from Ansible Galaxy execute the command below. 

```bash
ansible-galaxy install stafwag.sudo
```
### Source Code

If you want to use the source code directly.

Clone the role source code.

```bash
$ git clone https://github.com/stafwag/ansible-role-sudo
```

and put into the [role search path](https://docs.ansible.com/ansible/2.4/playbooks_reuse_roles.html#role-search-path)


## Role Variables

### OS related variables

The following variables are set by the role.

* **sudo_packages**: Required sudo packages for the operating system. sudo by default.
* **sudo_sudoers**: Path to sudoers on the operating. /etc/sudoers by default.
* **sudo_sudoers_d**: Path to sudoers.d. /etc/sudoers.d by default.
* **sudo_root_group**: sudo root group. wheel by default, sudo on debian systems.
* **sudo_visudo**: path to visudo on the operating system.

### Playbook related variables

* **sudo**:
  "namespace"
  * **sudoers_linesinfile**: Array of lineinfile.
    * **regexp**: The regular expression to look for in every line of the file.
    * **line**: The line to insert/replace into the file.
    * **state**: absent | present (default)
    * **backup**: no (default) | yes Create a backup
  * **sudoers_d_files**: array of the user files to manage.
    * **path**: path in ***sudoers_d**** .
    * **content**: file content
    * **state**: absent | present (default)
    * **backup**: no (default) | yes. create a backup file.

## Role templates

* **sudo_root_group_with_password**: template to allow the ***sudo_root_group*** to execute commands as root with password authentication.

  template content: ```%{{ sudo_root_group }} ALL=(ALL) ALL```
* **sudo_root_group_with_no_password**: template to allow the ***sudo_root_group*** to execute commands as root with password authentication.

  template content: ```%{{ sudo_root_group }} ALL=(ALL) NOPASSWD: ALL```


## Dependencies

None

## Example Playbooks

### Setup sudo

```
---
- name: setup sudo
  hosts: all
  become: true
  vars:
    sudo:
      sudoers_linesinfile:
        - name: remove targetpw
          regexp: "^Defaults targetpw"
          state: absent
        - name: don't be nice
          regexp: '^Defaults insults'
          line: 'Defaults insults'
          state: present
      sudoers_d_files:
        - name: sudo group with password
          path: '{{ sudo_root_group }}'
          state: present
          content: "{{ lookup('template','sudo_root_group_with_password.j2') }}"
  roles:
    - stafwag.sudo
```

## License

MIT/BSD

## Author Information

Created by Staf Wagemakers, email: staf@wagemakers.be, website: http://www.wagemakers.be.
