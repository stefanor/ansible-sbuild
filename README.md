# sbuild

This role configures a machine for building Debian packages with sbuild.
There is no support for automated building, just chroots for manually
triggered builds.

## Requirements

Spare block device(s) are needed, to host a LVM VG.
On an EC2 instance, the ephemeral storage is a good choice.

## Role Variables

### `sbuild_chroots`

By default, chroots are created for Debian unstable (i386, amd64, and
arm64).
This can be customised by setting the `sbuild_chroots` variable. e.g.

```yaml
---
sbuild_chroots:
  - sid-amd64
```

### `sbuild_lvm_block_devices`

By default, all block devices after `sda` will be formed into an LVM
Volume Group.
If you want to provide a list of block devices for LVM, set them in this
list, without the leading `/dev/`. e.g.

```yaml
sbuild_lvm_block_devices:
  - sda2
  - sdb2
```

## Dependencies

This role has no dependencies outside of the core ansible modules.

## Example Playbook

There are some example playbooks in the `playbooks` directory.

### HOWTO Use them:

1. `cp playbooks/ec2-config.yml.sample playbooks/ec2-config.yml`
1. Set all of the variables in `ec2-config.yml`:
   * `key_name`: EC2 keypair name.
   * `image`: AMI ID.
   * `ssh_user`: Username for the admin user (e.g. `admin` on a Debian image).
   * `region`: AWS Region.
   * `vpc_subnet_id`: VPC Subnet ID.
   * `group_id`: Security Group ID.
   * `instance_type`: EC2 Instance size.
   * `volumes`: Additional storage devices (instance storage / EBS)
1. Put some EC2 credentials in `~/.boto`
1. Set list of desired releases in `defaults/main.yml`.
1. If necessary, update `sbuild_lvm_block_devices` in
   `defaults/main.yml`.
1. `ansible-playbook -i ./playbooks/localhost -v playbooks/ec2-spawn.yml`
1. If it fails, you can create an inventory pointing at the machine (in
   the `sbuild` group and run
   `ansible-playbook -i my-inventory -v playbooks/sbuild.yml`

## License

This software is released under the ISC license, see [COPYING](COPYING).

## Author Information

By [Stefano Rivera](https://tumbleweed.org.za/).
