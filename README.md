yum_plugin
==========

Ansible role which helps to configure YUM plugins.

The configuraton of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing the role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Example
-------

```
---

# Example of default installation
- hosts: myhost1
  roles:
    # By default only the fastestmirror plugin is configured
    - yum_plugin

# Example of how to customize the default configuration
- hosts: myhost2
  roles:
    - role: yum_plugin
      # Disable the fastestmirror plugin
      yum_plugin_config_fastestmirror_main_enabled: 0

# Example of how to extend the default configuration with a new plugin
- hosts: myhost2
  roles:
    - role: yum_plugin
      # Adding configuration for the downloadonly plugin
      yum_plugin_config__custom:
        # This is the name of the new plugin
        downloadonly:
          # This is the [main] section
          main:
            # Disable the plugin
            enabled: 0
```

This role requires [Config Encoder
Macros](https://github.com/picotrading/config-encoder-macros) which must be
placed into the same directory as the playbook:

```
$ ls -1 *.yaml
site.yaml
$ git clone https://github.com/picotrading/config-encoder-macros.git ./templates/encoder
```


Role variables
--------------

List of variables used by the role:

```
# Default parameters for the fastestmirror plugin
yum_plugin_fastestmirror_main_enabled: 1
yum_plugin_fastestmirror_main_verbose: 0
yum_plugin_fastestmirror_main_always_print_best_host: 'true'
yum_plugin_fastestmirror_main_socket_timeout: 3
yum_plugin_fastestmirror_main_hostfilepath: timedhosts.txt
yum_plugin_fastestmirror_main_maxhostfileage: 10
yum_plugin_fastestmirror_main_maxthreads: 15

# Default custom parameters for the fastestmirror plugin
yum_plugin_fastestmirror_main__custom: {}

# Default main section of the fastestmirror plugin
yum_plugin_fastestmirror_main:
  enabled: "{{ yum_plugin_fastestmirror_main_enabled }}"
  verbose: "{{ yum_plugin_fastestmirror_main_verbose }}"
  always_print_best_host: "{{ yum_plugin_fastestmirror_main_always_print_best_host }}"
  socket_timeout: "{{ yum_plugin_fastestmirror_main_socket_timeout }}"
  hostfilepath: "{{ yum_plugin_fastestmirror_main_hostfilepath }}"
  maxhostfileage: "{{ yum_plugin_fastestmirror_main_maxhostfileage }}"
  maxthreads: "{{ yum_plugin_fastestmirror_main_maxthreads }}"

# Default configuration of the fastestmirror plugin
yum_plugin_fastestmirror:
  fastestmirror:
    main: "{{
      yum_plugin_fastestmirror_main__custom.update(
      yum_plugin_fastestmirror_main)
    }}{{ yum_plugin_fastestmirror_main__custom }}"


# Configuration for custom plugins
yum_plugin__custom: {}


# Configuration for all plugins
yum_plugin_config: "{{
    yum_plugin__custom.update(
    yum_plugin_fastestmirror)
  }}{{ yum_plugin__custom }}"
```


Dependencies
------------

* [Config Encoder Macros](https://github.com/picotrading/config-encoder-macros)


License
-------

MIT


Author
------

Jiri Tyr
