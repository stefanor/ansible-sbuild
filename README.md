# Set up an sbuild instance

This playbook configures a machine for building Debian packages with
sbuild. There is no support for automated building, just chroots for
manually triggered builds.

## Running on a pre-provisioned machine

Be aware that this cookbook will add block devices to an LVM volume
group. If you do not want this to happen, you'll need to hack it a bit.

Write an inventory file (if necessary) placing the desired hosts in the
`sbuild` group. e.g.:

```ini
[sbuild]
box.example.net ansible_ssh_user=admin
```

Run the `sbuild.yml` cookbook:

```console
$ ansible-playbook -i hosts sbuild.yml
```

See [Configuration](#Configuration) for further details.

## Spawning an EC2 instance, and deploying sbuild

You'll need AWS credentials, either exported to the environment, or in
your `~/.boto` config file.

We also need some specific details, such as the EC2 region, and desired
instance type, in `ec2-config.yml`. Copy the sample file, and fill it
in:

```console
$ cp ec2-config.yml.sample ec2-config.yml
$ sensible-editor ec2-config.yml
```

Then run the `spawn.yml` cookbook:

```console
$ ansible-playbook -i localhost spawn.yml
```

See [Configuration](#Configuration) for further details.

## Configuration

By default, chroots are created for Debian unstable (i386, amd64, and
arm64) and Ubuntu trusty (amd64). This can be customised by setting the
`chroots` variable. e.g.:

```console
$ ansible-playbook -i localhost spawn.yml -e '{"chroots": ["sid-amd64"]}'
```

# License

This software is released under the ISC license, see COPYING.
