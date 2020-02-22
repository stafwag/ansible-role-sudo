# Ansible Role: sudo

An ansible role to manage sudo

## Requirements

sudo

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
