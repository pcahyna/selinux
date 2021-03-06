# SELinux

## Expected functionality

Essentially provide mechanisms to manage local customizations:

* Set enforcing/permissive
* restorecon portions of filesystem tree
* Set/Get Booleans
* Set/Get file contexts
* Manage logins
* Manage ports

## Available modules in Ansible

[selinux](http://docs.ansible.com/ansible/selinux_module.html): Configures the
SELinux mode and policy.

[seboolean](http://docs.ansible.com/ansible/seboolean_module.html): Toggles SELinux booleans.

[sefcontext](http://docs.ansible.com/ansible/sefcontext_module.html): Manages
SELinux file context mapping definitions Similar to the `semanage fcontext`
command.

[seport](http://docs.ansible.com/ansible/seport_module.html): Manages SELinux
network port type definitions.

### Modules provided by this repository

selogin: Manages linux user to SELinux user mapping

## Usage

The general usage is demonstrated in [selinux-playbook.yml](selinux-playbook.yml) playbook.

### selinux role

This role can be configured using variables as it is described below.

```yaml
vars:
  [ see below ]
roles:
  - role: selinux
    become: true
```


#### set SELinux mode permanently and in running system

```yaml
selinux_policy: targeted
selinux_state: enforcing
selinux_change_running: 1
```

#### set SELinux booleans

```yaml
selinux_booleans:
  - { name: 'samba_enable_home_dirs', state: 'on' }
  - { name: 'ssh_sysadm_login', state: 'on', persistent: 'yes' }
```

#### Set SELinux file contexts

```yaml
selinux_fcontexts:
  - { target: '/tmp/test_dir(/.*)?', setype: 'user_home_dir_t', ftype: 'd' }
```

#### run restorecon on filesystem trees

```yaml
selinux_restore_dirs:
  - /tmp/test_dir
```

#### Set linux user to SELinux user mapping

```yaml
    selinux_logins:
      - { login: 'plautrba', seuser: 'staff_u', state: 'absent' }
      - { login: '__default__', seuser: 'staff_u', serange: 's0-s0:c0.c1023', state: 'present' }
```
