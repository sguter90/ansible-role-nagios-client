Ansible Role: Nagios Client
=========

Install and configure Nagios Client (NRPE) on CentOS

Requirements
------------

None

Role Variables
--------------
#### Full Configuration Example:
```yaml
nrpe_client_allowed_hosts:
    - 127.0.0.1
nrpe_client_debug: 0
nrpe_client_dont_blame_nrpe: 1
nrpe_client_plugin_path: /usr/lib64/nagios/plugins
nrpe_client_pid_file: /var/run/nrpe/nrpe.pid
nrpe_client_server_port: 5666
nrpe_client_command_timeout: 60
nrpe_client_connection_timeout: 300
nrpe_client_rsyslog_facility_name: local1
nrpe_client_rsyslog_file: /var/log/nrpe.log
nrpe_local_plugin_dir: /tmp/ansible/files/plugins
nrpe_local_nrped_dir: /tmp/ansible/files/nrpe.d
nrpe_local_etc_dir: /tmp/ansible/files/etc
nrpe_client_commands:
  - name: check_uptime
    command: 'check_uptime.pl -c $ARG1$'
```

#### General Options

`nrpe_client_allowed_hosts`: list of hosts will be added to nrpe.cfg option "allowed_hosts" and /etc/hosts.allow

`nrpe_client_debug`: enable nrpe debug mode

`nrpe_client_dont_blame_nrpe`:  0=do not allow nrpe arguments, 1=allow nrpe command arguments

`nrpe_client_plugin_path`: path to check scripts on remote host

`nrpe_client_pid_file`: path to nrpe pid file

`nrpe_client_server_port`: nrpe port

`nrpe_client_command_timeout`: nrpe command timeout

`nrpe_client_connection_timeout`: nrpe connection timeout in seconds

`nrpe_client_rsyslog_facility_name`: name of nrpe rsyslog facility

`nrpe_client_rsyslog_file`: path to rsynslog facility log file

#### File Handling

`nrpe_local_plugin_dir`: local path to folder which contains all check scripts - will be copied to path defined in `nrpe_client_plugin_path`

`nrpe_local_nrped_dir`: local path to custom configuration files - will be copied to /etc/nrpe.d

`nrpe_local_etc_dir`: local path to additional configuration files - will be copied to /etc/nagios 

### Command Definitions
This role will generate a configuration file for all commands defined in `nrpe_client_commands` and place it in /etc/nrpe.d/commands.cfg

```yaml
nrpe_client_commands:
  - name: check_uptime
    command: 'check_uptime.pl -c $ARG1$'
  - name: check_volume_var
    command: 'check_volume.sh --volume /var -c $ARG1$ -w ARG2$'
```

This will generate this configuration file in /etc/nrpe.d/commands.cfg which will contain the following:
```
# Don't edit manually!
# Ansible managed

command[check_uptime]=/usr/lib64/nagios/pluginscheck_uptime.pl -c $ARG1$
command[check_volume_var]=/usr/lib64/nagios/pluginscheck_volume.sh --volume /var -c $ARG1$ -w ARG2$
```



Dependencies
------------

None.

Example Playbook
----------------

```yaml
# deploy-nagios-client.yml
- name: setup nagios client
  hosts: servers
  become: yes
  vars_files:
    - vars/nagios-client.yml

  roles:
    - { role: sguter90.ansible-role-nagios-client }
```

Full playbook example available at: [sguter90/ansible-playbook-nagios-client](https://github.com/sguter90/ansible-playbook-nagios-client)

License
-------

BSD

Author Information
------------------

This role was created in 2018 by Christoph Mueller
