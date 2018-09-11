1. Create a playbook that performs the following on `bastion`:
  - create directory /home/users
  - create a 5 gig logical volume group called `vg01`
  - creates a 1 gig logical volume call `users01`, 
  - create a xfs file system on logical volume `users01`
  - mount volumes under /home/users and should be persistent

Note that if you are working with system that does not have a block device to work with, you can create one with a disk image and a loopback:

```
fallocate -l 1G /opt/disk01.img
losetup /dev/loop1 /opt/disk01.img
```

2.  Create a playbook on `web` to create:
  - Create in /home/sol
  - Usernames and passwords will be will be:
     - `alex` - `L4sagna`
     - `naomi` - `k1bBle`
     - `james` - `c0ff33`
     - `amos` - `h3nnEsy`
  - Only `naomi` and `james` should have shell access using bash
  - All the users will be in the group `crew`
  - The users will be created with tag `users`
  - Applied to hosts in group `bastion`
  - Password to protect the users will be `prot0mo|eculE`
  - tagged `web` 
3. Create playbook on all hosts to:
   - Install postfix
   - Enable postfix on startup
   - Launch postfix
   - tagged `mail`
4. Create playbook on `app` group to:
   - Install squid
   - Configure squid to block facebook.
   - tagged `filter`
5. Create playbook on `database` group to:
   - Install vsftp
   - Enable anonymous
   - tagged `ftp`
6. Create playbook on `database` group to:
   - Install samba
   - Enable for startup
   - Configure workgroup as `truman`
   - tagged `smb`
