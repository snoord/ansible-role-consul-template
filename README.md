<p align="center">
  <a href="https://github.com/snoord/ansible-role-consul-template/actions">
    <img alt="GitHub Actions" src="https://github.com/snoord/ansible-role-consul-template/workflows/build/badge.svg?branch=master">
  </a>
  <a href="https://github.com/semantic-release/semantic-release">
    <img alt="semantic-release" src="https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg">
  </a>
</p>

<h1 align="center" style="border-bottom: none;">Ansible Role: consul-template</h1>

This role installs and configures `consul-template` on `systemd`-enabled Linux systems.

The overall aim of this role is to do one thing and do it well - which in this case is installing the `consul-template` binary and `systemd` service unit.

The following items are **not** what this role will do:
* Distribute or create `consul-template` templates
* Allow for heavy tweaking and customization of default options
* Do insanely complicated things with loops within loops within loops...

What really makes this role unique compared to others (the ones I have seen anyway) is that it configures `consul-template` as a `systemd` "instantiated service" (see [here](https://www.freedesktop.org/software/systemd/man/systemd.service.html) or `man 5 systemd.service` for details).

Configuring the `consul-template` service like this means that you can effortlessly launch multiple `consul-template` `systemd` services that all use different configuration files.

If you wanted to use a `consul-template` configuration file located at `/etc/consul-template.d/consul.hcl`, you could easily launch it through the following command:

```sh
$ sudo systemctl start consul-template@consul
```

Thanks to this instantiated service unit, you can achieve deeds most unholy.

You can have multiple `consul-template` instances templating entirely separate `consul-template` templates for any number of services which, theoretically, could be other instantiated `consul-templates` that then use the templated `consul-templates` templates for templating other `consul-template` templates.

Not that I'd recommend that sort of thing... :slightly_smiling_face:

## Requirements

For the Ansible controller:
* The `unzip` system package

For the target hosts/environment:
* Linux
* `systemd`
* A Consul cluster
* (Optional) A Vault cluster with the PKI secrets engine enabled

## Dependencies

If you do not already have a Consul cluster installed and configured, you can use my [Ansible role for Consul](https://github.com/snoord/ansible-role-consul) to create one.

## Example Playbook

```yaml
- hosts: example-hosts
  become: yes
  roles:
    - role: snoord.consul_template
```

## License

MIT / BSD

## Author Information

Created by Samuel Noordhuis in 2020. Inspired heavily by the Ansible roles and writings of [Jeff Geerling](https://github.com/geerlingguy).

If you see any errors or think this role could be improved in some way, you are welcome to open an issue/feature request or create a pull request!
