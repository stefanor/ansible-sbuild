# Set up an sbuild instance

Some simple playbooks wrapping the `sbuild` role.

## Running on a pre-provisioned machine

Be aware that this playbook will add block devices to an LVM volume
group. If you do not want this to happen, you'll need to apply further
configuration.

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

See the role's documentation for configuration details.

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

Then run the `ec2-spawn.yml` cookbook:

```console
$ ansible-playbook -i localhost ec2-spawn.yml
```

See the role's documentation for configuration details.
