1. Create a playbook that:
  - creates a logical volume call users01, 
  - mount volue volumes under /home/users
  - Executed using the tag `volumes`

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
  
3. Create a playbook to:
  - Add `donnager.example.com` to /etc/hosts set to the ip of the host
    - Executed with tag `updated webhost`
  - Install apache and ensure:
    - It is running
    - It will start on boot
    - Set host to `donnager.example.com`
    - Executed when you run tag `webserver`
    - Create a site: 
      - Added an index.html in /var/www/html with the follow text
         - Welcome to the Martian Marine Web Site
      - Password protect page with the following username and password
         - bobbie - cucUmb3rS4nwitch
   - Applied to hosts in group `web`
   - Password to protect info will be `3arthSucks`

4. Create playbook to:
   - Install postfix
   - Enable postfix on startup
   - Launch postfix
   - executed with tag `mail`
   - Deployed to all hosts
5. Create playbook
   - Install squid
   - Configure squid to block facebook.
   - executed with tag `filter`
   - Deployed to all hosts
6. Create playbook to:
   - Install vsftp
   - Enable anonymous
   - executed with tag `ftp`
   - Deployed to all hosts
9. Create playbook to:
   - Install samba
   - Enable for startup
   - Configure workgroup as `truman`
   - executed with tag `smb`
   - Deployed to hosts in group `uns`
10. Create playbook to:
   - Install NFS
   - Export /home/users
   - executed with tag `ftp`
   - Deployed to hosts in group `uns`   
11. Create playbook to:
   - Install mysql client and server.
   - executed with tag `database`
   - Deployed to hosts in group `uns`

12. Install lighttpd using the role located at https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz

13. Install CA certs from https://www.geotrust.com/resources/root-certificates/ into /etc/pki/ca-trust/source/anchors/ and run update-ca-trust.

14. Write  play for each of the following modules:
   - Ping
   - Setup
   - Yum
   - service
   - user
   - copy
   - file
   - git
   - lineinfile
   - htpasswd
   - script
   - debug
