postfix
=======

Configure `postfix` to send mail via a relay client.

Installs both `postfix` and `bsd-mailx`/`mailx` packages. Default postfix configuration in `/etc/postfix/main.cf` as below. Override values in `postfix_main_cf`.

```ini
append_dot_mydomain = no
biff = no
inet_interfaces = 127.0.0.1, [::1]
mydestination = ${myhostname} localhost.${mydomain} localhost
mydomain = '{{ ansible_domain }}'
myhostname = '{{ ansible_fqdn }}'
mynetworks_style = host
myorigin = ${myhostname}
relaydomain = ${mydomain}
relayhost = aspmx.l.google.com
smtp_enforce_tls = yes
smtp_sasl_auth_enable = no
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
tls_append_default_CA = yes
```

Version
-------

* `3.0.0` --- update to ansible 2.12.9
* `2.2.0` --- added Ubuntu Jammy(22.04) and removed centos8.
* `2.1.0` --- removed Ubuntu 14.04 from test. Added RHEL 8 and set inet_interfaces = all for that dist.
* `2.0.0` --- removed centos6 from test. added update cache for ubuntu. reordered tasks so newaliases are run before postfix start.
* `1.2.0` --- remove ubuntu precise from testing
* `1.1.0` --- updated with ubuntu focal, 20.04
* `1.0.4` --- tested with Ansible 2.9.11
* `1.0.3` --- prepare for github
* `1.0.2` --- check if alias file exits before adding alias
* `1.0.1` --- fixed replace of existing alias
* `1.0.0` --- initial version
* `master` --- latest development version

Requirements
------------

This role is limited to:

* Ubuntu 16.04
* Ubuntu 18.04
* Ubuntu 20.04
* Ubuntu 22.04
* CentOS 7
* RHEL 8

Role Variables
--------------

* `postfix_aliases` --- configure mail aliases in a dictionary - empty string removes alias, default `{ root: '' }`.
* `postfix_main_cf` --- configuration values for `/etc/postfix/main.cf` as dictionary, default `{}`.
* `postfix_automatic_smt_tls_cafile` --- add reference to OS ca-certificates/ca-bundle file in postfix configuration, default `true`.

Dependencies
------------
The RHEL8 image needs to be registered with RedHat to install packages.

Example Playbook
----------------

Variables are kept in the `host_vars` or `group_vars` folder usually. Defining everything in playbook is not recommended. This is just an example.

    - hosts: servers
      vars:
      roles:
      - role: postfix
        vars:
          postfix_main_cf:
            mydomain: foo.bar
          postfix_aliases:
            root: linux@foo.bar
            johndoe: ''

Testing
-------

To test RHEL8 with vagrant, install `vagrant-register`

```bash
vagrant plugin install vagrant-registration
```

Testing the role with Vagrant running on VirtualBox.

    cd tests
    vagrant up

Rerun tests.

    vagrant provision

Remove test VMs.

    vagrant destroy -f

License
-------

GPLv2

Author Information
------------------

Arnulf Heimsbakk <aheimsbakk@met.no>

###### set vim: spell spelllang=en:
