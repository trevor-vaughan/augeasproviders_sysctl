# sysctl: type/provider for shell files for Puppet

This module provides a new type/provider for Puppet to read and modify shell
config files using the Augeas configuration library.

The advantage of using Augeas over the default Puppet `parsedfile`
implementations is that Augeas will go to great lengths to preserve file
formatting and comments, while also failing safely when needed.

This provider will hide *all* of the Augeas commands etc., you don't need to
know anything about Augeas to make use of it.

## Requirements

Ensure both Augeas and ruby-augeas 0.3.0+ bindings are installed and working as
normal.

See [Puppet/Augeas pre-requisites](http://docs.puppetlabs.com/guides/augeas.html#pre-requisites).

## Installing

On Puppet 2.7.14+, the module can be installed easily ([documentation](http://docs.puppetlabs.com/puppet/latest/reference/modules_installing.html)):

    puppet module install herculesteam/augeasproviders_sysctl

You may see an error similar to this on Puppet 2.x ([#13858](http://projects.puppetlabs.com/issues/13858)):

    Error 400 on SERVER: Puppet::Parser::AST::Resource failed with error ArgumentError: Invalid resource type `sysctl` at ...

Ensure the module is present in your puppetmaster's own environment (it doesn't
have to use it) and that the master has pluginsync enabled.  Run the agent on
the puppetmaster to cause the custom types to be synced to its local libdir
(`puppet master --configprint libdir`) and then restart the puppetmaster so it
loads them.

## Compatibility

### Puppet versions

Minimum of Puppet 2.7.

### Augeas versions

Augeas Versions           | 0.10.0  | 1.0.0   | 1.1.0   | 1.2.0   |
:-------------------------|:-------:|:-------:|:-------:|:-------:|
**PROVIDERS**             |
sysctl                    | **yes** | **yes** | **yes** | **yes** |

## Documentation and examples

Type documentation can be generated with `puppet doc -r type` or viewed on the
[Puppet Forge page](http://forge.puppetlabs.com/herculesteam/augeasproviders_sysctl).


### manage simple entry

    sysctl { "net.ipv4.ip_forward":
      ensure => present,
      value  => "1",
    }

### manage entry with comment

    sysctl { "net.ipv4.ip_forward":
      ensure  => present,
      value   => "1",
      comment => "test",
    }

### delete entry

    sysctl { "kernel.sysrq":
      ensure => absent,
    }

### remove comment from entry

    sysctl { "kernel.sysrq":
      ensure  => present,
      comment => "",
    }

### manage entry in another sysctl.conf location

    sysctl { "net.ipv4.ip_forward":
      ensure => present,
      value  => "1",
      target => "/etc/sysctl.d/forwarding.conf",
    }

### do not update value with the `sysctl` command

    sysctl { "net.ipv4.ip_forward":
      ensure => present,
      value  => "1",
      apply  => false,
    }


## Issues

Please file any issues or suggestions [on GitHub](https://github.com/hercules-team/augeasproviders_sysctl/issues).
