# domain_membership

Manage Active Directory domain membership with this module.

## Parameters

* ```domain```       - AD domain which the node should be a member of.
* ```username```     - User with ability to join machines to a Domain.
* ```password```     - Password for domain joining user.
* ```machine_ou```   - [Optional] OU in the directory for the machine account to be created in.
* ```resetpw```      - [Optional] Whether or not to force machine password reset if it becomes out of sync with the domain.
* ```reboot```       - [Optional] Whether or not to reboot when the machine joins the domain. (reboot by default)
* ```reboot_apply``` - [Optional] If `reboot` is true, this controls the `apply` parameter. (defaults to 'finished')
* ```join_options``` - [Optional] A bit field for options to use when joining the domain. See <http://msdn.microsoft.com/en-us/library/aa392154(v=vs.85).aspx> Defaults to '1' (default domain join).
* ```user_domain```  - [Optional] Domain of user account used to join machine, if different from domain machine will be joined to.  If not specified, the value passed to the `domain` parameter will be used.

## Usage

At the core, this module is invoking powershell to grab a `Win32_ComputerSystem`
WMI object to call `JoinDomainOrWorkgroup` for the domain joining action. This
method of domain joining is
[fully documented here.](https://msdn.microsoft.com/en-us/library/aa392154(v=vs.85).aspx)

To use the `domain_membership` class, you will need to pass three required
parameters. The required parameters are `domain`, `username`, and `password`.
See the parameter descriptions above for more information.

The method of domain joining is dictated by the `join_option` parameter. This
parameter is expecting a bit mask to indicate the various options that will be
used. The available options are described as part of the documentation linked
above. The value is passed as an integer. By default, this module is using an
integer value of '1' for the option. This translets simply to only using the
"JOIN_DOMAIN" option which does not imply the creation of the machine account.
To have at least a machine account created as part of the join, option '3'
should be used. Overall, one should consult the MSDN document and determine the
best combination of settings for their objective.

## Example

```ruby
class { 'domain_membership':
  domain       => 'puppet.example',
  username     => 'joinmember',
  password     => 'sUp3r_s3cR3t!',
  join_options => '3',
}
```

## Contact

If you have questions or concerns about this module, email me at tom@puppetlabs.com

## Support

Please log tickets and issues at our [Projects site](http://www.github.com/trlinkin/puppet-domain_membership)
