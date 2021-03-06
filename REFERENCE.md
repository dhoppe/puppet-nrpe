# Reference
<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

**Classes**

_Public Classes_

* [`nrpe`](#nrpe): Installs and configures NRPE

_Private Classes_

* `nrpe::config`: Configures NRPE
* `nrpe::config::ssl`: Configures SSL for NRPE
* `nrpe::install`: Installs required NRPE packages
* `nrpe::params`: Sets defaults based on OS
* `nrpe::service`: Manages the NRPE service

**Defined types**

* [`nrpe::command`](#nrpecommand): Installs NRPE commands
* [`nrpe::plugin`](#nrpeplugin): Installs additional plugins for NRPE

**Functions**

_Public Functions_


_Private Functions_

* `nrpe::ssl_logging`: A function that outputs a string suitable for the nrpe.conf `ssl_logging` parameter.  The nrpe.conf documentation has the following to say...

## Classes

### nrpe

Installs and configures NRPE

* **See also**
https://github.com/NagiosEnterprises/nrpe
https://github.com/NagiosEnterprises/nrpe/blob/master/README.SSL.md

#### Examples

##### Basic usage

```puppet
class { 'nrpe':
  allowed_hosts => [
    '127.0.0.1',
    'nagios.example.org',
  ],
}
```

##### With SSL

```puppet
class { 'nrpe':
  allowed_hosts               => 'nagios.example.org',
  ssl_cert_file_content       => file('profile/ssl/nagios.example.org.crt'),
  ssl_privatekey_file_content => file('profile/ssl/nagios.example.org.key'),
  ssl_cacert_file_content     => file('profile/ssl/GeoTrust_RSA_CA_2018.crt'),
  ssl_client_certs            => 'require',
}
```

#### Parameters

The following parameters are available in the `nrpe` class.

##### `allowed_hosts`

Data type: `Array[Stdlib::Host]`

Specifies the hosts that NRPE will accept connections from.

Default value: ['127.0.0.1']

##### `server_address`

Data type: `Stdlib::IP::Address`

Specifies the IP address of the inteface that NRPE should bind to. Useful when the system has more than one interface.

Default value: '0.0.0.0'

##### `commands`

Data type: `Hash`

A Hash of `nrpe::command` resources you want to create.  Recommended when you want to define `nrpe::command`s in hiera data.

Default value: {}

##### `plugins`

Data type: `Hash`

A Hash of `nrpe::plugin` resources you want to create.  Recommended when you want to define `nrpe::plugin`s in hiera data.

Default value: {}

##### `command_timeout`

Data type: `Integer[0]`

Specifies the maximum number of seconds that the NRPE daemon will allow plugins to finish executing before killing them off.

Default value: 60

##### `package_name`

Data type: `Variant[String[1], Array[String[1]]]`

The package name or array of package names that will be installed. The default is often fine, but you may wish to set this to install extra packages like `nrpe-selinux`.

Default value: $nrpe::params::nrpe_packages

##### `manage_package`

Data type: `Boolean`

By default, set to `true` and the `nrpe` class will manage the OS package(s).

Default value: `true`

##### `purge`

Data type: `Boolean`

When set to true, the module will purge any unmanaged commands from the NRPE includedir.

Default value: `false`

##### `dont_blame_nrpe`

Data type: `Boolean`

Determines whether or not the NRPE daemon will allow clients to specify arguments to commands that are executed. ENABLING THIS OPTION IS A SECURITY RISK!

Default value: $nrpe::params::dont_blame_nrpe

##### `log_facility`

Data type: `Nrpe::Syslogfacility`

The syslog facility that should be used for logging purposes.

Default value: $nrpe::params::log_facility

##### `server_port`

Data type: `Stdlib::Port::Unprivileged`

The port that NRPE should listen for connections on.

Default value: $nrpe::params::server_port

##### `command_prefix`

Data type: `Optional[Stdlib::Absolutepath]`

This option allows you to prefix all commands with a user-defined string.  Although often used to run all commands with sudo, `nrpe::command` has dedicated `sudo` parameters for this.

Default value: $nrpe::params::command_prefix

##### `debug`

Data type: `Boolean`

This option determines whether or not debugging messages are logged to the syslog facility.

Default value: $nrpe::params::debug

##### `connection_timeout`

Data type: `Integer[0]`

Specifies the maximum number of seconds that the NRPE daemon will wait for a connection to be established before exiting.

Default value: $nrpe::params::connection_timeout

##### `allow_bash_command_substitution`

Data type: `Optional[Boolean]`

Determines whether or not the NRPE daemon will allow clients to specify arguments that contain bash command substitutions of the form `$(...)`. ** ENABLING THIS OPTION IS A HIGH SECURITY RISK! **

Default value: $nrpe::params::allow_bash_command_substitution

##### `nrpe_user`

Data type: `String[1]`

Determines the effective user that the NRPE daemon should run as.

Default value: $nrpe::params::nrpe_user

##### `nrpe_group`

Data type: `String[1]`

Determines the effective group that the NRPE daemon should run as.

Default value: $nrpe::params::nrpe_group

##### `nrpe_pid_file`

Data type: `Stdlib::Absolutepath`

The name of the file in which the NRPE daemon should write it's process ID number.

Default value: $nrpe::params::nrpe_pid_file

##### `command_file_default_mode`

Data type: `Stdlib::Filemode`

The default file mode to use when creating NRPE command files in the includedir.

Default value: '0644'

##### `supplementary_groups`

Data type: `Array[String[1]]`

If set, the `nrpe_user` will be added to these supplementary groups.

Default value: []

##### `nrpe_ssl_dir`

Data type: `Stdlib::Absolutepath`

The directory that SSL certificates and keys will be created in.

Default value: $nrpe::params::nrpe_ssl_dir

##### `ssl_cert_file_content`

Data type: `Optional[String[1]]`

A string containing the SSL Certificate.

Default value: `undef`

##### `ssl_privatekey_file_content`

Data type: `Optional[String[1]]`

A string containing the SSL private **KEY**.  It is recommended to source this parameter from hiera and use EYAML or similar to encrypt the data.

Default value: `undef`

##### `ssl_cacert_file_content`

Data type: `Optional[String[1]]`

A string containing the SSL CA Cert file contents.

Default value: `undef`

##### `ssl_version`

Data type: `Nrpe::Sslversion`

The SSL Version to use.  The default of `TLSv1.2+` is the most secure option available at time of writing.  Avoid having to set it to a lower value if possible.

Default value: $nrpe::params::ssl_version

##### `ssl_ciphers`

Data type: `Array[String[1]]`

An array of ciphers that should be allowed by NRPE.  The defaults are for RSA keys and were taken from https://github.com/ssllabs/research/wiki/SSL-and-TLS-Deployment-Best-Practices.

Default value: $nrpe::params::ssl_ciphers

##### `ssl_client_certs`

Data type: `Enum['no','ask','require']`

This options determines client certificate usage.

Default value: $nrpe::params::ssl_client_certs

##### `ssl_log_startup_params`

Data type: `Boolean`

Whether to log startup SSL/TLS parameters.

Default value: `false`

##### `ssl_log_remote_ip`

Data type: `Boolean`

Whether to log remote IP address of SSL client.

Default value: `false`

##### `ssl_log_protocol_version`

Data type: `Boolean`

Whether to log SSL/TLS version of connections.

Default value: `false`

##### `ssl_log_cipher`

Data type: `Boolean`

Whether to log which encryption cipher is being used for SSL connections.

Default value: `false`

##### `ssl_log_client_cert`

Data type: `Boolean`

Whether to log if an SSL client has presented a certificate.

Default value: `false`

##### `ssl_log_client_cert_details`

Data type: `Boolean`

Whether to log details of client SSL certificates.

Default value: `false`

##### `config`

Data type: `Stdlib::Absolutepath`

**Private** You should not need to override this parameter.

Default value: $nrpe::params::nrpe_config

##### `include_dir`

Data type: `Stdlib::Absolutepath`

**Private** You should not need to override this parameter.

Default value: $nrpe::params::nrpe_include_dir

##### `provider`

Data type: `Optional[String[1]]`

**Private** You should not need to override this parameter.

Default value: $nrpe::params::nrpe_provider

##### `service_name`

Data type: `String[1]`

**Private** You should not need to override this parameter.

Default value: $nrpe::params::nrpe_service

## Defined types

### nrpe::command

Installs NRPE commands

#### Examples

##### Install a command called `check_users`

```puppet
nrpe::command { 'check_users':
  ensure  => present,
  command => 'check_users -w 5 -c 10',
}
```

#### Parameters

The following parameters are available in the `nrpe::command` defined type.

##### `name`

The name of the command.

##### `command`

Data type: `String[1]`

The command plugin to run and its arguments.

##### `ensure`

Data type: `Enum['present', 'absent']`

Whether to install or remove the command.

Default value: present

##### `file_mode`

Data type: `Optional[Stdlib::Filemode]`

The mode to use for the command file.  By default, this parameter is `undef`, and the command file will use `$nrpe::command_file_default_mode`.

Default value: `undef`

##### `sudo`

Data type: `Boolean`

Whether the command should use sudo.

Default value: `false`

##### `sudo_user`

Data type: `String[1]`

The user to run the command as when using sudo.

Default value: 'root'

### nrpe::plugin

Installs additional plugins for NRPE

#### Examples

##### Install a `check_mem` plugin from a file hosted in your `site` module

```puppet
nrpe::plugin { 'check_mem':
  ensure => present,
  source => 'puppet:///modules/site/nrpe/check_mem',
}
```

#### Parameters

The following parameters are available in the `nrpe::plugin` defined type.

##### `name`

The name of the plugin.

##### `ensure`

Data type: `Enum['present', 'absent']`

Whether to install or remove the plugin.

Default value: present

##### `content`

Data type: `Optional[String[1]]`

Defines the actual content of the plugin file.  Should not be used in conjunction with `source`.

Default value: `undef`

##### `source`

Data type: `Optional[Stdlib::Filesource]`

Defines the source of the plugin file. Should not be used in conjunction with `content`.

Default value: `undef`

## Functions

