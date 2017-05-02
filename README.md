uwsgi
=====

Ansible role which helps to install and configure
[uWSGI](http://uwsgi-docs.readthedocs.io/en/latest/index.html).

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```
- name: Default uWSGI installation
  hosts: all
  roles:
    - uwsgi

- name: Customized installation
  hosts: all
  vars:
    # Install Python plugin
    uwsgi_plugins:
      - python
    # Add application configuration
    uwsgi_config_uwsgi__custom:
      plugin: python
      socket: :8081
      chdir: /opt/RatticWeb-1.3.1
      wsgi-file: ratticweb/wsgi.py
  roles:
    - uwsgi
```


Role variables
--------------

```
# Whether to install EPEL YUM repo
uwsgi_epel_install: "{{ yumrepo_epel_install | default(true) }}"

# EPEL YUM repo URL
uwsgi_epel_yumrepo_url: "{{ yumrepo_epel_url | default('https://dl.fedoraproject.org/pub/epel/$releasever/$basearch/') }}"

# EPEL YUM repo GPG key
uwsgi_epel_yumrepo_gpgkey: "{{ yumrepo_epel_gpgkey | default('https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-$releasever') }}"

# Additional EPEL YUM repo params
uwsgi_epel_yumrepo_params: "{{ yumrepo_epel_params | default({}) }}"

# Package to be installed (explicit version can be specified here)
uwsgi_pkg: uwsgi

# List of plugins to install
uwsgi_plugins: []

# Name of the service
uwsgi_service: uwsgi


# Path to the main config file
uwsgi_config_path: /etc/uwsgi.ini

# Values of the default options of the uwsgi section of the main config
uwsgi_config_uwsgi_uid: uwsgi
uwsgi_config_uwsgi_gid: uwsgi
uwsgi_config_uwsgi_pidfile: /run/uwsgi/uwsgi.pid
uwsgi_config_uwsgi_emperor: /etc/uwsgi.d
uwsgi_config_uwsgi_stats: /run/uwsgi/stats.sock
uwsgi_config_uwsgi_chmod_socket: 660
uwsgi_config_uwsgi_emperor_tyrant: "true"
uwsgi_config_uwsgi_cap: setgid,setuid

# Default options of the uwsgi section of the main config
uwsgi_config_uwsgi__default:
  uid: "{{ uwsgi_config_uwsgi_uid }}"
  gid: "{{ uwsgi_config_uwsgi_gid }}"
  pidfile: "{{ uwsgi_config_uwsgi_pidfile }}"
  emperor: "{{ uwsgi_config_uwsgi_emperor }}"
  stats: "{{ uwsgi_config_uwsgi_stats }}"
  chmod-socket: "{{ uwsgi_config_uwsgi_chmod_socket }}"
  emperor-tyrant: "{{ uwsgi_config_uwsgi_emperor_tyrant }}"
  cap: "{{ uwsgi_config_uwsgi_cap }}"

# Custom options of the uwsgi section of the main config
uwsgi_config_uwsgi__custom: {}

# Final uwsgi section of the main config
uwsgi_config_uwsgi: "{{
  uwsgi_config_uwsgi__default.update(uwsgi_config_uwsgi__custom) }}{{
  uwsgi_config_uwsgi__default }}"

# Default main config
uwsgi_config__default:
  uwsgi: "{{ uwsgi_config_uwsgi }}"

# Custom main config
uwsgi_config__custom: {}

# Final main config
uwsgi_config: "{{
  uwsgi_config__default.update(uwsgi_config__custom) }}{{
  uwsgi_config__default }}"
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Authors
-------

Dennis Tait, Jiri Tyr
