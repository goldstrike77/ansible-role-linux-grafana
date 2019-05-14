![](https://img.shields.io/badge/Ansible-Grafana-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_grafana.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Grafana Versions](#Grafana-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
This Ansible role installs Grafana on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### Grafana versions

The following list of supported the Grafana releases:

* Grafana 5

## Role variables
### Main parameters
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `grafana_path`: Specify the Grafana data directory.
* `grafana_version`: Specify the Grafana version.
* `grafana_admin_user`: The name of the default Grafana admin user.
* `grafana_admin_password`: The password of the default Grafana admin.

##### Service Mesh
* `environments`: Define the service environment.
* `consul_public_register`: false Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

##### Listen port
* `grafana_port`: Grafana instance listen port.

##### NGinx parameters
* `grafana_ngx_dept`: A boolean value, whether proxy web interface using NGinx.
* `grafana_ngx_domain`: Defines domain name.
* `grafana_ngx_version`: # extras or standard
* `grafana_ngx_port_http`: NGinx HTTP listen port.
* `grafana_ngx_port_https`: NGinx HTTPs listen port.
* `grafana_ngx_backend`: Define groups of servers that can be referenced.
* `grafana_ngx_site`: Define backend traffic context.

##### Server System Variables
* `grafana_arg.default_theme`: Default UI theme.
* `reporting_enabled`: Sends usage counters to stats.grafana.org every 24 hours.
* `check_for_updates`: Enable or disable all checks to https://grafana.net for new vesions.

##### Datasource
* `grafana_ds_arg`: DataSource settings.

##### Plugins
* `grafana_plugins`: Grafana plugins list for installs.

##### Dashboard #
* `grafana_dashboard_arg`: DashBoard settings.

## Dependencies
- Ansible versions > 2.6 are supported.
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10' grafana_is_install='true'
    node02 ansible_host='192.168.1.11' grafana_is_install='true'
    node03 ansible_host='192.168.1.12' grafana_is_install='true'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-grafana
           grafana_is_install: true

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    grafana_path: '/data'
    grafana_version: '5'
    grafana_admin_user: 'admin'
    grafana_admin_password: 'password'
    grafana_port: '3000'
    grafana_ngx_dept: false
    grafana_ngx_domain: 'visual.example.com'
    grafana_ngx_version: 'standard'
    grafana_ngx_port_http: '80'
    grafana_ngx_port_https: '443'
    grafana_ngx_backend:
      - '127.0.0.1'
    grafana_ngx_site:
      - domain: 'visual.example.com'
        type: 'proxy'
        location: '/'
        syntax:
          - 'expires off'
          - 'proxy_set_header Host $http_host'
        backend_address: '127.0.0.1'
        backend_port: '3000'
        sticky: 'ip_hash'
        keepalive: '32'
    grafana_arg:
      default_theme: 'light'
      reporting_enabled: 'false'
      check_for_updates: 'false'
    grafana_ds_arg:
      - name: 'prometheus'
        settings:  
          - server: '127.0.0.1'
            port: '19090'
    grafana_plugins:
      - 'grafana-piechart-panel'
    grafana_dashboard_arg:
      - category: 'OperatingSystem'
        title:
          - 'Linux_Disk_Performance'
          - 'Linux_Disk_Space'
          - 'Linux_Network_Overview'
          - 'Linux_System_Overview'
          - 'Windows_System_Overview'
    environments: 'SIT'
    consul_public_register: false
    consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
    consul_public_clients: 'localhost'
    consul_public_http_port: '8500'

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
