1. Create a playbook that:
  - creates a logical volume call users01, 
  - mount volue volumes under /home/users
  - Executed using the tag `volumes`

Note that if you are working with system that does not have a block device to work with, you can create one with a disk image and a loopback:

```
fallocate -l 1G /opt/disk01.img
losetup /dev/loop1 /opt/disk01.img
```

2.  Create a playbook to create:
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
  
3. Create playbook to:
   - Install postfix
   - Enable postfix on startup
   - Launch postfix
   - executed with tag `mail`
   - Deployed to web group
4. Create playbook
   - Install squid
   - Configure squid to block facebook.
   - executed with tag `filter`
   - Deployed to all hosts
5. Create playbook to:
   - Install vsftp
   - Enable anonymous
   - executed with tag `ftp`
   - Deployed to all hosts
6. Create playbook to:
   - Install samba
   - Enable for startup
   - Configure workgroup as `truman`
   - executed with tag `smb`
   - Deployed to hosts in group `bastion`
